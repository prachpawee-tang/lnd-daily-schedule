<!-- fix: task title position -->
<!-- fix: hand move backward when change back to 0 -->

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const MOCK_SCHEDULE = [
  { start: 22, end: 5, title: 'sleep' },
  { start: 5, end: 6, title: 'wake up' },
  { start: 6, end: 8, title: 'breakfast' },
  { start: 8, end: 12, title: 'work' },
  { start: 12, end: 17, title: 'lunch' },
  { start: 17, end: 20, title: 'work' },
  { start: 20, end: 22, title: 'dinner' },
]

const deg = 6
const hourRotation = ref(0)
const minRotation = ref(0)
const secRotation = ref(0)
const schedule = ref(MOCK_SCHEDULE)
let clockInterval = null

const setClock = () => {
  const day = new Date()
  hourRotation.value = day.getHours() * 15 + (day.getMinutes() / 60) * 15
  minRotation.value = day.getMinutes() * deg + (day.getSeconds() / 60) * deg
  secRotation.value = day.getSeconds() * deg
}

onMounted(() => {
  setClock()
  clockInterval = setInterval(setClock, 1000)
})

onBeforeUnmount(() => {
  clearInterval(clockInterval)
})
</script>

<template>
  <div class="clock">
    <div class="clock-inner">
      <div class="clock-pivot" />
      <div
        class="hour-hand"
        :class="{ 'no-transition': hourRotation === 0 }"
        :style="{ transform: `rotateZ(${hourRotation}deg)` }"
      ></div>
      <div
        class="minute-hand"
        :class="{ 'no-transition': minRotation === 0 }"
        :style="{ transform: `rotateZ(${minRotation}deg)` }"
      ></div>
      <!-- <div
        class="second-hand"
        :class="{ 'no-transition': secRotation === 0 }"
        :style="{ transform: `rotateZ(${secRotation}deg)` }"
      ></div> -->
    </div>

    <div
      class="hour-mark"
      v-for="i in 24"
      :key="`hour-mark-${i}`"
      :style="{
        transform: `rotate(${i * 15}deg) translateY(-50%)`,
      }"
    ></div>

    <div class="clock-numbers" v-for="i in 24" :key="`clock-numbers-${i}`">
      <span :class="`h${i}`">{{ i }}</span>
    </div>

    <div class="clock-schedule" v-for="task in schedule" :key="task.title">
      <div
        class="schedule-start-mark"
        :style="{
          transform: `rotate(${task.start * 15}deg) translateY(20px)`,
        }"
      ></div>
      <div
        class="schedule-title"
        :style="{ transform: `rotate(${((task.start + task.end) / 2) * 15 - 90}deg)` }"
      >
        <span>
          {{ task.title }}
        </span>
      </div>
      <div
        class="schedule-end-mark"
        :style="{
          transform: `rotate(${task.end * 15}deg) translateY(20px)`,
        }"
      ></div>
    </div>
  </div>
</template>

<style scoped>
.clock-inner {
  width: 100%;
  aspect-ratio: 1;
  border-radius: 50%;
  border: 3px solid #000;
  margin: 3px;
  position: relative;

  display: flex;
  align-items: center;
  justify-content: center;
}

.hour-hand,
.minute-hand,
.second-hand {
  position: absolute;
  display: flex;
  justify-content: center;
  border-radius: 50%;

  transition: all 1s linear;
}

.no-transition {
  transition: ease-in-out 1s linear;
}

.hour-hand {
  height: 30%;
  aspect-ratio: 1;
}
.hour-hand:before {
  content: '';
  position: absolute;
  height: 50%;
  width: 6px;
  background-color: #000;
  border-radius: 6px;
}
.minute-hand {
  height: 60%;
  aspect-ratio: 1;
}
.minute-hand:before {
  content: '';
  height: 50%;
  width: 4px;
  background-color: #000;
  border-radius: 4px;
}
.second-hand {
  height: 13em;
  width: 13em;
}
.second-hand:before {
  content: '';
  height: 60%;
  width: 2px;
  background-color: #f00;
  border-radius: 2px;
}

.clock {
  height: 100%;
  max-width: 70dvw;
  max-height: 50dvh;
  aspect-ratio: 1 !important;
  margin: 8% auto 0;
  border-radius: 50%;
  position: relative;
  border: 1.5px solid #000;
  display: flex;
  align-items: center;
  justify-content: center;
}
.clock-pivot {
  width: 5%;
  aspect-ratio: 1;
  border-radius: 50%;
  background-color: #000;
}

.hour-mark {
  position: absolute;
  width: 2px;
  height: 12px;
  background-color: #000;
  transform-origin: 0% min(35.1vw, 200px);
  top: 0;
  left: 50%;
  border-radius: 2px;
}
.hour-mark:nth-of-type(6n + 1) {
  width: 4px;
  height: 15px;
}

.schedule-start-mark,
.schedule-end-mark {
  position: absolute;
  width: 1px;
  height: 40%;
  background-color: #000;
  transform-origin: 0% min(35.1vw, 200px);
  top: 0;
  left: 50%;
  border-radius: 2px;
}

.schedule-title {
  position: absolute;
  width: 50%;
  color: #000;
  top: calc(50% - 10px);
  left: 50%;
  margin-left: 20px;
  text-align: center;
  transform-origin: 0% 0%;
}

.clock-numbers {
  color: #000;
}

.clock-numbers span {
  position: absolute;
}

.h1 {
  top: -6%;
  right: 34%;
}
.h2 {
  top: -1%;
  right: 21%;
}
.h3 {
  top: 9%;
  right: 9%;
}
.h4 {
  top: 20%;
  right: 1%;
}
.h5 {
  top: 33%;
  right: -4%;
}
.h6 {
  top: 47%;
  right: -7%;
}
.h7 {
  top: 62%;
  left: 102%;
}
.h8 {
  top: 75%;
  left: 96%;
}
.h9 {
  top: 86%;
  left: 87%;
}
.h10 {
  top: 95%;
  left: 75%;
}
.h11 {
  top: 100%;
  left: 62%;
}
.h12 {
  top: 102%;
  left: 47%;
}
.h13 {
  top: 100%;
  left: 33%;
}
.h14 {
  top: 95%;
  left: 20%;
}
.h15 {
  top: 86%;
  left: 9%;
}
.h16 {
  top: 75%;
  left: 0%;
}
.h17 {
  top: 62%;
  left: -6%;
}
.h18 {
  top: 46%;
  left: -8%;
}
.h19 {
  top: 33%;
  left: -6%;
}
.h20 {
  top: 19%;
  left: 0%;
}
.h21 {
  top: 8%;
  left: 9%;
}
.h22 {
  top: -1%;
  left: 20%;
}
.h23 {
  top: -6%;
  left: 33%;
}
.h24 {
  top: -8%;
  left: 48%;
}
</style>
