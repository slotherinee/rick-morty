<script setup lang="ts">
import { ref, onMounted } from 'vue'
import type { Character } from '@/types/characterTypes'

const props = defineProps<{
  character: Character
}>()

const episode = ref<{ name: string } | null>(null)

const getEpisodeNumber = (arr: string[]) => {
  const episodeRegex = /episode\/(\d+)/
  const match = arr[0].match(episodeRegex)
  return match ? match[1] : null
}

const fetchEpisode = async (episodeNumber: string | null) => {
  if (!episodeNumber) return
  try {
    const response = await fetch(`https://rickandmortyapi.com/api/episode/${episodeNumber}`)
    const data = await response.json()
    episode.value = data
  } catch (error) {
    console.log(error)
  }
}

onMounted(() => {
  const episodeNumber = getEpisodeNumber(props.character.episode)
  fetchEpisode(episodeNumber)
})
</script>

<template>
  <div class="grid grid-cols-3">
    <div class="col-span-1">
      <img :src="character.image" :alt="character.name" class="rounded-l-md h-full" />
    </div>
    <div class="col-span-2 bg-[#3C3E44] rounded-r-md">
      <div class="flex flex-col gap-y-4 py-4 px-4 text-white">
        <div>
          <h2 class="font-bold text-2xl">{{ character.name }}</h2>
          <div class="flex items-center gap-x-2">
            <div
              :class="`w-2 h-2 ${character.status === 'Alive' ? 'bg-green-400' : character.status === 'Dead' ? 'bg-red-400' : 'bg-gray-400'} rounded-full`"
            ></div>
            <p class="text-sm">
              {{ character.status[0].toUpperCase() + character.status.slice(1) }} -
              {{ character.species }}
            </p>
          </div>
        </div>
        <div>
          <p class="text-gray-400 font-medium">Last known location:</p>
          <h2 class="font-medium text-lg">{{ character.location.name }}</h2>
        </div>
        <div>
          <p class="text-gray-400 font-medium">First seen in:</p>
          <h2 class="font-medium text-lg">{{ episode?.name || 'Pilot' }}</h2>
        </div>
      </div>
    </div>
  </div>
</template>
