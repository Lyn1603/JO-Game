<template>
  <div>
    <ScoreSystem :score="score" />
    <div class="app-container">
    </div>
  </div>
</template>

<script>
import Matter from "matter-js";
import ScoreSystem from "./ScoreSystem.vue";

export default {
  name: "GravitySimulator",
  components: {
    ScoreSystem,
  },
  data() {
    return {
      score: 0,
      engine: null,
      matterObjects: [],
      spriteImages: [
        require('@/assets/argent.png'),
        require('@/assets/or.png'),
        require('@/assets/bronze.png')
      ],
    };
  },
  mounted() {
    this.initMatter();
  },
  methods: {
    initMatter() {
      const engine = Matter.Engine.create({
        gravity: { y: 1 },
      });
      this.engine = engine;

      const ground = Matter.Bodies.rectangle(400, 580, 810, 60, {
        isStatic: true,
        label: 'ground',
        render : {fillStyle: "red"}
      });
      Matter.World.add(engine.world, [ground]);

      const runner = Matter.Runner.create();
      Matter.Runner.run(runner, engine);

      /*setInterval(() => {
        this.addRandomBodies(engine);
      }, 2000);*/

      this.addRandomBodies(engine);

      Matter.Events.on(engine, "collisionStart", this.handleCollision);
    },

    addRandomBodies(engine) {
      const randomX = Math.random() * 800;
      /*const randomSize = Math.random() * 40 + 10;*/
      const randomSize = 10;
      const randomImage = this.spriteImages[
          Math.floor(Math.random() * this.spriteImages.length)
          ];

      let newBody;
      newBody = Matter.Bodies.circle(randomX, 0, randomSize , {
        restitution: 0.8,
        render: {fillStyle : 'red'},
        label: 'circle',
      });
      /*if (Math.random() > 0.5) {
        newBody = Matter.Bodies.circle(randomX, 0, randomSize , {
          restitution: 0.8,
          //render: { sprite: { texture: randomImage }, wireframe : true },
          render: 'red',
          label: 'circle',
        });
      } else {
        newBody = Matter.Bodies.rectangle(randomX, 0, randomSize, randomSize, {
          restitution: 0.8,
          render: { sprite: { texture: randomImage } },
          label: 'rectangle',
        });
      }*/

      Matter.World.add(engine.world, [newBody]);

      this.matterObjects.push({
        body: newBody,
        size: randomSize,
        type: newBody.label,
        image: randomImage,
      });

    },

    getObjectStyle(object) {
      return {
        width: object.size + "px",
        height: object.size + "px",
        transform: `translate(${object.x}px, ${object.y}px) rotate(${object.angle}rad)`,
        backgroundImage: `url(${object.image})`,
        backgroundSize: "contain",
        position: "absolute",
      };
    },

    handleCollision(event) {
      const pairs = event.pairs;
      pairs.forEach((pair) => {
        const ground = this.engine.world.bodies.find(
            (body) => body.label === "ground"
        );
        if (pair.bodyA === ground || pair.bodyB === ground) {
          this.score += 10;
        }
      });
    },
  },
};
</script>

<style scoped>
.objects-container {
  position: relative;
  width: 800px;
  height: 600px;
  background-color: #eef;
  overflow: hidden;
}

.object {
  position: absolute;
  background-size: contain;
  background-repeat: no-repeat;
}
</style>
