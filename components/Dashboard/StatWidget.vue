<template>
  <div class="p-4 bg-white dark:bg-gray-900 rounded-md border border-gray-200 dark:border-gray-800 hover:border-gray-300 dark:hover:border-gray-700 transition-colors flex flex-col justify-between h-full group shadow-sm">
     <div>
        <h3 class="text-sm font-medium text-gray-500 dark:text-gray-400">{{ label }}</h3>
     </div>
     <div class="mt-2">
        <p class="text-3xl font-bold text-gray-900 dark:text-white tracking-tight">{{ value }}</p>
     </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  label: string
  value: string | number
}>()
</script>
