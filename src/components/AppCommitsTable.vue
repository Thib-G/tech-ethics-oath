<script setup>
import { ref, computed, onMounted } from 'vue'

const props = defineProps({
  apiUrl: {
    type: String,
    required: true
  }
})

const commitsRaw = ref([])
const commits = computed(() => commitsRaw.value.filter((c) => c.commit.message === 'I pledge'))

async function getApi() {
  const response = await fetch(props.apiUrl, {
    method: 'GET',
    headers: {
      Accept: 'application/vnd.github+json'
    }
  })
  commitsRaw.value = await response.json()
}
onMounted(async () => {
  await getApi()
})
</script>

<template>
  <div class="card mt-4 mb-4">
    <div class="card-header">They have pledged</div>
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
            <td>
              <a :href="c.html_url" target="_blank">{{ c.commit.message }}</a>
            </td>
            <td>{{ c.author?.login }}</td>
            <td>{{ c.commit?.author?.name }}</td>
            <td>{{ new Date(c.commit?.author?.date).toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
