<template>
  <div 
    v-if="user" 
    class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
    @click="$emit('close')"
  >
    <div class="bg-white rounded-lg shadow-xl max-w-lg w-full p-6 relative">
      <button 
        @click="$emit('close')" 
        class="absolute top-4 right-4 text-gray-500 hover:text-gray-700"
      >
        ✕
      </button>
      <h2 class="text-2xl font-bold mb-4 text-center md:text-left">{{ user.name }}</h2>
      <div class="space-y-2 text-gray-700">
        <div class="md:grid flex flex-col items-center md:grid-cols-[auto,1fr] gap-8">
          <img :src="user.picture" alt="User Picture" class="w-32 h-32 rounded-full" />
          <div class="grid grid-cols-[auto,1fr] gap-4 text-sm">
            <span class="text-gray-400">Registered:</span> <span>{{ user.date }}</span>
            <span class="text-gray-400">DOB:</span> <span>{{ user.dob }}</span>
            <span class="text-gray-400">Gender:</span> <span>{{ user.gender }}</span>
            <span class="text-gray-400">Location:</span> <span>{{ user.city }}, {{ user.country }}</span>
            <span class="text-gray-400">Email:</span> <span>{{ user.email }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  user: Object // null when closed
})
</script>
