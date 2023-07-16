<script setup>
import { ref, computed, onMounted } from 'vue'
import { marked } from 'marked'
import DOMPurify from 'dompurify'

const props = defineProps({
  title: {
    type: String,
    required: true,
  },
  markdownUrl: {
    type: String,
    required: true,
  }
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

const html = computed(() => DOMPurify.sanitize(marked.parse(md.value)))

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
    <!-- eslint-disable-next-line vue/no-v-html -->
    <div class="card-body" v-html="html"></div>
  </div>
</template>
