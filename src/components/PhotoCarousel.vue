<template>
  <div class="carousel-container">
    <h3 class="carousel-title">{{ title }}</h3>
    <Swiper
      ref="swiperRef"
      :modules="[SwiperAutoplay, SwiperPagination]"
      :slides-per-view="1"
      :loop="true"
      :autoplay="{ delay: 5000, disableOnInteraction: false }"
      :pagination="{ clickable: true }"
      class="carousel-swiper"
      @swiper="onSwiper"
    >
      <SwiperSlide v-for="(slide, index) in slides" :key="index">
        <div class="slide-image" :style="{ backgroundImage: `url(${slide.img})` }"></div>
      </SwiperSlide>
    </Swiper>

    <div class="swiper-button swiper-button-prev" @click.stop="slidePrev">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none"><path d="M15.41 7.41L14 6L8 12L14 18L15.41 16.59L10.83 12L15.41 7.41Z" fill="white"/></svg>
    </div>
    <div class="swiper-button swiper-button-next" @click.stop="slideNext">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none"><path d="M8.59 16.59L10 18L16 12L10 6L8.59 7.41L13.17 12L8.59 16.59Z" fill="white"/></svg>
    </div>

    <div class="message-player-box">
      <div class="slide-message">
        <p>{{ message }}</p>
      </div>
      <div class="audio-controls">
        <button class="audio-btn" @click.stop="toggleAudio">
          <span v-if="isPlaying">
            <!-- Ícone de pause SVG -->
            <svg width="28" height="28" viewBox="0 0 24 24" fill="none"><rect x="6" y="5" width="4" height="14" rx="1" fill="white"/><rect x="14" y="5" width="4" height="14" rx="1" fill="white"/></svg>
          </span>
          <span v-else>
            <!-- Ícone de play SVG -->
            <svg width="28" height="28" viewBox="0 0 24 24" fill="none"><polygon points="7,5 21,12 7,19" fill="white"/></svg>
          </span>
        </button>
        <div class="progress-bar-container">
          <div class="progress-bar" :style="{ width: progress + '%' }"></div>
        </div>
        <div class="audio-time">
          {{ formatTime(currentTime) }} / {{ formatTime(duration) }}
        </div>
      </div>
      <audio ref="audioRef" :src="audioSrc" loop @timeupdate="updateProgress" @loadedmetadata="updateDuration" autoplay />
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
const audioRef = ref(null)
const audioSrc = '/nosso-primeiro-beijo.mp3'

const isPlaying = ref(true)
const currentTime = ref(0)
const duration = ref(0)
const progress = ref(0)
const swiperInstance = ref(null)

function playAudio() {
  if (audioRef.value) {
    audioRef.value.play()
    isPlaying.value = true
  }
}

function pauseAudio() {
  if (audioRef.value) {
    audioRef.value.pause()
    isPlaying.value = false
  }
}

function toggleAudio() {
  if (!audioRef.value) return
  if (isPlaying.value) {
    pauseAudio()
  } else {
    playAudio()
  }
}

function updateProgress() {
  if (audioRef.value) {
    currentTime.value = audioRef.value.currentTime
    duration.value = audioRef.value.duration || 0
    progress.value = duration.value
      ? (audioRef.value.currentTime / duration.value) * 100
      : 0
  }
}

function updateDuration() {
  if (audioRef.value) {
    duration.value = audioRef.value.duration || 0
  }
}

function formatTime(sec) {
  if (!sec || isNaN(sec)) return '00:00'
  const m = Math.floor(sec / 60)
  const s = Math.floor(sec % 60)
  return `${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`
}

function goToNextSlide() {
  if (swiperRef.value && swiperRef.value.swiper) {
    swiperRef.value.swiper.slideNext();
    playAudio();
  }
}

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
  { img: '/couple-photo1.jpg' },
  { img: '/couple-photo2.jpg' },
  { img: '/couple-photo3.jpg' },
  { img: '/couple-photo4.jpg' },
];

const message = "Você é aquela bagunça boa que eu adoro ter na minha vida. Entre risadas, provocações e olhares que dizem tudo sem precisar de palavras, a gente se entende de um jeito só nosso. Adoro como você me faz querer ficar pertinho. Que a nossa conexão continue sendo esse segredo gostoso, cheio de vontade, sorrisos, malicias e carido. 😏❤️"
</script>

<style scoped>
.carousel-container {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  position: relative;
  max-width: 100vw;
  max-height: 100vh;
  touch-action: pan-y;
}

.carousel-title {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  margin: 0;
  padding: 24px 16px 12px 16px;
  background: rgba(0,0,0,0.5);
  color: #fff;
  font-size: 1.5em;
  font-weight: bold;
  text-align: center;
  z-index: 20;
  text-transform: uppercase;
}

.carousel-swiper,
:deep(.swiper) {
  width: 100vw;
  height: 100vh;
  max-width: 100vw;
  max-height: 100vh;
}

.slide-image {
  width: 100vw;
  height: 100vh;
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  position: relative;
  display: flex;
  align-items: flex-end;
  justify-content: center;
}

.message-player-box {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 15;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100vw;
  max-width: 100vw;
  pointer-events: auto;
}

.slide-message {
  width: 100%;
  max-width: 100%;
  box-sizing: border-box;
  max-height: 22vh;
  min-height: 50px;
  background: rgba(0, 0, 0, 0.5);
  color: #fff;
  box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.4);
  padding: 14px 16px 8px 16px;
  font-size: 1.1em;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  overflow-y: auto;
  flex-grow: 1;
}

.slide-message p {
  margin-bottom: 0;
  padding: 0 16px;
  width: 90%;
  box-sizing: border-box;
  word-break: break-word;
}

.audio-controls {
  width: 100vw;
  max-width: 100vw;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 6px 0 10px 0;
  background: rgba(0,0,0,0.5);
  border-bottom-left-radius: 24px;
  border-bottom-right-radius: 24px;
  flex-shrink: 0;
}

.progress-bar-container {
  width: 50vw;
  max-width: 240px;
  min-width: 80px;
  height: 6px;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 4px;
  overflow: hidden;
  margin: 0 6px;
  flex: 1;
}

.progress-bar {
  height: 100%;
  background: #fff;
  width: 0%;
  transition: width 0.2s;
}

.audio-time {
  color: #fff;
  font-size: 0.85em;
  text-shadow: 0 1px 2px #000;
  min-width: 60px;
  text-align: right;
  margin-left: 6px;
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

@media (max-width: 600px) {
  .carousel-title,
  .slide-message {
    font-size: 1em;
    padding-left: 8px;
    padding-right: 8px;
  }
  .slide-message {
    max-height: 18vh;
    padding: 10px 0 4px 0;
    font-size: 1em;
  }
  .slide-message p {
    padding: 0 12px;
  }
  .progress-bar-container {
    max-width: 160px;
    min-width: 40px;
  }
}

</style>