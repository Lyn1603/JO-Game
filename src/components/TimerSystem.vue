<template>
  <div class="timer">
    <p>{{ duration }} secondes </p>
  </div>
</template>

<script>
export default {
  name: "TimerSystem",
  props: {
    duration: {
      type: Number,
      required: true,
    },
  },

};
</script>

<style scoped lang="scss">
.timer {
  position: absolute;
  top:  0;
  left: 0 ;
  right: 0;
  font-size: 24px;
  color: #333;
  padding: 10px;
  border-radius: 8px;
  text-align: center;

  p{
    font-family:Rubik ;
  }
}
</style>
