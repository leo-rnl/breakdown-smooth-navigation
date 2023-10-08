<template>
    <NuxtLayout>
      <section class="section">
        <div class="section--bg"></div>
        <div class="z-99">
          <h1>About</h1>
          <NuxtLink to="/">Home</NuxtLink>
        </div>
      </section>
    </NuxtLayout>
</template>


<script setup>

import anime from "animejs";
import {onBeforeRouteLeave} from "#app";

onMounted(() => {

  const title2 = document.querySelector('.z-99')
  const bg = document.querySelector('.section--bg')

  anime({
    targets: bg,
    scale: [1.5, 1],
    duration: 1200,
    easing: 'easeInOutQuad'
  })

  anime({
    targets: title2,
    opacity: [0, 1],
    duration: 1200,
    easing: 'easeInOutQuad'
  })

})

onBeforeRouteLeave((from, to, next) => {

  anime({
    targets: '.z-99',
    opacity: [1, 0],
    duration: 500,
    easing: 'easeInOutQuad',
    complete: () => next()
  })

})

</script>

<style lang="scss">

.section {
  @apply relative h-[100vh] w-[100vw] bg-gray-200 flex items-center justify-center;

  & > div {
    z-index: 10;
  }

  display: flex !important;
  &--bg {
    @apply bg-white;

    position: absolute;
    left: 10vh;
    top: 10vw;
    right: 10vw;
    bottom: 10vh;
    z-index: 0;


  }
}

</style>