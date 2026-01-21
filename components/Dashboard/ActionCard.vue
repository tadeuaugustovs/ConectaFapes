<template>
  <button
    class="flex flex-col items-start w-full p-3 bg-white dark:bg-gray-900 rounded-md border border-gray-200 dark:border-gray-800 hover:border-blue-300 dark:hover:border-blue-800 hover:shadow-sm transition-all duration-200 text-left group h-full"
  >
    <!-- Icon Container -->
    <div class="mb-2 w-8 h-8 rounded-lg bg-gray-50 dark:bg-gray-800 flex items-center justify-center group-hover:bg-blue-50 dark:group-hover:bg-blue-900/20 transition-colors">
       <UIcon
        :name="icon"
        class="w-4 h-4 text-gray-400 group-hover:text-blue-500 dark:group-hover:text-blue-400 transition-colors"
      />
    </div>
    
    <!-- Title and Description -->
    <div class="w-full">
      <h3 class="text-sm font-semibold text-gray-900 dark:text-white group-hover:text-blue-600 dark:group-hover:text-blue-400 transition-colors">
        {{ title }}
      </h3>
      
      <p class="mt-0.5 text-xs text-gray-500 dark:text-gray-400 line-clamp-2 leading-relaxed">
        {{ description }}
      </p>
    </div>
  </button>
</template>

<script setup lang="ts">
defineProps<{
  title: string
  description: string
  icon: string
}>()
</script>
