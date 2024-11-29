<template>
  <div class="w-full h-full flex flex-col items-center justify-center">
    <h1 class="text-7xl mt-20 text-amber-500">GUESS THE RANDOM NUMBER!</h1>
    <h2 class="text-3xl mt-16 text-green-400">
      Well normal random guessers are way too easy. But with this, you will
      almost <a class="text-amber-500">never win</a>
    </h2>
    <h1 class="text-7xl mt-24">
      Can you stop it at
      <a class="text-amber-600">{{ actuallyRandomNumber }}</a>
    </h1>
    <div class="text-5xl mt-10">{{ randomNumber }}</div>
    <UMeter :value="randomNumber" indicator class="w-1/2 mt-12" />
    <button
      @click="stopRandomNumber"
      class="mt-5 px-4 py-2 bg-blue-500 text-white rounded"
    >
      Stop
    </button>
    <button
      @click="restartRandomNumber"
      class="mt-5 px-4 py-2 bg-green-500 text-white rounded"
    >
      Restart
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
console.log("Haha the console cannot help you mate");
let actuallyRandomNumber: Ref<number> = ref(
  Math.floor(Math.random() * 100) + 1
);

const randomNumber = ref(0);
let intervalId: number | undefined;

const generateRandomNumber = () => {
  randomNumber.value = Math.floor(Math.random() * 100) + 1;
};

const stopRandomNumber = () => {
  if (actuallyRandomNumber.value === randomNumber.value) {
    alert("Eyy you thought something was gonna happen lol that's funny");
  }
  if (intervalId) {
    clearInterval(intervalId);
  }
};
const checkRandomNumber = () => {
  if (randomNumber.value === actuallyRandomNumber.value) {
    alert("You guessed the number!");
    stopRandomNumber();
  }
};

const restartRandomNumber = () => {
  stopRandomNumber();
  if (intervalId) {
    actuallyRandomNumber.value = Math.floor(Math.random() * 100) + 1;
    intervalId = setInterval(generateRandomNumber, 100);
  }
};

onMounted(() => {
  intervalId = setInterval(generateRandomNumber, 100);
});

onUnmounted(() => {
  if (intervalId) {
    clearInterval(intervalId);
  }
});
</script>
