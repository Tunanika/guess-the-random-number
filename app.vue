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
    <div class="text-2xl mt-4 text-purple-500">
      Current Level: {{ currentLevel }}
    </div>
    <div class="text-sm text-gray-500 mb-4">
      Speed increases every 5 seconds!
    </div>
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
console.log("Haha the console cannot help you mate");
let actuallyRandomNumber: Ref<number> = ref(
  Math.floor(Math.random() * 100) + 1
);

const randomNumber = ref(0);
let intervalId: ReturnType<typeof setInterval> | undefined;
// I shall scrap this game it's not fun
const baseInterval = ref(500); // Start at 0.5 seconds
const minInterval = 50; // Fastest speed (0.05 seconds)
const speedIncreaseRate = 0.8; // Reduce interval by 20% each level
const currentLevel = ref(1);
const isGameActive = ref(false);

const startRandomNumber = () => {
  isGameActive.value = true;
  const updateNumber = () => {
    randomNumber.value = Math.floor(Math.random() * 100) + 1;
  };

  const progressDifficulty = () => {
    if (isGameActive.value) {
      currentLevel.value++;

      // Calculate new interval (speed up)
      const newInterval = Math.max(
        minInterval,
        baseInterval.value * Math.pow(speedIncreaseRate, currentLevel.value - 1)
      );

      // Clear existing interval and start new one with increased speed
      if (intervalId) {
        clearInterval(intervalId);
      }
      intervalId = setInterval(updateNumber, newInterval);
    }
  };

  // Start initial interval
  intervalId = setInterval(updateNumber, baseInterval.value);

  // Increase difficulty every 15 seconds
  const progressionInterval = setInterval(progressDifficulty, 15000);
};

const stopRandomNumber = () => {
  isGameActive.value = false;
  if (intervalId) {
    clearInterval(intervalId);
  }
  if (actuallyRandomNumber.value === randomNumber.value) {
    alert(`Congratulations! You made it to level ${currentLevel.value}!`);
  }
};

const restartRandomNumber = () => {
  currentLevel.value = 1;
  if (intervalId) {
    clearInterval(intervalId);
  }
  randomNumber.value = 0;
  actuallyRandomNumber.value = Math.floor(Math.random() * 100) + 1;
  startRandomNumber();
};

onMounted(() => {
  startRandomNumber();
});

onUnmounted(() => {
  if (intervalId) {
    clearInterval(intervalId);
  }
});
</script>
