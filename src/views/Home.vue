<template>
  <div class="home">
    <Carousel class="carousel" v-slot="{ currentSlide }">
      <Slide v-for="(slide,index) in carouselSlides" :key="index">
        <div v-show="currentSlide === index + 1" class="slideInfo">
            <img :src="require(`../assets/${slide}.jpg`)" alt="">
        </div>
      </Slide>
    </Carousel>
  </div>
</template>

<script>

import Carousel from "../components/Carousel.vue"
import Slide from "../components/Slide.vue"

export default {
  name: 'Home',
  components: {
    Slide,
    Carousel,
  },
  setup(){
    const carouselSlides = ['image1','image2','image3','image4']


    return {
      carouselSlides,
    }
  }
}

</script>


<style lang="scss" scoped>
  .carousel {
    position: relative;
    max-height: 100vh;
    height: 100vh;
    overflow-y: hidden;
  }

  .slideInfo{
    position: absolute;
    top:0;
    left: 0;
    width: 100%;
    max-height: 100%;
    height: 100%;

    img{
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }


</style>