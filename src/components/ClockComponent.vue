<template>
  <div class="clock">
    <div class="numbers">
      <div class="number number-12">12</div>
      <div class="number number-3">3</div>
      <div class="number number-6">6</div>
      <div class="number number-9">9</div>
    </div>
    <div ref="hourHand" class="hand hour-hand"></div>
    <div ref="minuteHand" class="hand minute-hand"></div>
    <div ref="secondHand" class="hand second-hand"></div>
  </div>
</template>

<script setup>
import {ref, onMounted} from 'vue';

const HOURS_ON_CLOCK = 12;
const UPDATE_INTERVAL = 1000;
const HOUR_DEGREE = 30;
const MINUTE_DEGREE = 6;
const SECOND_DEGREE = 6;
const HOUR_OFFSET = 0.5;
const FULL_CIRCLE = 360;
const MINUTES_IN_HOUR = 60;

const hourHand = ref(null);
const minuteHand = ref(null);
const secondHand = ref(null);

function setClock() {
  const now = new Date();
  const hours = now.getHours();
  const minutes = now.getMinutes();
  const seconds = now.getSeconds();

  const hourAngle = (hours % HOURS_ON_CLOCK) * HOUR_DEGREE + minutes * HOUR_OFFSET;
  const minuteAngle = minutes * MINUTE_DEGREE;
  const secondAngle = seconds * SECOND_DEGREE;

  hourHand.value.style.transform = `rotate(${hourAngle}deg)`;
  minuteHand.value.style.transform = `rotate(${minuteAngle}deg)`;
  secondHand.value.style.transform = `rotate(${secondAngle}deg)`;
}

function clockAngle(hours, minutes) {
  const hourAngle = (hours % HOURS_ON_CLOCK) * HOUR_DEGREE + (minutes / MINUTES_IN_HOUR) * HOUR_DEGREE;
  const minuteAngle = minutes * MINUTE_DEGREE;
  let angle = Math.abs(hourAngle - minuteAngle);
  return Math.min(angle, FULL_CIRCLE - angle);
}

console.log(clockAngle(3, 15));
console.log(clockAngle(12, 30));

onMounted(() => {
  setClock();
  setInterval(setClock, UPDATE_INTERVAL);
});
</script>

<style>
</style>