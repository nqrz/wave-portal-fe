<script setup>
defineProps({
  waves: Array
})

const formatDate = (date) => {
  const options = { year: 'numeric', month: 'long', day: 'numeric' }
  return new Date(date).toLocaleDateString('en', options)
}
</script>

<template>
  <h2 style="text-align: center;">
    Waves
  </h2>
  <div class="container" style="background-color: aqua;">
    <div class="grid">
      <div v-for="wave in waves" class="card">
        <h6 style="margin: 0;">
          {{ formatDate(wave.timestamp) }}
        </h6>
        <p style="margin: 0; margin-top: 8px;">
          Sender : {{ wave.address.slice(0, 14) }}...
        </p>
        <h4>
          {{ wave.message }}
        </h4>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  background-color: aliceblue;
  margin: 10px;
  padding: 14px;
  word-wrap: break-word;
  border-radius: 6px;
  max-width: 675px;
}

.grid {
  width: 720px;
  margin-top: 22px;
  margin-bottom: 22px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
}
</style>