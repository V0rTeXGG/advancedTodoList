<template>
  <swiper
    :spaceBetween="30"
    :pagination="{
      clickable: true,
    }"
    :modules="modules"
    class="mySwiper"
  >
    <swiper-slide v-for="task in dailyTask" :key="task.id">
      <div class="slide-wrapper">
        <p class="slider-wrapper__title">{{task.title}}</p>
        <div v-if="task.tags.length > 0" class="slide-wrapper__tags">
          <span v-for="tag in task.tags" class="slide-wrapper-tags__tag">{{tag}}</span>
        </div>
        <p v-else class="slider-wrapper__empty-tags">Non-tags</p>
        <router-link :to="'/task/' + task.id" tag="button" class="slide-wrapper__btn">Open</router-link>
      </div>
    </swiper-slide>
<!--    <swiper-slide>Slide 2</swiper-slide>-->
<!--    <swiper-slide>Slide 3</swiper-slide>-->
<!--    <swiper-slide>Slide 4</swiper-slide>-->
<!--    <swiper-slide>Slide 5</swiper-slide>-->
<!--    <swiper-slide>Slide 6</swiper-slide>-->
<!--    <swiper-slide>Slide 7</swiper-slide>-->
<!--    <swiper-slide>Slide 8</swiper-slide>-->
<!--    <swiper-slide>Slide 9</swiper-slide>-->
  </swiper>
</template>
<script>
  // Import Swiper Vue.js components
  import { Swiper, SwiperSlide } from 'swiper/vue';

  // Import Swiper styles
  import 'swiper/css';

  import 'swiper/css/pagination';

  // import required modules
  import { Pagination } from 'swiper';

  export default {
    name: 'app-slider',
    components: {
      Swiper,
      SwiperSlide,
    },
    setup() {
      return {
        modules: [Pagination],
      };
    },
    computed: {
      tasks() {
        return this.$store.getters.tasks
      },
      dailyTask() {
        return this.tasks.filter(task => new Date(task.date).getDate() === new Date().getDate())
      }
    },
  };
</script>
<style src="@/style/slider.scss" lang="scss" scoped>
@import "@/style/varibles.scss";

.swiper-pagination-bullet-active {
  background-color: #000 !important;
}

</style>
