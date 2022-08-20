<template>
  <div class="flex flex-col min-h-screen font-roboto bg-weather-primary">
    <Header />
    <RouterView />
  </div>
</template>

<script setup lang="ts">
import { RouterView } from "vue-router";
import Header from "./components/Header.vue";
</script>