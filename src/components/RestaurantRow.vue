<template>
  <div class="restaurant-row">
      <h2 class="title">Nos restaurants</h2>
      <div class="wrapper-card">
          <RestaurantCard v-for="(card, i) in resto" :key="i" :info_resto="card" />
      </div>
  </div>
</template>

<script>
import RestaurantCard from './RestaurantCard.vue';

export default {
    name: "RestaurantRow",
    components: {
        RestaurantCard,
    },
    props: {
        resto: Array
    }
}
</script>

<style lang="scss">
    .restaurant-row {
        .title {
            font-size: 2rem;
        }
        .wrapper-card {
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
    }
</style>