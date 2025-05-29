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
      <p v-html="message"></p>
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
];

const message = "Você é aquela bagunça boa que eu adoro ter na minha vida. Entre risadas, provocações e olhares que dizem tudo sem precisar de palavras, a gente se entende de um jeito só nosso. Adoro como você me faz querer ficar pertinho. Que a nossa conexão continue sendo esse segredo gostoso, cheio de vontade, sorrisos, malícias e carinho.<br>😏❤️"
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
  padding: 12px 10px;
  background: rgba(0, 0, 0, 0.5);
  color: #fff;
  font-size: 1.4rem;
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
  min-height: 0;
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
  padding: 0;
  box-sizing: border-box;
}

.slide-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.slide-message {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 5%; /* Posicionado a 5% do final da tela */
  z-index: 15;
  background: rgba(0, 0, 0, 0.85);
  color: #fff;
  padding: 12px 15px;
  font-size: 0.95rem;
  text-align: justify;
  max-height: 35vh; /* Altura máxima reduzida */
  overflow-y: auto;
  box-sizing: border-box;
  margin: 0 10px;
  border-radius: 8px;
  width: calc(100% - 20px);
}

.slide-message p {
  margin: 5px 0;
  line-height: 1.5;
}

:deep(.swiper-pagination) {
  bottom: calc(5% + 35vh + 10px) !important; /* Acima da mensagem */
  z-index: 25;
}

:deep(.swiper-pagination-bullet-active) {
  background: #fff;
}

.swiper-button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 35px;
  height: 35px;
  background: rgba(0, 0, 0, 0.5);
  color: #fff;
  z-index: 20;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
}

.swiper-button-prev {
  left: 8px;
}

.swiper-button-next {
  right: 8px;
}

/* Media Queries para responsividade */
@media (max-width: 768px) and (orientation: portrait) {
  .carousel-title {
    font-size: 1.2rem;
    padding: 10px 8px;
  }
  
  .slide-message {
    font-size: 0.9rem;
    padding: 10px 12px;
    max-height: 30vh;
    bottom: 5%;
  }
  
  .swiper-button {
    width: 30px;
    height: 30px;
  }
  
  :deep(.swiper-pagination) {
    bottom: calc(5% + 30vh + 8px) !important;
  }
}

@media (max-width: 480px) and (orientation: portrait) {
  .carousel-container {
    width: 100vw;
    height: 100vh;
  }
  
  .carousel-title {
    font-size: 1.1rem;
    padding: 8px 6px;
  }
  
  .slide-message {
    font-size: 0.85rem;
    max-height: 35vh;
    padding: 5px;
    margin-bottom: 20px;
    bottom: 10%;
  }
  
  .swiper-button {
    width: 28px;
    height: 28px;
  }
  
  :deep(.swiper-pagination) {
    bottom: calc(5% + 35vh + 5px) !important;
  }
  
  /* Aumentar espaço para texto em telas muito curtas */
  @media (max-height: 700px) {
    .slide-message {
      max-height: 40vh;
      bottom: 2%;
    }
    :deep(.swiper-pagination) {
      bottom: calc(2% + 40vh + 5px) !important;
    }
  }
}

@media (max-width: 768px) and (orientation: landscape) {
  .slide-message {
    max-height: 50vh;
    font-size: 0.8rem;
    bottom: 5%;
  }
  
  .carousel-title {
    padding: 8px 6px;
    font-size: 1.1rem;
  }
  
  :deep(.swiper-pagination) {
    bottom: calc(5% + 50vh + 5px) !important;
  }
}

@media (min-width: 769px) {
  .carousel-container {
    max-width: 90%;
    max-height: 90vh;
    margin: auto;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
  }
  
  .slide-image {
    object-fit: contain;
  }
  
  .slide-message {
    width: calc(100% - 40px);
    margin: 0 20px;
    bottom: 20px;
    max-height: 25vh;
    border-radius: 8px;
  }
  
  :deep(.swiper-pagination) {
    bottom: calc(25vh + 25px) !important;
  }
}

@media (min-width: 1024px) {
  .carousel-container {
    max-width: 800px;
    max-height: 800px;
  }
}
</style>