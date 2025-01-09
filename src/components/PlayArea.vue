<script setup>
import { ref, onMounted } from 'vue'
import WinOrLoseComp from './WinOrLoseComp.vue'

const cards = defineProps(['cards'])

const jsonData = ref([])
const resultInfo = ref([])
const clicked = []

const getRandomItem = (arr) => {
  if (!Array.isArray(arr) || arr.length === 0) return null

  return arr[Math.floor(Math.random() * arr.length)]
}

const fetchData = async () => {
  try {
    const response = await fetch('/data.json')
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    jsonData.value = await response.json()
    console.log('Fetched JSON Data:', jsonData.value)
    getCards()
  } catch (error) {
    console.error('Error fetching JSON:', error)
  }
}

const getCards = () => {
  while (resultInfo.value.length !== cards.cards) {
    const series = getRandomItem(Object.values(jsonData.value))
    const card = getRandomItem(series)

    const rand = Math.random()
    if (rand >= 0.5) {
      const standImage = getRandomItem(card.stand_images || [])
      if (standImage) {
        resultInfo.value.push({
          name: card.stand,
          image: standImage,
        })
      }
    } else {
      const userImage = getRandomItem(card.user_images || [])
      if (userImage) {
        resultInfo.value.push({
          name: card.user,
          image: userImage,
        })
      }
    }
  }

  console.log(resultInfo.value)
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
    console.log('you lost')
    best.value = Math.max(best.value, curr.value)
  } else {
    clicked.push(card)
    curr.value++
    console.log(clicked)
    if (curr.value === cards.cards) {
      console.log('you won')
      best.value = Math.max(best.value, curr.value)
    } else {
      shuffleArray()
    }
  }
}

onMounted(() => {
  fetchData()
})

const reset = () => {
  curr.value = 0
  best.value = 0
  clicked.length = 0
  resultInfo.value.length = 0
  getCards()
}

const curr = ref(0)
const best = ref(0)
</script>

<template>
  <div>
    <div class="headers">
      <h2 style="color: white" class="header1">Current Score : {{ curr }}</h2>
      <h2 style="color: white" class="header2">Best Score : {{ best }}</h2>
    </div>
    <div class="cardspace">
      <div v-for="(card, index) in resultInfo" :key="index" class="card" @click="shuffle(card)">
        <img :src="card.image" class="image" alt="image couldnt be loaded" />
        <p style="color: white">{{ card.name }}</p>
      </div>
    </div>
    <div class="rbuttondiv">
      <button @click="reset" class="resetB"><p class="text">Reset</p></button>
    </div>
  </div>
  <WinOrLoseComp />
</template>

<style scoped>
.rbuttondiv {
  display: flex;
  justify-content: center;

  padding-top: 1rem;
  margin-bottom: 1.65rem;
}
.resetB {
  text-align: center;

  width: 15rem;
  height: 3rem;
  background: #800080;
  border: 0.1rem solid #fd4300;
  border-radius: 2.5rem;
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
  width: 3rem;
  height: 4rem;
}
.cardspace {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  width: 100%;
  height: 25rem;
  padding: 2rem 20rem 2rem 20rem;
}
</style>
