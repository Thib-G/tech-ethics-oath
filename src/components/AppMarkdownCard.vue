<script setup>
import { ref, computed, onMounted } from 'vue'
import { marked } from 'marked'

const props = defineProps({
  title: String,
  markdownUrl: String
})

marked.use({
  async: false,
  pedantic: false,
  gfm: true,
  breaks: true,
  mangle: false,
  headerIds: false
})

const md = ref('# Loading...')

const html = computed(() => marked.parse(md.value))

async function getMd() {
  const response = await fetch(props.markdownUrl)
  md.value = await response.text()
}

onMounted(async () => {
  await getMd()
})
</script>

<template>
  <div class="card mt-4 mb-5">
    <div class="card-header">{{ title }}</div>
    <div class="card-body" v-html="html"></div>
  </div>
</template>
