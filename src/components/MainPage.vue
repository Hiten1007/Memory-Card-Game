<script setup>
import Step1 from './MenuComp.vue'
import Step2 from './PlayArea.vue'
import { ref, computed, onMounted, onUnmounted, watch } from 'vue'

const instructions = ref(false)
const toggleinstruct = () => {
  instructions.value = !instructions.value
}

const music = ref(false)
const backgroundMusicPath =
  '/JoJo_s_Bizarre_Adventure_Phantom_Blood_OST_-_Fukutsu_Mushin_No_Sakebi_(mp3.pm).mp3'
let backgroundMusic = null

const musicSrc = computed(() => (music.value ? '/image copy 6.png' : '/image copy 13.png'))

const toggleMusic = () => {
  music.value = !music.value
  if (music.value) {
    backgroundMusic.play().catch((err) => console.warn('Music playback failed:', err))
  } else {
    backgroundMusic.pause()
  }
}

const audio = ref(true)
const audioSrc = computed(() => (audio.value ? '/image copy 5.png' : '/image copy 12.png'))

const clickSoundPath = '/mixkit-select-click-1109.wav'

const toggleAudio = () => {
  audio.value = !audio.value
}

const playClickSound = () => {
  if (audio.value) {
    const audioElement = new Audio(clickSoundPath)
    audioElement.play().catch((error) => {
      console.warn('Audio playback failed:', error)
    })
  }
}

watch(audio, (newValue) => {
  if (newValue) {
    window.addEventListener('click', playClickSound)
  } else {
    window.removeEventListener('click', playClickSound)
  }
})

onMounted(() => {
  backgroundMusic = new Audio(backgroundMusicPath)
  backgroundMusic.loop = true
  if (music.value) {
    backgroundMusic.play().catch((err) => console.warn('Music playback failed:', err))
  }

  if (audio.value) {
    window.addEventListener('click', playClickSound)
  }
})

onUnmounted(() => {
  if (backgroundMusic) {
    backgroundMusic.pause()
    backgroundMusic = null
  }
  window.removeEventListener('click', playClickSound)
})

const currentStep = ref(1)
const cards = ref(0)

const home = () => {
  currentStep.value = 1
}

const start = (difficulty) => {
  currentStep.value = 2
  cards.value = difficulty
}

const getCurrentStepComponent = computed(() => {
  return currentStep.value === 1 ? Step1 : Step2
})

const toggleFullscreen = () => {
  if (!document.fullscreenElement) {
    document.documentElement.requestFullscreen().catch((err) => {
      console.warn('Error attempting to enable fullscreen mode:', err)
    })
  } else {
    document.exitFullscreen().catch((err) => {
      console.warn('Error attempting to disable fullscreen mode:', err)
    })
  }
}
</script>

<template>
  <div class="home">
    <div class="navbuttons">
      <img
        src="../assets/image copy.png"
        alt="Home"
        height="75rem"
        width="75rem"
        style="margin: 1.6rem 1.6rem 0 1.6rem"
        @click="home"
      />
      <img
        src="../assets/image copy 2.png"
        height="25rem"
        width="25rem"
        style="margin: 2.4rem 1.5rem 0 0"
        @click="toggleFullscreen"
        class="topimages"
      />
    </div>

    <component :is="getCurrentStepComponent" :cards="cards" @start="start" />

    <div class="interact">
      <div class="audiobuttons">
        <button class="interactbuttons" @click="toggleAudio">
          <img :src="audioSrc" class="interactimage" />
        </button>
        <button class="interactbuttons" @click="toggleMusic">
          <img :src="musicSrc" class="interactimage" />
        </button>
      </div>
      <div>
        <div class="instructionbox">
          <button class="interactbuttons infobutton" @click="toggleinstruct">?</button>
          <div v-if="instructions" class="overlay" @click="toggleinstruct">
            <img src="../assets/image copy 14.png" class="instructorimage" />
            <div class="cloud">
              <p class="instructtext">1. Click the top left icon to return to the home page.</p>
              <p class="instructtext">2. Don’t click on the same card twice!</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.instructionbox {
  position: relative; /* Set this as the reference for positioning */
  display: inline-block; /* Ensure it takes the size of its contents */
}

/* Infobutton styles */
.infobutton {
  font-style: normal;
  font-family: 'Anton';
  font-weight: 700;
  font-size: 1.5rem;
  line-height: 2rem;
  color: #ffffff;
  z-index: 1; /* Ensures button remains clickable */
  position: relative; /* Relative to allow proper stacking of child elements */
}

/* Image copy 14 positioned relative to the infobutton */
.instructorimage {
  position: fixed;
  bottom: 5.5rem; /* Align with the top-left of the infobutton */
  right: 5rem; /* Adjust for horizontal alignment */
  width: 5rem; /* Adjust size */
  height: auto;
  z-index: 2; /* Above infobutton */
}

/* Cloud image (image copy 15) with text inside */
.cloud {
  position: fixed;
  bottom: 13.5rem; /* Adjust to align with the top-left of image copy 14 */
  right: 7.5rem; /* Horizontal alignment with image copy 14 */
  background-image: url('../assets/image copy 15.png');
  background-size: cover;
  background-repeat: no-repeat;
  width: 18rem;
  height: 15rem; /* Adjust size for the cloud */

  padding: 2rem; /* Inner padding for the text */
  border-radius: 1rem; /* Optional rounded corners */

  z-index: 3; /* Above image copy 14 */
  display: flex;
  flex-direction: column;
  gap: 0.1rem;
  padding-left: 2.9rem;
  padding-right: 2.7rem;
  padding-bottom: 6rem;
}

.cloud p {
  margin: 0;
  color: #333;
  font-size: 1rem;
  font-family: 'Anton';
  font-style: normal;
  font-weight: 700;

  color: #1e1e1e;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;

  z-index: 100;
}

.audiobuttons {
  display: flex;

  gap: 2rem;
}
.interactimage {
  height: 1.5rem;
  width: 1.5rem;
}
.infobutton {
  font-style: normal;
  font-family: 'Anton';
  font-weight: 700;
  font-size: 1.5rem;
  line-height: 2rem;

  color: #ffffff;
}
.interact {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-top: 1rem;
  padding: 0 2rem;
}
.interactbuttons {
  width: 4rem;
  height: 4rem;
  background: #800080;
  border: 0.15rem solid #fd4300;
  border-radius: 10rem;
  position: relative;
  overflow: hidden;
  transition: background-color 0.3s ease;
}

@keyframes bubble {
  0% {
    transform: scale(1);
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 0);
  }
  50% {
    transform: scale(1.1);
    box-shadow: 0 0 10px 5px rgba(255, 255, 255, 0.3);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 0);
  }
}

.interactbuttons:active {
  animation: bubble 0.8s ease-out;
}

.navbuttons {
  display: flex;
  justify-content: space-between;
  margin: 0 1rem;
}

.home {
  background-image: url('../assets/image.png');
  width: 100vw;
  min-height: 100vh;
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}
</style>
