<script setup>
import { ref } from 'vue'

defineProps({
  user: {
    type: Object,
    required: true,
  },
  phone: {
    type: String,
    required: true,
  },
  company: {
    type: String,
    required: true,
  },
  city: {
    type: String,
    required: true,
  },
})

// Technical Assignment 2 - Problem 1
const detailTerbuka = ref(false)

function toggleDetail() {
  detailTerbuka.value = !detailTerbuka.value
}
</script>

<template>
  <li class="user-card">
    <strong>{{ user.name }}</strong>
    <span>{{ user.email }}</span>

    <button @click="toggleDetail">
      {{ detailTerbuka ? 'Sembunyikan' : 'Lihat Detail' }}
    </button>

    <div v-if="detailTerbuka" class="user-detail">
      <p>Telepon: {{ phone }}</p>
      <p>Perusahaan: {{ company }}</p>
      <p>Kota: {{ city }}</p>
    </div>
  </li>
</template>

<style scoped>
.user-card {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  padding: 0.75rem 1rem;
  border-bottom: 1px solid #e0e0e0;
}

.user-card button {
  align-self: flex-start;
  margin-top: 0.5rem;
}

.user-detail {
  margin-top: 0.5rem;
  padding: 0.75rem;
  background: #eef5f7;
}
</style>
