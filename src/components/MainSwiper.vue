<template>
  <div>
    <swiper>
      <template>
        <swiper-item v-for="(item, index) in dataSrc" :key="index">
          <template>
            <a href=""><img :src="item" alt=""/></a>
          </template>
        </swiper-item>
      </template>
    </swiper>
  </div>
</template>
<script>
import Swiper from "components/swiper/Swiper.vue";
import SwiperItem from "components/swiper/SwiperItem.vue";
export default {
  data() {
    return {
      //dataSrc由 Home父传子
      dataSrc: [
        require("assets/img/img1.jpg"),
        require("assets/img/img2.jpg"),
        require("assets/img/img3.jpg"),
        require("assets/img/img4.jpg")
      ]
    };
  },
  components: {
    Swiper,
    SwiperItem
  }
};
</script>
<style></style>
