<script setup lang="ts">
import { onMounted, ref } from 'vue'
import CharacterCard from './components/CharacterCard.vue'
import FilterForm from './components/FilterForm.vue'
import PaginationWidget from './components/PaginationWidget.vue'
import type { Character } from './types/characterTypes'

const currentPage = ref(1)
const characters = ref<null | Character[]>(null)
const filteredCharacters = ref<null | Character[]>(null)
const error = ref<string | null>(null)
const loading = ref(false)

const getCharacters = async () => {
  try {
    loading.value = true
    const response = await fetch(
      `https://rickandmortyapi.com/api/character/?page=${currentPage.value}`
    )
    const data = await response.json()
    characters.value = data.results
    filteredCharacters.value = data.results
  } catch (err) {
    console.log(err)
    error.value = (err as Error).message
  } finally {
    loading.value = false
  }
}

const filterCharacters = (filterData: { name: string; status: string }) => {
  filteredCharacters.value = characters.value.filter((character) => {
    return (
      character.name.toLowerCase().includes(filterData.name.toLowerCase()) &&
      character.status.toLowerCase().includes(filterData.status.toLowerCase())
    )
  })
}

const changePage = (direction: 'next' | 'prev') => {
  if (direction === 'next') {
    currentPage.value++
  } else if (direction === 'prev' && currentPage.value > 1) {
    currentPage.value--
  }
  getCharacters()
}

onMounted(getCharacters)
</script>

<template>
  <div class="antialiased flex items-center justify-center min-h-screen">
    <div v-if="!error">
      <div v-if="loading">
        <div>
          <h2>Loading data...</h2>
        </div>
      </div>
      <div class="flex flex-col gap-y-4 items-center w-full">
        <FilterForm @filter="filterCharacters" />
        <PaginationWidget :currentPage="currentPage" @changePage="changePage" />
        <div v-auto-animate class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <CharacterCard
            v-for="character in filteredCharacters"
            :key="character.id"
            :character="character"
          />
        </div>
      </div>
    </div>
    <div class="text-2xl text-red-500 font-bold" v-else>
      {{ error }}
    </div>
  </div>
</template>
