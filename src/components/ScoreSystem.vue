<template>
  <div class="score">
    <h2>Score: {{ score }}</h2>
  </div>
</template>

<script>
export default {
  name: "ScoreSystem",
  props: {
    score: {
      type: Number,
      required: true,
    },
  },
  
};
</script>

<style scoped>
.score {
  position: absolute;
  top: 0px;
  left: 20px;
  background-color: rgba(255, 255, 255, 0.8);
  padding: 10px;
  border-radius: 5px;
}
</style>
