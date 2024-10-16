<script>
export default {
  name: 'JoComponent',
  props: {
    phryge: {
      type: String,
      required: true
    },
    sport: {
      type: String,
      required: true
    },
    date: {
      type: String,
      required: true
    },
    video: {
      type: String,
      required: true
    }
  },
  computed: {
    // Propriété calculée pour construire l'URL de la vidéo YouTube
    videoUrl() {
      // Utilisation du format embed pour l'URL YouTube
      return `https://www.youtube.com/embed/${this.video}`;
    }
  }
}
</script>

<template>
  <div class="souvenir">
    <!-- Image Phryge -->
    <img class="point-phryge" :src="phryge" alt="Phryge image">

    <!-- Titres (sport et date) -->
    <div class="titles">
      <h3>{{ sport }}</h3>
      <h4>{{ date }}</h4>
    </div>

    <!-- Vidéo YouTube -->
    <div class="video-wrapper">
      <iframe
          :src="videoUrl"
          title="YouTube video player"
          frameborder="0"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
      ></iframe>
    </div>
  </div>
</template>

<style scoped>
.souvenir {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
  width: 100%;
  max-width: 800px;
}

.point-phryge {
  width: 200px;
  height: 200px;
  object-fit: cover;
  border-radius: 50%;
  background: white;
}

.titles {
  align-self: start;
  gap: 16px;
}

.titles h3 {
  font-size: 32px;
  font-weight: bold;
}

.titles h4 {
  font-size: 16px;
  font-weight: bold;
}

/* Wrapper pour la vidéo pour maintenir le ratio 16:9 */
.video-wrapper {
  position: relative;
  width: 100%;
  padding-bottom: 56.25%; /* Ratio 16:9 (9/16 = 0.5625) */
  height: 0;
  overflow: hidden;
}

.video-wrapper iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

/* Responsive pour les petits écrans */
@media screen and (max-width: 900px) {
  .point-phryge {
    width: 100px;
    height: 100px;
  }

  .titles h3 {
    font-size: 24px;
  }

  .titles h4 {
    font-size: 12px;
  }
}
</style>
