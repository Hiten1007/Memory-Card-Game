<script setup></script>

<template>
  <div id="modal" class="modal">
    <div class="modal-content">
      <div id="winmodal">
        <img src="../assets/image copy 7.png" alt="" class="resultimg1" /><img
          src="../assets/image copy 8.png"
          alt=""
          class="resulteffectimg1"
        />
      </div>
      <div id="losemodal" style="display: none">
        <img src="../assets/image copy 10.png" alt="" class="resultimg2" /><img
          src="../assets/image copy 11.png"
          alt=""
          class="resulteffectimg2"
        />
      </div>
      <img src="../assets/image copy 9.png" alt="" class="tryagain" />
    </div>
  </div>
</template>

<style scoped>
.tryagain {
  width: 8rem;
  padding-top: 0.8rem;
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1000;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: purple;
  padding: 1.5rem 2.5rem;
  border-radius: 2rem;
  width: 29rem;
  height: 22rem;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
#winmodal,
#losemodal {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  gap: 0.8rem;
}
.resultimg2 {
  width: 17rem;
  height: 3.4rem;
}
.resultimg1 {
  width: 16rem;
  height: 2.5rem;
}
.resulteffectimg1 {
  width: 20rem;
}
.resulteffectimg2 {
  width: 18rem;
}
</style>
