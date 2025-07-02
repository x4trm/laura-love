<script setup>
import { ref, computed } from 'vue';

const stage = ref(0);
const noButtonPosition = ref({ top: 'auto', left: 'auto', transform: 'none' });

const handleYesPlay = () => {
  stage.value = 1;
};

// DODANA FUNKCJA
const handleYesLove = () => {
  stage.value = 2;
};

const handleNoLove = (event) => {
  const button = event.target;
  const buttonWidth = button.offsetWidth;
  const buttonHeight = button.offsetHeight;

  const scrollX = window.scrollX || window.pageXOffset;
  const scrollY = window.scrollY || window.pageYOffset;

  const viewportWidth = window.innerWidth;
  const viewportHeight = window.innerHeight;

  const maxX = scrollX + viewportWidth - buttonWidth;
  const maxY = scrollY + viewportHeight - buttonHeight;

  const newLeft = scrollX + Math.random() * (viewportWidth - buttonWidth);
  const newTop = scrollY + Math.random() * (viewportHeight - buttonHeight);

  const clampedLeft = Math.max(scrollX, Math.min(newLeft, maxX));
  const clampedTop = Math.max(scrollY, Math.min(newTop, maxY));

  noButtonPosition.value = {
    top: `${clampedTop}px`,
    left: `${clampedLeft}px`,
    position: 'fixed',
    transform: 'none',
  };
};

const noButtonStyle = computed(() => {
  return {
    ...noButtonPosition.value,
    transition: 'top 0.3s ease-out, left 0.3s ease-out',
  };
});
</script>

<template>
  <div class="container">
    <h1 class="main-text" v-if="stage === 0">Laura do you want play?</h1>
    <h1 class="main-text" v-else-if="stage === 1">Do you love me?</h1>
    <h1 class="main-text" v-else-if="stage === 2">Love you too!</h1>

    <div v-if="stage === 0" class="button-group">
      <button class="cute-button yes-button" @click="handleYesPlay">Yes</button>
    </div>

    <div v-else-if="stage === 1" class="button-group">
      <button class="cute-button yes-button green" @click="handleYesLove">Yes</button>
      <button
        class="cute-button no-button red"
        @click="handleNoLove"
        :style="noButtonStyle"
      >
        No
      </button>
    </div>

    <div v-else-if="stage === 2" class="gif-container">
      <img src="./assets/cute-icegif.gif" alt="Cute GIF" class="responsive-gif" />
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Pacifico&display=swap');
body {
  overflow: hidden;
}
.container {
  font-family: 'Pacifico', cursive;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  box-sizing: border-box;
  width: 100%;
  background-color: #ffffff; 
  color: #ffb6c1;
  text-align: center;
}

.main-text {
  font-size: 2.5em;
  margin-bottom: 30px;
  color: #ffb6c1;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
}

.button-group {
  display: flex;
  flex-direction: column;
  gap: 20px;
  position: relative;
  width: 100%;
  align-items: center;
}

.cute-button {
  padding: 15px 30px;
  font-size: 1.5em;
  font-family: 'Pacifico', cursive;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
  transition: transform 0.2s ease-in-out, background-color 0.2s ease-in-out;
  width: 80%;
  max-width: 300px;
}

.cute-button:hover {
  transform: translateY(-3px);
}

.yes-button {
  background-color: #ffb6c1;
  color: white;
}

.yes-button.green {
  background-color: #90ee90;
  color: white;
}

.no-button {
  background-color: #ff6347;
  color: white;
}

.gif-container {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.responsive-gif {
  max-width: 90%;
  height: auto;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
}

@media (min-width: 768px) {
  .main-text {
    font-size: 3.5em;
  }

  .button-group {
    flex-direction: row;
    justify-content: center;
    width: auto;
  }

  .cute-button {
    width: auto;
    min-width: 180px;
  }
}
</style>
