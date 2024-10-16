<template>
  <div class="container">
    <!-- Menu de démarrage -->
    <StartMenu v-if="!gameStarted && !isTimeUp" @startGame="startGame" />
    <TimerSystem
        v-if="gameStarted && !isTimeUp"
        :duration="timerDuration"
        @time-up="endSimulation"
        @timer-tick="adjustGravity"
    />
    <div v-if="isTimeUp" class="overlay">
      <TimesUp :score="score" @restart="restartGame" @goToHome="navigateToHome" />
    </div>
    <ScoreSystem v-if="gameStarted" :score="score" />
    <div ref="app"></div>
  </div>
</template>

<script>
import Matter from "matter-js";
import ScoreSystem from "./ScoreSystem.vue";
import TimerSystem from "./TimerSystem.vue";
import TimesUp from "@/components/TimesUp.vue";
import StartMenu from "@/components/StartMenu.vue"; // Import du menu de démarrage
import bonusImage from '@/assets/bonus.png';
import malusImage from '@/assets/malus.png';
import basket from '@/assets/panier.png';

export default {
  name: "GravitySimulator",
  components: {
    ScoreSystem,
    StartMenu,
    TimerSystem,
    TimesUp
  },
  data() {
    return {
      score: 0,
      engine: null,
      ground: null,
      matterObjects: [],
      groundSpeed: 15,
      sprite_size: 64,
      isTimeUp: false,
      malusActive: false,
      malusDuration: 15,
      malusTimer: null,
      timerDuration: 120,
      timerInterval: null,
      ObjectInterval: null,
      gameStarted: false, // État pour savoir si le jeu a commencé
      medals: [
        {
          type: "argent",
          image: require("@/assets/argent.png"),
        },
        {
          type: "or",
          image: require("@/assets/or.png"),
        },
        {
          type: "bronze",
          image: require("@/assets/bronze.png"),
        }
      ]
    };
  },
  mounted() {
    this.initMatter();
    this.addKeyboardControls();
    this.addTouchControls();
  },
  methods: {
    startGame() {
      this.gameStarted = true; // Met à jour l'état lorsque le jeu commence
      this.startTimer(); // Démarre le timer lorsque le jeu commence
      this.ObjectInterval = setInterval(() => {
        this.addRandomBodies(this.engine);
      }, 2000); // Ajoute des corps aléatoires lorsque le jeu commence
    },
    navigateToHome() {
      // Logique pour naviguer vers la HomeView, par exemple :
      this.$router.push({ name: 'home' }); // Assure-toi que 'HomeView' est le nom correct
    },

    initMatter() {
      const engine = Matter.Engine.create({
        gravity: {y: 0.5},
      });
      this.engine = engine;

      this.ground = Matter.Bodies.rectangle(400, 580, 240, 200, {
        isStatic: true,
        label: "ground",
        render: {
          sprite: {
            texture: basket,
            xScale: 0.5,
            yScale: 0.5,
          }
        },
      });
      Matter.World.add(engine.world, [this.ground]);

      const render = Matter.Render.create({
        element: this.$refs.app,
        engine: engine,
        options: {
          width: window.innerWidth,
          height: window.innerHeight,
          background: "none",
          wireframes: false,
        },
      });

      Matter.Render.run(render);
      const runner = Matter.Runner.create();
      Matter.Runner.run(runner, engine);

      Matter.Events.on(engine, "collisionStart", this.handleCollision);
    },

    addRandomBodies(engine) {
      const randomX = Math.random() * window.innerWidth;
      const randomSize = 20 + Math.random() * 30;
      let newBody;
      let type;

      const randomType = Math.random();
      if (randomType < 0.2) {
        newBody = Matter.Bodies.circle(randomX, 0 - randomSize, randomSize, {
          render: {
            sprite: {
              texture: bonusImage,
              xScale: 0.5,
              yScale: 0.5,
            }
          },
          label: "bonus", //malus
        });
      } else if (randomType < 0.4) {
        newBody = Matter.Bodies.circle(randomX, 0 - randomSize, randomSize, {
          render: {
            sprite: {
              texture: malusImage, // Use the imported bonus image
              xScale: 0.5, // Adjust size
              yScale: 0.5, // Adjust size
            }
          },
          label: "malus", //malus
        });
      } else {
        const randomMedail = this.medals[Math.floor(Math.random() * this.medals.length)];
        newBody = Matter.Bodies.circle(randomX, 0 - randomSize, randomSize, {
          render: {
            sprite: {
              texture: randomMedail.image,
              xScale: 0.2, // Reduce medal size
              yScale: 0.2, // Reduce medal size
            },
          },
          label: randomMedail.type,
        });
      }

      Matter.World.add(engine.world, [newBody]);

      this.matterObjects.push({
        body: newBody,
        size: randomSize,
        type: type,
      });
    },

    startTimer() {
      this.timerInterval = setInterval(() => {
        if (this.timerDuration > 0) {
          this.timerDuration--;
        } else {
          this.endSimulation();  // Appeler endSimulation directement
        }
      }, 1000);
    },

    endSimulation() {
      if (this.engine && this.engine.runner) {
        Matter.Runner.stop(this.engine.runner);
      }

      if (this.engine && this.engine.world) {
        this.engine.world.gravity.y = 0;
      }
      clearInterval(this.ObjectInterval);
      this.isTimeUp = true;
    },

    restartGame() {
      window.location.reload();
    },

    handleCollision(event) {
      const pairs = event.pairs;
      pairs.forEach((pair) => {
        const ground = this.engine.world.bodies.find(
            (body) => body.label === "ground"
        );
        if (pair.bodyA === ground || pair.bodyB === ground) {
          const object = pair.bodyA === ground ? pair.bodyB : pair.bodyA;
          Matter.World.remove(this.engine.world, object);

          switch (object.label) {
            case "bonus":
              this.score += 50;
              this.timerDuration += 10;
              break;
            case "malus":
              this.timerDuration -= 10;
              break;
            case "normal":
              this.score += 10;
              break;
            case "or":
              this.score += 50;
              break;
            case "argent":
              this.score += 20;
              break;
            case "bronze":
              this.score += 10;
              break;
          }
        }
      });
    },

    moveGround(direction) {
      if (!this.ground) return;

      const displacement = direction === "left" ? -this.groundSpeed : this.groundSpeed;

      const groundPosition = this.ground.position;

      const screenWidth = window.innerWidth;
      const groundWidth = this.ground.bounds.max.x - this.ground.bounds.min.x;

      const newXPosition = groundPosition.x + displacement;

      const leftEdge = newXPosition - groundWidth / 2;
      const rightEdge = newXPosition + groundWidth / 2;

      if (leftEdge >= 0 && rightEdge <= screenWidth) {
        Matter.Body.translate(this.ground, {x: displacement, y: 0});
      }
    },

    addKeyboardControls() {
      this.keysPressed = {};
      this.movementInterval = null;

      window.addEventListener("keydown", (event) => {
        if (event.key === "ArrowLeft" || event.key === "ArrowRight") {
          if (!this.keysPressed[event.key]) {
            this.keysPressed[event.key] = true;
            this.startMovement();
          }
        }
      });

      window.addEventListener("keyup", (event) => {
        if (event.key === "ArrowLeft" || event.key === "ArrowRight") {
          this.keysPressed[event.key] = false;
          if (!this.keysPressed["ArrowLeft"] && !this.keysPressed["ArrowRight"]) {
            this.stopMovement();
          }
        }
      });
    },

    startMovement() {
      if (this.movementInterval === null) {
        const move = () => {
          if (this.keysPressed["ArrowLeft"]) {
            this.moveGround("left");
          }
          if (this.keysPressed["ArrowRight"]) {
            this.moveGround("right");
          }
          this.movementInterval = requestAnimationFrame(move);
        };
        this.movementInterval = requestAnimationFrame(move);
      }
    },

    stopMovement() {
      if (this.movementInterval !== null) {
        cancelAnimationFrame(this.movementInterval);
        this.movementInterval = null;
      }
    },

    addTouchControls() {
      let touchStartX = 0;

      window.addEventListener("touchstart", (event) => {
        touchStartX = event.touches[0].clientX;
      });

      window.addEventListener("touchmove", (event) => {
        const touchCurrentX = event.touches[0].clientX;

        if (touchCurrentX < touchStartX) {
          this.moveGround("left");
        } else if (touchCurrentX > touchStartX) {
          this.moveGround("right");
        }

        touchStartX = touchCurrentX;
      });
    },

    adjustGravity(remainingTime) {
      const initialGravity = 0.5;
      const minGravity = 1;

      const timeRatio = remainingTime / this.timerDuration;

      const newGravity = minGravity + timeRatio * (initialGravity - minGravity);

      this.engine.world.gravity.y = newGravity;
    },
  },
};
</script>

<style scoped>
.container {
  overflow: hidden;
  height: 100vh;
  background: url("../assets/bg.png");
  background-size: cover;
}
</style>
