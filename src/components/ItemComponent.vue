<template>
  <div class="item-component">
    <ul class="item-list">
      <li v-for="item in items" :key="item.msg" class="item-card">
        <h3>{{ item.name }}</h3>
        <div class="item-content">
          <img :src="item.img" alt="" class="item-image">
          <p>{{ item.msg }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'ItemComponent',
  data() {
    return {
      items: [
        { name: 'Médaille d\'or', img: require('@/assets/or.png'), msg: '+50 points' },
        { name: 'Médaille d\'argent', img: require('@/assets/argent.png'), msg: '+20 points' },
        { name: 'Médaille de bronze', img: require('@/assets/bronze.png'), msg: '+10 points' },
        { name: 'Flamme olympique', img: require('@/assets/bonus.png'), msg: 'Vous rajoute 10 secondes' },
        { name: 'Anne Hidalgo', img: require('@/assets/malus.png'), msg: 'Vous divise vos points par 2 et vous enlève 10 secondes' }
      ]
    };
  }
}
</script>

<style scoped>
.item-component {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.item-list {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  list-style: none;
  padding: 0;
  margin: 0;
}

.item-card {
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  padding: 20px;
  max-width: 200px;
  text-align: center;
  transition: transform 0.3s, box-shadow 0.3s;
}

.item-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.item-image {
  width: 40%;
  max-width: 150px;
  height: auto;
  object-fit: contain;
  margin-bottom: 10px;
}

@media (max-width: 768px) {
  .item-list {
    grid-template-columns: repeat(2, 1fr); /* 2 colonnes pour les tablettes */
  }

  .item-card {
    max-width: 150px;
  }

  .item-image {
    width: 80px;
    height: 80px;
  }
}

@media (max-width: 480px) {
  .item-list {
    grid-template-columns: 1fr; /* 1 colonne pour les petits écrans */
  }

  .item-card {
    max-width: 100%;
  }
}
</style>
