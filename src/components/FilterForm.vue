<script setup>
import { ref } from 'vue'

const filterData = ref({
  name: '',
  status: ''
})

const emit = defineEmits(['filter'])

const submitForm = () => {
  emit('filter', filterData.value)
}
</script>

<template>
  <form @submit.prevent="submitForm" class="flex items-center">
    <div class="flex items-end gap-4">
      <div class="flex flex-col gap-y-2">
        <label class="font-medium text-lg" for="name">Name</label>
        <input
          class="p-2 border border-gray-500 rounded-md"
          placeholder="Rick"
          type="text"
          name="name"
          v-model="filterData.name"
        />
      </div>
      <div class="flex flex-col gap-y-2">
        <label class="font-medium text-lg" for="status">Status</label>
        <input
          class="p-2 border border-gray-500 rounded-md"
          placeholder="Alive"
          type="text"
          name="status"
          v-model="filterData.status"
        />
      </div>
      <div>
        <button
          type="submit"
          class="px-4 py-2 rounded-xl text-white font-bold shadow-blue-200 shadow-md bg-sky-400 hover:bg-sky-500"
        >
          Применить
        </button>
      </div>
    </div>
  </form>
</template>
