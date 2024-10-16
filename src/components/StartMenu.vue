<template>
  <div class="start-menu-container">
    <div class="start-menu-grid">
      <!-- Colonne gauche avec du texte explicatif -->
      <div class="left-column">
        <h2>Bienvenue dans JOCATCH !</h2>
        <p>
          Utilisez les flèches pour déplacer la plateforme et attraper des bonus.
          Faites attention aux malus qui réduisent votre score.
        </p>
      </div>

      <!-- Colonne centrale avec le bouton "Start" -->
      <div class="center-column">
        <button @click="startGame">GO</button>
      </div>

      <!-- Colonne droite avec le composant ItemComponent en grille -->
      <div class="right-column">
        <ItemComponent /> <!-- Intégration du composant ItemComponent ici -->
      </div>
    </div>
  </div>
</template>

<script>
import ItemComponent from "@/components/IteamComponent.vue";

export default {
  name: "StartMenu",
  components: {
    ItemComponent,
  },
  methods: {
    startGame() {
      this.$emit("startGame"); // Émettre un événement pour démarrer le jeu
    },
  },
};
</script>

<style scoped>
.start-menu-container {
  position: relative;
  z-index: 3;
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(0, 0, 0, 0.5); /* Couche de fond sombre translucide */
  backdrop-filter: blur(10px); /* Effet de flou sur l'arrière-plan */
  font-family: Rubik, sans-serif;
}

.start-menu-grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr; /* 3 colonnes */
  padding: 20px;
  background-color: rgba(255, 255, 255, 0.39); /* Couleur de fond légèrement translucide */
  border-radius: 10px;
  backdrop-filter: none; /* Aucun flou sur la grille de contenu */
}

.left-column {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 20px;
}

.left-column h2 {
  margin-bottom: 10px;
}

.center-column {
  display: flex;
  justify-content: center;
  align-items: center;
}

button {
  padding: 20px 40px;
  font-size: 24px;
  color: white;
  background-color: #2c3e50;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #218838; /* Couleur du bouton au survol */
}

.right-column {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  font-size: small;
  gap: 20px;
  padding: 20px;
  justify-items: center;
}

@media (max-width: 768px) {
  .start-menu-grid {
    grid-template-columns: 1fr; /* Passe à une seule colonne sur petit écran */
  }

  .right-column {
    grid-template-columns: 1fr; /* Passe à une seule colonne sur petit écran */
  }
}
</style>
