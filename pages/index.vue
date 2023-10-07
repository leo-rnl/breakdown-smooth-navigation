<template>

  <NuxtLayout>

    <section class="section">

      <h1 class="section--title">
        <span>Long</span>
        <span>way</span>
        <span>to</span>
        <span>go</span>
      </h1>

      <Nuxt-img src="/spacemonkey.jpg"/>

      <NuxtLink to="/about">About</NuxtLink>

      <div class="section--bg"></div>



    </section>
    <section class="section two">

      <div class="section--bg"></div>
    </section>

  </NuxtLayout>

</template>

<script setup>
import anime from 'animejs/lib/anime.es.js';


definePageMeta({
  pageTransition: {
    name: 'custom-flip',
    mode: 'out-in',
    onBeforeEnter: (el) => {
      console.log('Before enter...')
    },
    onEnter: (el, done) => {},
    onAfterEnter: (el) => {}
  }
})



onMounted(() => {

  /*
  * Anime
  *
  */
  const title = document.querySelector('.section--title')
  const titleParts = document.querySelectorAll('.section--title span')
  const background = document.querySelector('.section--bg')
  const background2 = document.querySelector('.two .section--bg')
  const img = document.querySelector('img')

  anime({
    targets: titleParts,
    opacity: 1,
    translateY:[20, 0],
    duration: 800,
    easing: 'easeInOutSine',
    delay: anime.stagger(100, {start: 500}) // increase delay by 100ms for each elements.
  })

  anime({
    targets: background,
    perspective: {
      value: 1500,
      duration: 1
    },
    rotateX:  {
      value: [0, 5],
      duration: 600,
      easing: 'easeInOutQuad'
    }
  })

   /* On scroll */

  let previousTargetY = 0

  const sizes = {
    width: window.innerWidth,
    height: window.innerHeight
  }

  const handleScroll = () => {

    const scrollY = window.scrollY;

    const normalizeY = scrollY / sizes.height

    // title.style.transform = 'perspective(1500px) rotateX(' + (5 + (40 * (previousTargetY))) +  'deg)'
    title.style.transform = 'perspective(1500px) rotateX(' + (5 + (40 * (previousTargetY))) +  'deg) translate3d(0px, ' + (-400 * (previousTargetY)) +  'px, 0px)'

    img.style.transform = 'perspective(1000px) translateZ(200px) rotateX(' + (5 + (40 * (previousTargetY))) +  'deg) translate3d(0px, ' + (200 - (800 * previousTargetY)) +  'px, 0px)'

    background.style.transform = 'perspective(1500px) rotateX(' + (5 + (40 * (previousTargetY))) +  'deg) translate3d(0px, ' + (-200 * (previousTargetY)) +  'px, 0px)'
    background2.style.transform = 'perspective(1500px) rotateX(' + (-50 + (55 * (previousTargetY))) +  'deg) translate3d(0px, ' + (-200 * (previousTargetY)) +  'px, 0px)'

    previousTargetY = normalizeY


  }

  window.addEventListener('scroll', handleScroll)


})

</script>

<style lang="scss">

  .section{
    @apply relative h-[100vh] w-[100vw] bg-gray-200 flex items-start justify-center;

    img {
      position: absolute;
      z-index: 20;
      transform-style: preserve-3d;
      top: 10vh;
      right: 15vw;
      transform: perspective(1000px) rotateX(5deg) translateZ(200px);
      width: 500px;

      &::after {
        content: '';
        display: block;
        width: 100%;
        height: 500px;
        border: 1px red;
        z-index: 30;
        transform: translate(-50%, -50%);
      }
    }


    &--bg {
      @apply bg-white;

      position: absolute;
      left: 10vh;
      top: 10vw;
      right: 10vw;
      bottom: 10vh;
      z-index: 0;
      transform-style: preserve-3d;
      transform: perspective(1500px) rotateX(5deg) translate3d(0px, 200px, 0px);

    }


    flex-direction: column;

    &--title {
      @apply  px-[20vw];

      transform-style: preserve-3d;
      transform: perspective(1500px) rotateX(5deg);

      font-size: calc(7000vw / 2018);
      display: flex;
      gap: .75rem;
      z-index: 1;

      span {
        display: block;
        opacity: 0;
        font-weight: 100;

        transition: font-weight .3s ease-in-out ;

        &:hover {
          cursor: pointer;
          font-weight: bold;
        }
      }
    }
  }

</style>