<script setup lang="ts">
import anime from 'animejs';
import { onMounted } from 'vue';

onMounted(() => {
  if (!window.matchMedia('(prefers-color-scheme: dark)').matches) {
    anime({
      targets: ['#blue1', '#pink1', '#white', '#pink2', '#blue2'],
      width: '100%',
      easing: 'easeInOutExpo',
      delay: (_el, i, _l) => {
        return i * 50;
      },
      direction: 'alternate',
      loop: true
    });
  }
});
</script>

<template>
  <div id="transgender-flag">
    <div id="blue1"></div>
    <div id="pink1"></div>
    <div id="white"></div>
    <div id="pink2"></div>
    <div id="blue2"></div>
  </div>
</template>

<style scoped>
#transgender-flag {
  position: absolute;
  left: 0;
  top: 0;
  width: 100vw;
  z-index: -1;
  div {
    flex-grow: 1;
    width: 0;
    height: 20vh;
  }
}

#blue1,
#blue2 {
  background-color: #5bcefa;
}
#pink1,
#pink2 {
  background-color: #f5a9b8;
}
</style>
