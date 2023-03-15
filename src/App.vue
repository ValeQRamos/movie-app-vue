<script setup>
import Movies from "./components/Movies.vue";
</script>

<template>
  <Suspense>
    <template #default>
      <Movies />
    </template>
    <template #fallback>
      <h1>Loading movies...</h1>
    </template>
  </Suspense>
</template>
