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
  transform: translate(-50%, -100%) rotate(calc(v-bind(digit) * 30deg))
    translate(0%, -100%);
  background-color: black;
  user-drag: none;
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}

.mc-clock-digit-container {
  position: absolute;
  width: 1%;
  height: 50%;
  top: 50%;
  left: 50%;
  transform-origin: bottom center;
  transform: translate(-50%, -100%) rotate(calc(v-bind(digit) * 30deg));
  user-drag: none;
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}

.mc-clock-digit {
  position: absolute;
  font-family: Arial, Helvetica, sans-serif;
  font-size: calc(min(7.6vw, 7.6vh));
  width: calc(min(10vw, 10vh));
  height: calc(min(10vw, 10vh));
  line-height: calc(min(10vw, 10vh));
  text-align: center;
  background-color: darkgrey;
  border-radius: 50%;
  border-width: 1px;
  border-style: solid;
  transform: translate(-50%, 0%) rotate(calc(v-bind(digit) * -30deg));
}
</style>
