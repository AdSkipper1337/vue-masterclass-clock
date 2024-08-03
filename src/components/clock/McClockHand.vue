<script setup>
import { computed, ref, onMounted, onBeforeUnmount } from "vue";
const props = defineProps({
  position: Number,
  positionMax: Number,
  length: String,
  imgSrc: String,
  imgClass: String,
});

const emit = defineEmits(["update:position"]);

const rotation = computed(
  () => 180 + Math.round((360 * props.position) / props.positionMax) + "deg"
);

const dragging = ref(false);

function startDrag() {
  dragging.value = true;
}

function endDrag() {
  dragging.value = false;
}

const mcClockHandRef = ref();
function mouseDrag(event) {
  if (!dragging.value) return;

  // get the bounding rect of the whole clock which is the parent of our ref
  const rect = mcClockHandRef.value.parentElement.getBoundingClientRect();

  // calculate the relative position of the mouse inside of the clock face
  const x = event.clientX - rect.x;
  const y = event.clientY - rect.y;

  // calculate the position of the mouse relative to the center of teh clock
  const dx = 2 * x - rect.width;
  const dy = 2 * y - rect.height;

  // calculate the angel of the mouse position relative to the center
  const angel = 180 - (Math.atan2(dx, dy) * 180) / Math.PI;

  // normalize and round the angel based on the number of steps we need
  const angelPosition = Math.round((angel * props.positionMax) / 360);

  // emit the updated position - return instead of max to prevent flip over
  if (angelPosition === props.positionMax) emit("update:position", 0);
  else emit("update:position", angelPosition);
}

function touchDrag(event) {
    event.preventDefault();
    event.stopPropagation();
  mouseDrag(event.changedTouches[0]);
}

onMounted(() => {
  document.addEventListener("mouseup", endDrag);
  document.addEventListener("touchend", endDrag);
  document.addEventListener("mousemove", mouseDrag);
  document.addEventListener("touchmove", touchDrag);
});

onBeforeUnmount(() => {
  document.removeEventListener("mouseup", endDrag);
  document.removeEventListener("touchend", endDrag);
  document.removeEventListener("mousemove", mouseDrag);
  document.removeEventListener("touchmove", touchDrag);
});
</script>

<template>
  <div class="mc-clock-hand" ref="mcClockHandRef">
    <img :src="props.imgSrc" :class="props.imgClass" />
    <div
      :class="{
        'mc-clock-hand-tip': true,
        'mc-clock-hand-tip-shining': dragging,
      }"
      @mousedown="startDrag"
      @touchstart="startDrag"
    ></div>
  </div>
</template>

<style scoped>
.mc-clock-hand {
  position: absolute;
  width: 10%;
  height: v-bind(length);
  top: 50%;
  left: 50%;
  transform-origin: top center;
  transform: translate(-50%, 0%) rotate(v-bind(rotation));
  user-drag: none;
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  touch-action: none;
}

.mc-clock-hand-tip {
  position: absolute;
  width: 200%;
  aspect-ratio: 1;
  background-color: yellow;
  bottom: -10%;
  left: -50%;
  border-radius: 50%;
  box-sizing: border-box;
  cursor: pointer;
  opacity: 0;
  filter: blur(2vw);
}

.mc-clock-hand-tip-shining,
.mc-clock-hand-tip:hover {
  transform: scale(1.1);
  opacity: 0.4;
}
</style>
