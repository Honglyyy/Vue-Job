<script setup>
  import Navbar from './components/Navbar.vue';  
  import { RouterView } from 'vue-router';
</script>

<template>
  <Navbar/>
  <!-- Prop(propities): data the passes thru -->
  <!-- <Hero title="Hello World" subtitle="KDMH"/> -->
  <RouterView/>
</template>