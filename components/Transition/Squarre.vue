<script setup>
// JavaScript hooks logic...

const onEnter = () => {
  console.log('onEnter')
}

const onLeave = () => {
  console.log('onLeave')
}
</script>

<template>
  <!-- wrap the built-in Transition component -->
  <Transition
      name="my-transition"
      :css="false"
      @enter="onEnter"
      @leave="onLeave">
    <slot></slot> <!-- pass down slot content -->
  </Transition>
</template>

<style>
/*
  Necessary CSS...
  Note: avoid using <style scoped> here since it
  does not apply to slot content.
*/
</style>