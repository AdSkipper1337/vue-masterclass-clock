<script setup>
const props = defineProps({
  digit: Number,
});
</script>

<template>
  <div class="mc-clock-digit-line"></div>
  <div class="mc-clock-digit-container">
    <div class="mc-clock-digit">
      {{ digit }}
    </div>
  </div>
</template>

<style scoped>
.mc-clock-digit-line {
  position: absolute;
  width: 1%;
  height: 20%;
  top: 50%;
  left: 50%;
  transform-origin: bottom center;
  transform: translate(-50%, -100%) rotate(calc(v-bind(digit) * 30deg)) translate(0%, -100%);
  background-color: black;
}

.mc-clock-digit-container {
  position: absolute;
  width: 1%;
  height: 50%;
  top: 50%;
  left: 50%;
  transform-origin: bottom center;
  transform: translate(-50%, -100%) rotate(calc(v-bind(digit) * 30deg));
}

.mc-clock-digit {
  position: absolute;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 7.6vw;
  width: 10vw;
  height: 10vw;
  line-height: 10vw;
  text-align: center;
  background-color: darkgrey;
  border-radius: 50%;
  border-width: 1px;
  border-style: solid;
  transform: translate(-50%, 0%) rotate(calc(v-bind(digit) * -30deg));
}
</style>
