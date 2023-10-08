<template>
   <NuxtLayout>
        <section class="section">
          <h1 class="section--title -py-2">
            <span>Long</span>
            <span>way</span>
            <span>to</span>
            <span>go</span>
          </h1>
          <Nuxt-img src="/spacemonkey.jpg"/>
          <div class="section--bg"></div>
        </section>
        <section class="section two">
          <div class="section--bg z-0 pointer-events-none"></div>
          <NuxtLink class="z-10 block" to="/about">About</NuxtLink>
        </section>
   </NuxtLayout>
</template>

<script setup>
import anime from 'animejs/lib/anime.es.js';
import {onBeforeRouteLeave} from "#app";

onMounted(() => {
  animSetup()
})

const animSetup = () => {
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
    translateY:[100, 0],
    duration: 300,
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

  anime({
    targets: img,
    opacity: [0, 1],
    translateY: [-500, 0],
    duration: 500,
    easing: 'easeInOutQuad'
  })

  /* On scroll */

  const sizes = {
    width: window.innerWidth,
    height: window.innerHeight
  }

  const handleScroll = () => {

    const scrollY = window.scrollY;
    const normalizeY = scrollY / sizes.height

    // Apply transforms
    title.style.transform = 'perspective(1500px) rotateX(' + (5 + (40 * (normalizeY))) +  'deg) translate3d(0px, ' + (-400 * (normalizeY)) +  'px, 0px)'
    img.style.transform = 'perspective(1000px) translateZ(200px) rotateX(' + (5 + (40 * (normalizeY))) +  'deg) translate3d(0px, ' + (200 - (800 * normalizeY)) +  'px, 0px)'
    background.style.transform = 'perspective(1500px) rotateX(' + (5 + (40 * (normalizeY))) +  'deg) translate3d(0px, ' + (-200 * (normalizeY)) +  'px, 0px)'
    background2.style.transform = 'perspective(1500px) rotateX(' + (-50 + (55 * (normalizeY))) +  'deg) translate3d(0px, ' + (-200 * (normalizeY)) +  'px, 0px)'

  }

  window.addEventListener('scroll', handleScroll)

}

onBeforeRouteLeave((to, from, next) => {

  anime({
    targets: document.querySelector('.two .section--bg'),
    scale: 1.5,
    duration: 1200,
  })

  anime({
    targets: document.querySelector('.z-10'),
    opacity: 0,
    duration: 800,
    complete: () => next()
  })

})

</script>

<style lang="scss" scoped>

  .section.two {
    @apply items-center;
  }

  .section{
    @apply relative h-[100vh] w-[100vw] bg-gray-200 flex items-start justify-center;

    img {
      position: absolute;
      z-index: 20;
      transform-style: preserve-3d;
      top: 10vh;
      right: 15vw;
      transform: perspective(1000px) translateZ(200px) rotateX(5deg) translate3d(0px, 200px, 0px);
      width: 500px;

      &::after {
        content: '';
        display: block;
        width: 100%;
        height: 500px;
        border: 1px red;
        z-index: 30;
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
      overflow: hidden;

      font-size: calc(7000vw / 2018);
      display: flex;
      gap: .75rem;
      z-index: 1;

      span {
        display: block;

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