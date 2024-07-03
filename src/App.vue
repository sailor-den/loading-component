<template>
  <div id="app">
    <CircleProgress :MaxValue="108" :Value="36" :TargetValue="80" />
  </div>
</template>

<script>
import CircleProgress from './components/CircleProgress.vue';

export default {
  name: 'App',
  components: {
    CircleProgress
  }
}
</script>

<style>
@import './assets/styles.css';
</style>
