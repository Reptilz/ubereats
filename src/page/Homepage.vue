<template>
  <div class="home">
    <div class="header">
      <img src="../assets/ubereatslogo.svg" alt="ubereats" />
      <div class="wrapper-input">
        <input
          v-model="user_search_restaurant"
          type="text"
          placeholder="De quoi avez-vous envie ?"
        />
        <i id="loupe" class="fa-solid fa-magnifying-glass"></i>
        <div class="search">
          <div v-for="(resto, i) in search_restaurant" :key="i" class="container-restaurant-search">
            <div class="wrapper-image">
              <img :src="resto.image" alt="resto.name">
            </div>
            <p>{{resto.name}}</p>
          </div>
        </div>
      </div>
    </div>
    <div class="banniere"></div>
    <RestaurantRow
      v-for="(data, index) in data_restaurant"
      v-bind:key="index"
      :resto="data"
    />
  </div>
</template>

<script>
//IMPORTS
import BDD from "../BDD";
import { onMounted, ref, watch } from "vue";

//COMPONENTS
import RestaurantRow from "../components/RestaurantRow.vue";

export default {
  name: "Home-page",
  components: {
    RestaurantRow,
  },
  setup() {
    class Restaurant {
      constructor(name, note, image, drive_time) {
        this.name = name;
        this.note = note;
        this.image = image;
        this.drive_time = drive_time;
      }
    }
    let data_restaurant = ref([]);
    let all_restaurants = [];

    const makeDataRestaurant = () => {
      let three_restaurants = [];

      for (const restaurant of BDD) {
        const new_restaurant = new Restaurant(
          restaurant.name,
          restaurant.note,
          restaurant.image,
          restaurant.drive_time
        );
        //make all restaurant array
        all_restaurants.push(new_restaurant);

        if (three_restaurants.length == 2) {
          three_restaurants.push(new_restaurant);
          data_restaurant.value.push(three_restaurants);
          three_restaurants = [];
        } else {
          three_restaurants.push(new_restaurant);
        }
      }
    };

    //user search restaurants
    let user_search_restaurant = ref("");
    let search_restaurant = ref([]);

    watch(user_search_restaurant, (new_value) => {
      let regex = RegExp(new_value.toLowerCase());
      let new_search_restaurant = all_restaurants.filter((restaurant) =>
        regex.test(restaurant.name.toLowerCase())
      );

      //vide la liste si il n'y a pas de recherche
      new_value == 0 ? search_restaurant.value = [] : search_restaurant.value = new_search_restaurant;

    });

    onMounted(makeDataRestaurant);

    //return
    return {
      data_restaurant,
      user_search_restaurant,
      search_restaurant,
    };
  },
};
</script>

<style lang="scss">
.home {
  .header {
    height: 120px;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    img {
      width: 200px;
    }
    .wrapper-input {
      position: relative;
      input {
        background-color: #ededed;
        border: none;
        border-radius: 20px;
        height: 50px;
        width: 350px;
        outline: none;
        padding: 0px 30px;
        &:hover {
          transition: .1s;
          border: 2px solid rgba(61, 61, 61, 0.6);
        }
      }
      .search {
        position: absolute;
        top: 100%;
        width: 100%;
        background-color: #ffff;
        .container-restaurant-search {
          display: flex;
          align-items: center;
          padding: 10px;
          &:hover {
            background-color: #f6f6f6;
          }
          .wrapper-image {
            height: 60px;
            width: 60px;
            margin-right: 25px;
            border-radius: 50%;
            overflow: hidden;
            img {
              height: 100%;
              width: auto;
            }
          }

        }
      }
    }
    #loupe {
      position: absolute;
      right: 50px;
      bottom: 50%;
      transform: translateY(50%);
      opacity: 0.5;
    }
  }
  .banniere {
    height: 200px;
    width: 100%;
    background-image: url("../assets/banniere.png");
    background-size: cover;
    background-position: center center;
  }
}
</style>
