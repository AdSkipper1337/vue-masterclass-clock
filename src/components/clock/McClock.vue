<script setup>
import McClockHand from "./McClockHand.vue";
import McClockDigit from "./McClockDigit.vue";
import { ref, computed, onMounted, onBeforeUnmount } from "vue";

const currentDate = ref(new Date());
const hours = computed({
  get: () => currentDate.value.getHours(),
  set: (value) => {
    if (value === "") return;
    const date = currentDate.value;
    date.setHours(value);
    currentDate.value = new Date(date);
    tick.value = false;
  },
});
const minutes = computed({
  get: () => currentDate.value.getMinutes(),
  set: (value) => {
    if (value === "") return;
    const date = currentDate.value;
    date.setMinutes(value);
    currentDate.value = new Date(date);
    tick.value = false;
  },
});
const seconds = computed({
  get: () => currentDate.value.getSeconds(),
  set: (value) => {
    if (value === "") return;
    const date = currentDate.value;
    date.setSeconds(value);
    currentDate.value = new Date(date);
    tick.value = false;
  },
});
const tick = ref(true);

function updateCurrentDate() {
  if (tick.value) currentDate.value = new Date();
  getNextTick();
}
let timeout = null;
function getNextTick() {
  const milliseconds = new Date().getMilliseconds();
  timeout = setTimeout(updateCurrentDate, 1000 - milliseconds);
}

onMounted(() => {
  getNextTick();
});
onBeforeUnmount(() => {
  clearTimeout(timeout);
});

const clockMargin = ref("10%");
</script>

<template>
  <div class="mc-clock">
    <div>CLOCK</div>
    <div>
      <label><input v-model="tick" type="checkbox" /> Tick </label>
    </div>
    <input v-model="hours" type="number" min="0" max="24" />
    <input v-model="minutes" type="number" min="0" max="60" />
    <input v-model="seconds" type="number" min="0" max="60" />

    <div class="mc-clock-area" ref="mcClockAreaRef">
      <McClockDigit
        v-for="index in 12"
        :key="indes"
        :digit="index"
      ></McClockDigit>

      <McClockHand
        v-model:position="hours"
        :positionMax="12"
        length="41%"
        imgSrc="hours.svg"
        imgClass="mc-clock-hand-hours"
      />
      <McClockHand
        v-model:position="minutes"
        :positionMax="60"
        length="48%"
        imgSrc="minutes.svg"
        imgClass="mc-clock-hand-minutes"
      />
      <McClockHand
        v-model:position="seconds"
        :positionMax="60"
        length="55%"
        imgSrc="seconds.svg"
        imgClass="mc-clock-hand-seconds"
      />
    </div>
  </div>
</template>

<style scoped>
.mc-clock {
  overflow: hidden;
  background-color: beige;
}

.mc-clock-area {
  margin: v-bind(clockMargin);
  position: relative;
  width: calc(100% - v-bind(clockMargin) * 2);
  aspect-ratio: 1;
  background-color: gray;
  border-radius: 50%;
  border-width: 1vw;
  border-style: solid;
  border-color: balck;
  box-sizing: border-box;
}
</style>
