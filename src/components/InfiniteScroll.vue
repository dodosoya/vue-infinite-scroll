<script setup>
import { ref } from 'vue'
import Observer from './Observer.vue'

const page = ref(1)
const items = ref([])

const getItems = async () => {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/comments?_page=${page.value}&_limit=20`
  )
  page.value++
  const data = await res.json()
  items.value = [...items.value, ...data]
}
</script>

<template>
  <div
    class="h-[400px] w-[800px] overflow-auto border border-solid border-blue-300"
  >
    <ui class="block p-4">
      <li class="text-left" v-for="item in items" :key="item.id">
        {{ item.name }}
      </li>
    </ui>

    <Observer @intersect="getItems" />
  </div>
</template>
