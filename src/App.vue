<script setup>
import { ref, computed, onMounted } from 'vue'
import { marked } from 'marked'

marked.use({
  async: false,
  pedantic: false,
  gfm: true,
  breaks: true,
  mangle: false,
  headerIds: false,
});

const md = ref("# Loading...")
const commitsRaw = ref([])

const repo_url = ref('https://github.com/virginiemarelli/AI-ethic-oath')
const readme_url = 'https://raw.githubusercontent.com/virginiemarelli/AI-ethic-oath/main/README.md'
const api_url = 'https://api.github.com/repos/virginiemarelli/AI-ethic-oath/commits?sha=main'

const html = computed(() => marked.parse(md.value))
const commits = computed(() => commitsRaw.value.filter((c) => c.commit.message === 'I pledge'))

async function getReadme() {
  const response = await fetch(readme_url)
  md.value = await response.text()
}

async function getApi() {
  const response = await fetch(api_url, {
    method: 'GET',
    headers: {
      Accept: 'application/vnd.github+json'
    }
  })
  commitsRaw.value = await response.json()
}

onMounted(async () => {
  await getReadme()
  await getApi()
})
</script>

<template>
  <div class="container">
    <h1>Tech ethics oath</h1>
    <div class="card mt-4 mb-5">
      <div class="card-header">
        Original <a :href="repo_url">repo</a> on Github
      </div>
      <div class="card-body" v-html="html"></div>
    </div>
    <div class="card mt-4 mb-4">
      <div class="card-header">
        They have pledged
      </div>
      <div class="card-body mb-3">
        <table class="table table-sm">
          <thead>
            <tr>
              <th>Message</th>
              <th>Github author</th>
              <th>Commit author</th>
              <th>Date</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="c in commits" :key="c.sha">
              <td><a :href="c.html_url" target="_blank">{{ c.commit.message }}</a></td>
              <td>{{ c.author?.login }}</td>
              <td>{{ c.commit?.author?.name }}</td>
              <td>{{ (new Date(c.commit?.author?.date)).toLocaleString() }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div class="small mb-2 text-center">
      <a href="https://github.com/Thib-G/tech-ethics-oath">Source</a>
    </div>
  </div>
</template>
