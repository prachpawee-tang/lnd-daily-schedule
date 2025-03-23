<script setup></script>

<template>
  <div class="clock"></div>
</template>

<style scoped>
.clock {
  height: 100%;
  max-width: 70dvw; /* Maximum width is 70% of viewport width */
  max-height: 50dvh; /* Maximum height is 50% of viewport height */
  aspect-ratio: 1 !important; /* Ensures perfect circle */
  margin: 8% auto 0; /* Centered with top margin */
  border-radius: 50%; /* Circular shape */
  position: relative;
  border: 2px solid #000;
}
</style>
