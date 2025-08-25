<template>
  <header class=" px-6 py-4 bg-white shadow">
    <div class="container mx-auto px-4 flex justify-between items-center">
      <!-- Logo -->
      <div class="flex items-center space-x-2">
        <img src="/Logo.png" alt="Kiratech" class="h-12" />
      </div>
      <!-- Icons -->
      <div class="flex items-center space-x-6 text-gray-600">
        <button><IconBell /></button>
        <button><IconCog /></button>
        <button><IconLogout /></button>
      </div>
    </div>
  </header>
</template>

<script setup>
import IconBell from './icons/IconBell.vue'
import IconCog from './icons/IconCog.vue'
import IconLogout from './icons/IconLogout.vue'
</script>
