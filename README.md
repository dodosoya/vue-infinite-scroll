# Vue Infinite Scroll

Implement an infinite scroll component using Vue and intersection observer API.

## Steps

1. Create `Observer.vue` component to use [Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)

   ```js
   <script setup>
   import { ref, onMounted, onUnmounted } from 'vue'
   const props = defineProps({
     options: {
       type: Object,
       default: () => {},
     },
   })
   const emit = defineEmits(['intersect'])
   const root = ref(null)
   const observer = ref(null)
   onMounted(() => {
     observer.value = new IntersectionObserver(([entry]) => {
       if (entry && entry.isIntersecting) {
         emit('intersect')
       }
     }, props.options)
     observer.value.observe(root.value)
   })
   onUnmounted(() => {
     observer.value.disconnect()
   })
   </script>

   <template>
     <div ref="root" />
   </template>
   ```

2. Create `InfiniteScroll.vue` component

   ```js
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
   ```

## Reference

- [Build an Infinite Scroll component using Intersection Observer API](https://vueschool.io/articles/vuejs-tutorials/build-an-infinite-scroll-component-using-intersection-observer-api/)
