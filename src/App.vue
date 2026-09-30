<script setup>
import { ref, computed, watch } from 'vue'
import UserCard from './components/UserCard.vue'

const judul = 'Dashboard Vue — Pertemuan 5'
const users = ref([])
const keadaan = ref('idle') // idle | loading | empty | error | success

async function muatPengguna() {
  keadaan.value = 'loading'

  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')

    if (!response.ok) {
      throw new Error('Status HTTP: ' + response.status)
    }

    const data = await response.json()

    if (data.length === 0) {
      keadaan.value = 'empty'
      return
    }

    users.value = data
    keadaan.value = 'success'
  } catch (error) {
    console.error('Gagal memuat data:', error)
    keadaan.value = 'error'
  }
}

const queryPencarian = ref('')
const riwayatPencarian = ref([])

const penggunaTersaring = computed(() => {
  const q = queryPencarian.value.toLowerCase().trim()

  if (!q) return users.value

  return users.value.filter((user) => user.username.toLowerCase().includes(q))
})

watch(queryPencarian, (nilaiBaru) => {
  if (nilaiBaru.trim() !== '') {
    riwayatPencarian.value.push(nilaiBaru)
  }
})

// Exercise A dari Session 5
const jumlahHasil = computed(() => penggunaTersaring.value.length)

// Technical Assignment 2 - Problem 2
const arahUrutan = ref('asc')

const penggunaTerurut = computed(() => {
  const hasil = [...penggunaTersaring.value]

  if (arahUrutan.value === 'asc') {
    return hasil.sort((a, b) => a.name.localeCompare(b.name))
  }

  return hasil.sort((a, b) => b.name.localeCompare(a.name))
})

function ubahUrutan(arah) {
  arahUrutan.value = arah
}
</script>

<template>
  <header class="app-header">
    <h1>{{ judul }}</h1>
  </header>

  <main class="app-main">
    <button @click="muatPengguna">Muat Pengguna</button>
    <input v-model="queryPencarian" type="text" placeholder="Cari username..." />

    <button
      :class="{ active: arahUrutan === 'asc' }"
      @click="ubahUrutan('asc')"
    >
      Urutkan A-Z
    </button>
    <button
      :class="{ active: arahUrutan === 'desc' }"
      @click="ubahUrutan('desc')"
    >
      Urutkan Z-A
    </button>

    <p v-if="keadaan === 'loading'">Memuat data...</p>
    <p v-else-if="keadaan === 'empty'">Tidak ada pengguna ditemukan.</p>
    <p v-else-if="keadaan === 'error'">Gagal memuat data. Coba lagi.</p>

    <template v-else-if="keadaan === 'success'">
      <p>Menampilkan {{ jumlahHasil }} dari {{ users.length }} pengguna</p>

      <ul>
        <UserCard
          v-for="user in penggunaTerurut"
          :key="user.id"
          :user="user"
          :phone="user.phone"
          :company="user.company.name"
          :city="user.address.city"
        />
      </ul>
    </template>
  </main>
</template>

<style scoped>
.app-header {
  padding: 1.5rem;
  background: #0b4f6c;
  color: white;
}

.app-main {
  padding: 1.5rem;
}

button.active {
  background: #0b4f6c;
  color: white;
}
</style>
