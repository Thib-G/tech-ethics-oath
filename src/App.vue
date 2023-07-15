<script setup>
import AppHeader from './components/AppHeader.vue'
import AppMarkdownCard from './components/AppMarkdownCard.vue'
import AppCommitsTable from './components/AppCommitsTable.vue'
import AppFooter from './components/AppFooter.vue'

const repo_url = 'https://github.com/virginiemarelli/AI-ethic-oath'
const readme_url = 'https://raw.githubusercontent.com/virginiemarelli/AI-ethic-oath/main/README.md'
const api_url = 'https://api.github.com/repos/virginiemarelli/AI-ethic-oath/commits?sha=main'
</script>

<template>
  <div class="container">
    <AppHeader title="Tech Ethics Oath" :url="repo_url" />
    <AppMarkdownCard title="Text from repo" :markdown-url="readme_url" />
    <AppCommitsTable :api-url="api_url" />
    <AppFooter />
  </div>
</template>
