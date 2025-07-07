<template>
  <div class="app">
    <IntroPopup />
    <!-- Büyük Mektup İkonu -->
    <div class="envelope-icon" @click="handleEnvelopeClick">
      💌
    </div>

    <!-- Müzik Çalar (Sabit) -->
    <AudioPlayer ref="audioPlayer" />
    <!-- Açılan Mektup -->
    <LoveLetter v-if="showLetter" @close="showLetter = false" />
    <LoveNotes />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import LoveLetter from './components/LoveLetter.vue';
import AudioPlayer from './components/AudioPlayer.vue';
import IntroPopup from './components/IntroPopup.vue';
import LoveNotes from './components/LoveNotes.vue';


const showLetter = ref(false);
const audioPlayer = ref(null); // AudioPlayer bileşenine erişim

const handleEnvelopeClick = () => {
  showLetter.value = true;

  // AudioPlayer'daki play fonksiyonunu çağır
  audioPlayer.value?.playAudio?.();
};
</script>

<style>
/* Ana Sayfa Stilleri */
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
  box-sizing: border-box;
}

/* Mektup İkonu */
.envelope-icon {
  font-size: 420px;
  cursor: pointer;
  animation: float 3s ease-in-out infinite, pulse 1.5s infinite;
  filter: drop-shadow(0 0 10px rgba(255, 105, 180, 0.7));
  transition: transform 0.3s ease;
}

.envelope-icon:hover {
  transform: scale(1.1);
}

/* Animasyonlar */
@keyframes float {
  0% {
    transform: translateY(0px);
  }

  50% {
    transform: translateY(-20px);
  }

  100% {
    transform: translateY(0px);
  }
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.1);
  }

  100% {
    transform: scale(1);
  }
}

/* 📱 Mobil Uyumluluk */
@media (max-width: 768px) {
  .envelope-icon {
    font-size: 200px;
  }
}

@media (max-width: 480px) {
  .envelope-icon {
    font-size: 250px;
  }

  .app {
    padding: 10px;
  }
}
</style>