<script>
export default {
  name: "ScoreBoard",
  mounted() {
    this.getScore();
  },
  data(){
    return{
      scores: [],
    }
  },

  methods:{
    //on recupere le score et le temps dans le local storage
    getScore(){
      this.scores = JSON.parse(localStorage.getItem('scores')) || [];
    }
  }
}
</script>

<template>
  <div class="scord-board">
    <h3>Tableau de score</h3>
    <ul v-for="(score, index) in scores" :key="index">
      <li>{{ score.name }} : {{ score.score }}</li>
    </ul>
  </div>

</template>

<style scoped>
.scord-board{
  padding: 32px;
  overflow-scrolling: auto;

  h3{
    text-align: center;
    font-size: 24px;
    font-family: Rubik;
  }

  ul{
    list-style: none;
    padding: 0;
    margin: 0;
    font-family: Rubik;
    font-size: 18px;
  }

  li{
    padding: 8px;
    border-bottom: 1px solid #ccc;
  }
}


</style>
