<template>
  <div class="swiper-release-list">
    <div class="title">Releases</div>
    <div v-swiper:mySwiper="swiperOption">
      <div class="swiper-wrapper">
        <div v-for="i in sortByDate" class="swiper-slide item">
          <router-link v-touch-ripple v-if="i.slug" :to="'/release/' + i.slug + '/'" class="item__link" active-class="is-selected">
            <div class="item__wrapper">
              <div class="item__cover">
                <div v-if="i.cover" class="swiper-lazy-preloader swiper-lazy-preloader-white"></div>
                <img v-if="i.cover" class="item__img swiper-lazy"
                  :src="'https://content.sentimony.com/assets/img/releases/small/' + i.cat_no + '/' + i.slug + '.jpg'"
                  :srcset="'https://content.sentimony.com/assets/img/releases/small/' + i.cat_no + '/' + i.slug + '.jpg 1x, https://content.sentimony.com/assets/img/releases/small-retina/' + i.cat_no + '/' + i.slug + '.jpg 2x'"
                  :alt="i.title + ' Small Thumbnail'"
                >
                <div v-else class="item__soon">Artwork<br>in progress</div>
              </div>
              <div v-if="i.coming_soon" class="item__status--green">Coming Soon</div>
              <div v-if="i.new" class="item__status--red">Out Now</div>
            </div>
            <div class="item__title">
              {{ i.title }}
              <span v-if="i.format == 'EP'">{{ i.format }}</span>
            </div>
          </router-link>
        </div>
      </div>
      <div class="swiper-button-prev" slot="button-prev"></div>
      <div class="swiper-button-next" slot="button-next"></div>
      <!-- <div class="swiper-scrollbar" slot="scrollbar"></div> -->
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import sortBy from 'lodash/sortBy'

export default {
  data() {
    return {
      releases: [],
      swiperOption: {
        lazy: true,
        navigation: {
          prevEl: '.swiper-button-prev',
          nextEl: '.swiper-button-next'
        },
        // scrollbar: {
        //   el: '.swiper-scrollbar',
        //   hide: true
        // },
        mousewheelControl: true,
        mousewheelForceToAxis: true,
        freeMode: true,
        slidesPerView: 'auto',
        speed: 350,
        // slidesPerGroup: 2,
        slideToClickedSlide: true,
        centeredSlides: true
      }
    }
  },
  mounted () {
    return axios({
      url: 'https://sentimony-db.firebaseio.com/releases.json'
    })
    .then((res) => {
      this.releases = res.data;
    })
  },
  computed: {
    sortByDate () {
      return sortBy(this.releases, 'date').reverse()
    }
  }
}
</script>

<style lang="scss">
@import '../node_modules/coriolan-ui/tools/variables';
@import '../node_modules/coriolan-ui/mixins/media';
@import '../assets/scss/item';
@import '../assets/scss/title';
@import '../assets/scss/swiper-restyle';

.swiper-release-list {
  position: relative;
}
</style>
