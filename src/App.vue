<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import { useMessageStore } from './stores/message';
import { storeToRefs } from 'pinia';
const store = useMessageStore()
const { message } = storeToRefs(store)
</script>

<template>
  <header class="max-h-screen leading-normal">
    <div id="flashMessage" v-if="message" class="animate-yellowfade">
    <h4 class="text-lg">{{ message }}</h4>
    <h1>SORA</h1>
    </div>
    <nav class="p-8">
      <RouterLink :to="{name: 'EventList'}" class="font-bold text-gray-700 hover:text-green-500 m-4">Home</RouterLink>
      <RouterLink :to="{name: 'about'}" class="font-bold text-gray-700 hover:text-green-500 m-4">About</RouterLink>
      <RouterLink :to="{name: 'Component'}" class="font-bold text-gray-700 hover:text-green-500 m-4">Component</RouterLink>
      <RouterLink :to="{name: 'Student'}" class="font-bold text-gray-700 hover:text-green-500 m-4">Student</RouterLink>
    </nav>
  </header>

  <RouterView />
</template>

<style>
#app {
  front-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-front-smoothing: antialiased;
  -moz-osx-front-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

@keyframes yellowfade {
  from {
    background: yellow;
  }
  to {
    background: transparent;
  }
}

.animate-yellowfade {
  animation: yellowfade 3s ease-in-out
}
</style>
