<script setup lang="ts">
defineProps<{
  currentPage: number
}>()

const emit = defineEmits(['prev', 'next'])
</script>

<template>
  <div class="flex gap-x-2 font-semibold">
    <button @click="$emit('changePage', 'prev')"><</button>
    <span class="font-bold">{{ currentPage }}</span>
    <button @click="$emit('changePage', 'next')">></button>
  </div>
</template>
