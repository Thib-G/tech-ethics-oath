<script setup>
defineProps({
  title: String,
  url: String
})
</script>
<template>
  <header class="d-flex justify-content-between align-items-md-center pb-3 mt-3 mb-5 border-bottom">
    <h1 class="h4">
      <a href="/" class="d-flex align-items-center text-dark text-decoration-none">
        <span>
          {{ title }}
        </span>
      </a>
    </h1>
    <a :href="url" target="_blank" rel="noopener">View on GitHub</a>
  </header>
</template>
