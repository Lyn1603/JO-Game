<template>
  <div class="popup">
    <div class="popup-content">
      <h2>Temps écoulé !</h2>
      <p>Votre score final est de : {{ score }}</p>

      <input type="text" v-model="name">
      <button @click="registerScore">sauvegarder mon score</button>
      <button @click="restartGame">Recommencer</button>
      <button @click="goToHome">Retour à la page d'accueil</button>
      <score-board/>
    </div>
  </div>
</template>

<script>
import ScoreBoard from "@/components/ScoreBoard.vue";

export default {
  components: {ScoreBoard},
  props: {
    score: {
      type: Number,
      default: 0
    }
  },
  methods: {
    restartGame() {
      this.$emit('restart');
    },
    registerScore() {
      const scores = JSON.parse(localStorage.getItem('scores')) || [];
      scores.push({ name: this.name, score: this.score });
      localStorage.setItem('scores', JSON.stringify(scores));
      //on fait la conditon de validation de la sauvegarde du score
    },
    goToHome() {
      this.$emit('goToHome'); // Émettre un événement pour retourner à la HomeView
    }

  },
  data() {
    return {
      name: ''
    };
  }

};
</script>

<style scoped>
.popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1000;
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup-content {
  width: fit-content;
  height: fit-content;
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

button {
  background-color: #2c3e50;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  background-color: #1A78FF;
}
</style>
