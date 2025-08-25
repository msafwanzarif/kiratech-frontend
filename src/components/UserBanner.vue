<template>
  <section class="bg-sky-500 text-white p-6  ">
    <div class="flex items-center space-x-6 container mx-auto px-4 relative">
      <!-- Profile Picture -->
      <img
        src="/Avatar.png"
        alt="Profile"
        class="relative md:absolute top-0 left-0 w-40 h-40 rounded-lg object-cover border-1 border-white"
      />
      <!-- User Info -->
      <div class="flex items-center md:items-end">
        <div class="md:w-40 h-24"></div>
        <div>
          <h1 class="text-2xl md:text-4xl font-bold">{{ name }}</h1>
          <p class="text-sm opacity-80">Last online: {{ lastOnline }}</p>
          <div class="mt-3 items-center space-x-3 flex md:hidden">
          <button class="bg-white text-sky-500 hover:bg-sky-700 hover:text-white px-4 py-2 rounded shadow flex items-center space-x-2">
            <IconSendMessage class="mr-2" />
          </button>
          <button class="bg-sky-500 text-white border border-white hover:bg-gray-100 hover:text-sky-500 px-4 py-2 rounded shadow">
            <IconAddUser/>
          </button>
        </div>
        </div>
        <div class="mt-3 ml-8 items-center space-x-3 hidden md:flex">
          <button class="bg-white hover:border hover:border-white text-sky-500 hover:bg-sky-500 hover:text-white px-4 py-3 rounded shadow flex items-center space-x-2">
            <IconSendMessage class="mr-2" /> Send Message
          </button>
          <button class="bg-sky-500 text-white border border-white hover:bg-gray-100 hover:text-sky-500 px-8 py-3 rounded shadow">
            + Add Friend
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import IconAddUser from './icons/IconAddUser.vue'
import IconSendMessage from './icons/IconSendMessage.vue'
defineProps({
  name: String,
  lastOnline: String,
})
</script>
