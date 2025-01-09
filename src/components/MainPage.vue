<script setup>
import Step1 from './MenuComp.vue'
import Step2 from './PlayArea.vue'
import { ref, computed } from 'vue'

let currentStep = ref(1)
let cards = ref(0)

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
    if (document.documentElement.requestFullscreen) {
      document.documentElement.requestFullscreen()
    } else if (document.documentElement.webkitRequestFullscreen) {
      document.documentElement.webkitRequestFullscreen()
    } else if (document.documentElement.mozRequestFullScreen) {
      document.documentElement.mozRequestFullScreen()
    } else if (document.documentElement.msRequestFullscreen) {
      document.documentElement.msRequestFullscreen()
    }
  } else {
    if (document.exitFullscreen) {
      document.exitFullscreen()
    } else if (document.webkitExitFullscreen) {
      document.webkitExitFullscreen()
    } else if (document.mozCancelFullScreen) {
      document.mozCancelFullScreen()
    } else if (document.msExitFullscreen) {
      document.msExitFullscreen()
    }
  }
}
</script>

<template>
  <div class="home">
    <div class="navbuttons">
      <img
        src="../assets/image copy.png"
        alt=""
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
        <button class="interactbuttons">
          <img src="../assets/image copy 5.png" class="interactimage" />
        </button>
        <button class="interactbuttons">
          <img src="../assets/image copy 6.png" class="interactimage" />
        </button>
      </div>
      <div>
        <button class="interactbuttons infobutton">?</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
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
