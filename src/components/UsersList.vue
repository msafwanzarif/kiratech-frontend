<template>
  <section class="container mx-auto px-4 mt-4 md:mt-12">
    <div class="">
      <div class="w-full bg-white p-4 flex items-center justify-end sticky top-0 z-20">
        <input type="text" v-model="searchInput" id="search" placeholder="Search users..." class="border border-gray-300 rounded-md px-4 py-2 mr-4" />
        <span v-if="searchInput" class="text-gray-400 hidden md:block">Matched {{ users.length }} users of {{ totalUsers }}</span>
        <span v-else class="text-gray-400 hidden md:block">Showing {{ users.length }} users of {{ totalUsers }}</span>
        <span class="text-gray-400 md:hidden">{{ users.length }} / {{ totalUsers }}</span>
      </div>
      <table class="w-full table-fixed text-left border-separate border-spacing-y-4  hidden md:table">
        <thead class="text-gray-400 text-sm bg-white sticky top-16 z-10">
          <tr class="pb-8">
            <th class="px-6 py-4 w-4">#</th>
            <th class="px-6 py-4 w-40">Registered</th>
            <th class="px-6 py-4 w-full">Name</th>
            <th class="px-6 py-4 w-32">Gender</th>
            <th class="px-6 py-4 w-52">Country</th>
            <th class="px-6 py-4 w-full">Email</th>
          </tr>
        </thead>
        <tbody class="mt-8">
          <tr v-for="(user,index) in users" @click="$emit('select-user', user.email)" :key="user.email" class="group hover:bg-gray-50 border border-blue-600 bg-white shadow-md hover:shadow-sky-400 hover:shadow-[0_0_0_2px_rgba(59,130,246,0.7)] cursor-pointer rounded">
            <td class="px-6 py-4">{{ index + 1 }}</td>
            <td class="px-6 py-4 text-gray-400">{{ user.date }}</td>
            <td class="px-6 py-4 group-hover:text-sky-400">{{ user.name }}</td>
            <td class="px-6 py-4 text-gray-400">{{ user.gender }}</td>
            <td class="px-6 py-4">{{ user.country }}</td>
            <td class="px-6 py-4 text-gray-400">{{ user.email }}</td>
          </tr>
        </tbody>
      </table>
      <table class="w-full table-fixed text-left border-separate border-spacing-y-4 table md:hidden">
        <thead class="text-gray-400 text-sm bg-white sticky top-16 z-10">
          <tr class="pb-8">
            <th class="px-6 py-4 w-4">#</th>
            <th class="px-6 py-4 w-full">Name</th>
            <th class="px-6 py-4 w-52">Country</th>
          </tr>
        </thead>
        <tbody class="divide-y mt-8 divide-gray-200">
          <tr v-for="(user,index) in users" @click="$emit('select-user', user.email)" :key="user.email" class="hover:bg-gray-50 bg-white shadow-md cursor-pointer">
            <td class="px-6 py-4">{{ index + 1 }}</td>
            <td class="px-6 py-4">{{ user.name }}</td>
            <td class="px-6 py-4">{{ user.country }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    
    <div ref="loadTrigger" class="h-8 py-4 flex justify-center items-center text-gray-400">
      <span v-if="isLoading">Loading...</span>
    </div>

    <!-- Refresh Button -->
    <div class="flex justify-center my-6">
      <button 
        @click="$emit('refresh')" 
        class="bg-sky-500 hover:bg-sky-600 text-white px-6 py-2 rounded-lg shadow"
      >
        Refresh List
      </button>
      <button 
        @click="$emit('repopulate')" 
        class="bg-sky-500 hover:bg-sky-600 text-white px-6 py-2 rounded-lg shadow ml-4"
      >
        Repopulate Data
      </button>
    </div>
    
  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount,watch } from "vue"

const props = defineProps({
  users: Array,
  isLoading: Boolean,
  totalUsers: Number,
})
const searchInput = ref("")

watch(searchInput, (newValue) => {
  // Emit search event with the new value
  emit("search", newValue)
})

const emit = defineEmits(["next-page","search","refresh","repopulate","select-user"])

const loadTrigger = ref(null)
let observer

// Compute paged users
function loadPage() {
  if (props.isLoading.value) return
  emit("next-page")
}

onMounted(() => {
  loadPage() // load first page

  observer = new IntersectionObserver((entries) => {
    if (entries[0].isIntersecting) {
      loadPage()
    }
  })

  if (loadTrigger.value) {
    observer.observe(loadTrigger.value)
  }
})

onBeforeUnmount(() => {
  if (observer && loadTrigger.value) {
    observer.unobserve(loadTrigger.value)
  }
})
</script>
