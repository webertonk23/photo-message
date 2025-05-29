<template>
  <div class="carousel-container">
    <h3 class="carousel-title">{{ title }}</h3>
    
    <div class="swiper-wrapper">
      <Swiper ref="swiperRef" :modules="[SwiperAutoplay, SwiperPagination]" 
        :slides-per-view="1" :loop="true"
        :autoplay="{ delay: 5000, disableOnInteraction: false }" 
        :pagination="{ clickable: true }"
        class="carousel-swiper" @swiper="onSwiper">
        
        <SwiperSlide v-for="(slide, index) in slides" :key="index">
          <div class="slide-content">
            <img :src="slide.img" alt="Slide" class="slide-image">
          </div>
        </SwiperSlide>
      </Swiper>

      <div class="swiper-button swiper-button-prev" @click.stop="slidePrev">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
          <path d="M15.41 7.41L14 6L8 12L14 18L15.41 16.59L10.83 12L15.41 7.41Z" fill="white" />
        </svg>
      </div>
      <div class="swiper-button swiper-button-next" @click.stop="slideNext">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
          <path d="M8.59 16.59L10 18L16 12L10 6L8.59 7.41L13.17 12L8.59 16.59Z" fill="white" />
        </svg>
      </div>
    </div>

    <div class="slide-message">
      <p>{{ message }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { Swiper, SwiperSlide } from 'swiper/vue';
import { Autoplay, Pagination } from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/pagination';

const SwiperAutoplay = Autoplay;
const SwiperPagination = Pagination;

const swiperRef = ref(null);
const swiperInstance = ref(null)

function slideNext() {
  if (swiperInstance.value) {
    swiperInstance.value.slideNext();
  }
}

function slidePrev() {
  if (swiperInstance.value) {
    swiperInstance.value.slidePrev();
  }
}

function onSwiper(swiper) {
  swiperInstance.value = swiper;
}

const title = "parabéns";

const slides = [
  { img: '/couple-photo3.jpg' },
  { img: '/couple-photo1.jpg' },
  { img: '/couple-photo2.jpg' },
  { img: '/couple-photo4.jpg' },
];

const message = "Você é aquela bagunça boa que eu adoro ter na minha vida. Entre risadas, provocações e olhares que dizem tudo sem precisar de palavras, a gente se entende de um jeito só nosso. Adoro como você me faz querer ficar pertinho. Que a nossa conexão continue sendo esse segredo gostoso, cheio de vontade, sorrisos, malícias e carinho. 😏❤️"
</script>

<style scoped>
.carousel-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 100vw;
  height: 100vh;
  max-height: 100vh;
  overflow: hidden;
  position: relative;
  background: #000;
}

.carousel-title {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  margin: 0;
  padding: 15px 10px;
  background: rgba(0, 0, 0, 0.5);
  color: #fff;
  font-size: 1.5rem;
  font-weight: bold;
  text-align: center;
  z-index: 20;
  text-transform: uppercase;
}

.swiper-wrapper {
  flex: 1;
  position: relative;
  display: flex;
  flex-direction: column;
  min-height: 0; /* Fix para flexbox em browsers antigos */
}

.carousel-swiper,
:deep(.swiper) {
  width: 100%;
  height: 100%;
  flex: 1;
  min-height: 0;
}

.slide-content {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  padding: 40px 0 60px; /* Espaço para título e mensagem */
  box-sizing: border-box;
}

.slide-image {
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
  object-fit: contain;
  display: block;
  margin: 0 auto;
}

.slide-message {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 15;
  background: rgba(0, 0, 0, 0.7);
  color: #fff;
  padding: 12px 15px;
  font-size: 0.95rem;
  text-align: justify;
  max-height: 30vh;
  overflow-y: auto;
}

.slide-message p {
  margin: 5px;
  line-height: 1.4;
}

:deep(.swiper-pagination) {
  bottom: 30vh !important;
}

:deep(.swiper-pagination-bullet-active) {
  background: #fff;
}

.swiper-button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 40px;
  height: 40px;
  background: rgba(0, 0, 0, 0.3);
  color: #fff;
  z-index: 20;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
}

.swiper-button-prev {
  left: 10px;
}

.swiper-button-next {
  right: 10px;
}

/* Media Queries para responsividade */
@media (max-width: 768px) {
  .carousel-title {
    font-size: 1.2rem;
    padding: 12px 8px;
  }
  
  .slide-message {
    font-size: 0.85rem;
    padding: 10px 12px;
    max-height: 25vh;
  }
  
  :deep(.swiper-pagination) {
    bottom: 25vh !important;
  }
}

@media (max-width: 480px) {
  .carousel-title {
    font-size: 1.1rem;
  }
  
  .slide-message {
    font-size: 0.8rem;
    max-height: 20vh;
  }
  
  .swiper-button {
    width: 30px;
    height: 30px;
  }
  
  :deep(.swiper-pagination) {
    bottom: 20vh !important;
  }
}

@media (min-width: 1024px) {
  .carousel-container {
    max-width: 800px;
    margin: 0 auto;
    height: 90vh;
    max-height: 800px;
  }
  
  .slide-image {
    max-height: 90%;
  }
}
</style>