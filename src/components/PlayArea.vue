<script setup>
import { ref, onMounted } from 'vue'
import WinOrLoseComp from './WinOrLoseComp.vue'

const showGif = ref(false)

defineEmits(['start'])
const cards = defineProps(['cards'])

const jsonData = ref([])
const resultInfo = ref([])
const clicked = []

const isFlipping = ref(false) // Control flip state

const getRandomItem = (arr) => {
  if (!Array.isArray(arr) || arr.length === 0) return null
  return arr[Math.floor(Math.random() * arr.length)]
}

const fetchData = async () => {
  try {
    const response = await fetch('/data.json')
    if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`)
    jsonData.value = await response.json()
    showGif.value = true
    setTimeout(getCards, 5000)
  } catch (error) {
    console.error('Error fetching JSON:', error)
  }
}

const getCards = () => {
  while (resultInfo.value.length !== cards.cards) {
    const series = getRandomItem(Object.values(jsonData.value))
    const card = getRandomItem(series)
    const rand = Math.random()
    const image =
      rand >= 0.5 ? getRandomItem(card.stand_images || []) : getRandomItem(card.user_images || [])
    if (image) {
      resultInfo.value.push({ name: rand >= 0.5 ? card.stand : card.user, image })
    }
  }

  showGif.value = false
}

const shuffleArray = () => {
  const shuffled = [...resultInfo.value]
  for (let i = shuffled.length - 1; i > 0; i--) {
    const randomIndex = Math.floor(Math.random() * (i + 1))
    ;[shuffled[i], shuffled[randomIndex]] = [shuffled[randomIndex], shuffled[i]]
  }
  resultInfo.value = shuffled
}

const shuffle = (card) => {
  if (clicked.includes(card)) {
    lose.value = true
    best.value = Math.max(best.value, curr.value)
  } else {
    clicked.push(card)
    curr.value++
    if (curr.value === cards.cards) {
      win.value = true
      best.value = Math.max(best.value, curr.value)
    } else {
      // Start flipping all cards
      isFlipping.value = true

      setTimeout(() => {
        shuffleArray() // Shuffle after flip animation
        isFlipping.value = false // Stop flipping after shuffle
      }, 600) // Duration should match the flip animation
    }
  }
}

const reset = () => {
  curr.value = 0
  best.value = 0
  clicked.length = 0
  resultInfo.value.splice(0)
  getCards()
  isFlipping.value = true

  // After flip animation, shuffle and reset
  setTimeout(() => {
    isFlipping.value = false // Stop flipping after reset
  }, 600) // Duration should match the flip animation
}

const tryagain = () => {
  win.value = false
  lose.value = false
  curr.value = 0
  clicked.length = 0
  resultInfo.value.splice(0)
  getCards()
  isFlipping.value = true

  // After flip animation, shuffle and retry
  setTimeout(() => {
    isFlipping.value = false // Stop flipping after retry
  }, 600) // Duration should match the flip animation
}

const curr = ref(0)
const best = ref(0)
const win = ref(false)
const lose = ref(false)

onMounted(() => {
  fetchData()
})
</script>

<template>
  <div>
    <div class="headers">
      <h2 style="color: white" class="header1">Current Score : {{ curr }}</h2>
      <h2 style="color: white" class="header2">Best Score : {{ best }}</h2>
    </div>
    <div class="cardspace">
      <div v-if="showGif" class="gif-container">
        <!-- Replace with your GIF -->
        <img
          src="../assets/jojo_ss___rohan_loading_gif_by_jindopon_dac2vsz(1).gif"
          alt="Loading..."
          width="200rem"
        />
      </div>
      <div
        v-else
        v-for="(card, index) in resultInfo"
        :key="index"
        class="card-container"
        :class="{ flip: isFlipping }"
        @click="shuffle(card)"
      >
        <div class="card">
          <div class="front">
            <img :src="card.image" class="image" alt="image couldn't be loaded" />
            <div class="cardtextbox">
              <p class="cardtext">{{ card.name }}</p>
            </div>
          </div>
          <div class="back">
            <img src="../assets/image copy 16.png" width="25rem" />
          </div>
        </div>
      </div>
    </div>
    <div class="rbuttondiv">
      <button @click="reset" class="resetB"><p class="text">Reset</p></button>
    </div>
  </div>
  <WinOrLoseComp @tryagain="tryagain" :win="win" :lose="lose" />
</template>

<style scoped>
.gif-container {
  padding: 20vh 19.5vw;
}

.cardtextbox {
  background-color: #fd4300;
  width: 100%;
  padding: 0.2rem 0.5rem; /* Adjust padding for better text placement */
  position: absolute;
  bottom: 0; /* Place it at the bottom of the card */
  border-radius: 0 0 1.5rem 1.5rem; /* Rounded bottom corners */
}

.cardtext {
  font-family: 'Antonio';
  font-style: normal;
  font-weight: 700;
  font-size: 0.8rem; /* Increase font size as needed */
  line-height: 0.8rem;
  text-align: center;
  color: #ffffff;
  word-wrap: break-word; /* Ensures text wraps correctly without overflowing */
}

.card-container {
  perspective: 1000px;
  display: inline-block;
  margin: 0.5rem 1rem;
  position: relative; /* Ensure positioning context for the .cardtextbox */
}

.card {
  width: 5.4rem;
  height: 8.7rem;
  transform-style: preserve-3d;
  transition: transform 0.6s ease-in-out;
}

.card .front,
.card .back {
  position: absolute;
  backface-visibility: hidden;
  width: 100%;
  height: 100%;
}

.card .front {
  background: #800080;
  border: 0.15rem solid #fd4300;
  border-radius: 1.5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.card .back {
  background: #800080;
  border: 0.15rem solid #fd4300;
  border-radius: 1.5rem;
  transform: rotateY(180deg);
  display: flex;
  justify-content: center;
  align-items: center;
}

.card-container.flip .card {
  transform: rotateY(180deg);
}

.rbuttondiv {
  display: flex;
  justify-content: center;
  padding-top: 1rem;
  margin-bottom: 0 rem;
}
.resetB {
  text-align: center;
  width: 15rem;
  height: 3rem;
  background: #800080;
  border: 0.1rem solid #fd4300;
  border-radius: 2.5rem;
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

.resetB:active {
  animation: bubble 0.3s ease-out;
}
.text {
  font-style: normal;
  font-family: 'Anton';
  font-weight: 700;
  font-size: 1.5rem;
  line-height: 2rem;

  color: #ffffff;
  -webkit-text-stroke: 0.06rem #002bff;
}
.headers {
  display: flex;
  justify-content: center;
  flex-direction: column;
}
.header1,
.header2 {
  display: flex;
  justify-content: end;
  font-style: normal;
  padding-right: 5rem;

  font-family: 'Anton';
  font-weight: 700;
  font-size: 1.5rem;
  line-height: 2rem;

  color: #ffffff;
  letter-spacing: 0.08rem;
}

.header1 {
  -webkit-text-stroke: 0.06rem #ff0000;
}

.header2 {
  -webkit-text-stroke: 0.06rem #00ff00;
}

.image {
  width: 5rem;
  height: 6rem;
  color: #ffd700;
  font-family: 'Antonio';
  font-style: normal;
  font-weight: 700;
  font-size: 0.7rem;
  text-wrap-mode: wrap;
  text-align: center;
}
.cardspace {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  width: 100%;
  height: 28rem;
  padding: 0rem 20rem 2rem 20rem;
}
</style>
