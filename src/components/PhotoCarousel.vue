<template>
  <div class="carousel-container" @click="goToNextSlide">
    <h3 class="carousel-title">{{ title }}</h3>
    <Swiper
      ref="swiperRef"
      :modules="[SwiperAutoplay, SwiperPagination]"
      :slides-per-view="1"
      :loop="true"
      :autoplay="{ delay: 5000, disableOnInteraction: false }"
      :pagination="{ clickable: true }"
      class="carousel-swiper"
    >
      <SwiperSlide v-for="(slide, index) in slides" :key="index">
        <div class="slide-image" :style="{ backgroundImage: `url(${slide.img})` }"></div>
      </SwiperSlide>
    </Swiper>

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
</template>

<script setup>
import { ref, onMounted } from 'vue'
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

onMounted(() => {
  playAudio();
});

function goToNextSlide() {
  if (swiperRef.value && swiperRef.value.swiper) {
    swiperRef.value.swiper.slideNext();
    playAudio();
  }
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
  border-top-left-radius: 24px;
  border-top-right-radius: 24px;
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
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
  position: relative;
  display: flex;
  align-items: flex-end;
  justify-content: center;
}

.slide-message {
  width: 100vw;
  max-width: 100vw;
  box-sizing: border-box;
  max-height: 25vh;
  min-height: 80px;
  background: rgba(0, 0, 0, 0.5);
  color: #fff;
  box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.4);
  padding: 24px 0 32px 0;
  border-top-left-radius: 24px;
  border-top-right-radius: 24px;
  font-size: 1.2em;
  position: absolute;
  bottom: 60px;
  left: 0;
  right: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  z-index: 5;
  overflow-y: auto;
}

.slide-message p {
  margin-bottom: 0;
  padding: 0 24px;
  width: 100%;
  box-sizing: border-box;
  word-break: break-word;
}

.audio-controls {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 10px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  z-index: 10;
  pointer-events: auto;
  gap: 12px;
  max-width: 100vw;
}

.audio-btn {
  background: rgba(0, 0, 0, 0.7);
  border: none;
  border-radius: 50%;
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 8px;
  cursor: pointer;
  padding: 0;
}

.progress-bar-container {
  width: 50vw;
  max-width: 300px;
  min-width: 100px;
  height: 8px;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 4px;
  overflow: hidden;
  margin: 0 8px;
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
  font-size: 0.95em;
  text-shadow: 0 1px 2px #000;
  min-width: 70px;
  text-align: right;
  margin-left: 8px;
}

:deep(.swiper-pagination-bullet-active) {
  background: #fff;
}

/* Responsivo para telas menores */
@media (max-width: 600px) {
  .carousel-title,
  .slide-message {
    font-size: 1em;
    padding-left: 8px;
    padding-right: 8px;
  }
  .slide-message p {
    padding: 0 8px;
  }
  .progress-bar-container {
    max-width: 180px;
    min-width: 60px;
  }
}
</style>