<script setup>
defineEmits(['start'])
</script>

<template>
  <div class="content">
    <div class="headers">
      <img src="../assets/image copy 3.png" class="mainh" />
      <img src="../assets/image copy 4.png" class="subh" />
    </div>
    <div class="buttons">
      <button class="startBs" @click="$emit('start', 5)"><p class="text1">Easy</p></button>
      <button class="startBs" @click="$emit('start', 10)"><p class="text2">Medium</p></button>
      <button class="startBs" @click="$emit('start', 15)"><p class="text3">Hard</p></button>
    </div>
  </div>
</template>

<style scoped>
.content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin-top: 1.6rem;
}

.headers {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.mainh {
  padding: 8rem 20rem 0 20rem;
}

.subh {
  padding: 4.5rem 30rem 0 30rem;
}

.buttons {
  display: flex;
  justify-content: space-around;
  padding: 4rem 10rem 4rem 10rem;
}

.startBs {
  text-align: center;

  width: 15rem;
  height: 3rem;
  background: #800080;
  border: 0.1rem solid #fd4300;
  border-radius: 2.5rem;
}

.text1,
.text2,
.text3 {
  font-style: normal;
  font-family: 'Anton';
  font-weight: 700;
  font-size: 1.5rem;
  line-height: 2rem;

  color: #ffffff;
  -webkit-text-stroke: 0.06rem #00ff00;
}

.text2 {
  -webkit-text-stroke: 0.05rem #ffff00;
}

.text3 {
  -webkit-text-stroke: 0.05rem #ff0000;
}
</style>
