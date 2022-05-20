<template>
  <div class="restaurant-card">
    <div :style="changeBackgroundImg" class="restaurant-card__img"></div>
    <div class="restaurant-card__informations">
      <div class="top">
        <p class="name">{{ info_resto.name }}</p>
        <p class="note">
          <span>{{ info_resto.note }}</span>
        </p>
      </div>
      <p class="time">{{ info_resto.drive_time }}</p>
    </div>
  </div>
</template>

<script>
import { computed } from "vue";

export default {
  name: "RestaurantCard",
  props: {
    info_resto: Object,
  },
  setup(props) {
    const changeBackgroundImg = computed(() => {
      return {
        backgroundImage: `url(${props.info_resto.image})`,
      };
    });

    return {
      changeBackgroundImg,
    };
  },
};
</script>

<style lang="scss">
.restaurant-card {
  width: 32%;
  height: 30vh;
  p {
    margin: 0;
  }
  &__img {
    background-image: url("https://cn-geo1.uber.com/image-proc/resize/eats/format=webp/width=550/height=440/quality=70/srcb64=aHR0cHM6Ly9kMXJhbHNvZ25qbmczNy5jbG91ZGZyb250Lm5ldC8zNzg4MDJiMC1jNTI4LTQ4MjktYjBiNS0wY2M2NDBkZjYzY2QuanBlZw==");
    height: 70%;
    background-size: cover;
    background-position: center;
  }
  &__informations {
    .top {
      margin: 10px 0px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      .note {
        background: #eeeeee;
        height: 30px;
        width: 30px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 0.8rem;
        border-radius: 50%;
      }
    }
    .time {
      font-size: 0.9rem;
    }
  }
}
</style>
