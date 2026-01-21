<template>
  <ClientOnly>
    <div class="flex items-center justify-center w-full h-full">
      <!-- Collapsed State (Icons) -->
      <img 
        v-if="collapsed && !isDark"
        src="/Icone.png"
        alt="Conecta FAPES"
        class="w-16 h-16 object-contain transition-all duration-300"
      />
      <img 
        v-else-if="collapsed && isDark"
        src="/Icone-modo-escuro.png"
        alt="Conecta FAPES"
        class="w-16 h-16 object-contain transition-all duration-300"
      />
      
      <!-- Expanded State (Full Logos) -->
      <img 
        v-else-if="!collapsed && !isDark"
        src="/logo-modo-claro.png"
        alt="Conecta FAPES"
        class="h-full w-auto max-h-28 object-contain transition-all duration-300"
      />
      <img 
        v-else-if="!collapsed && isDark"
        src="/logo-modo-escuro.png"
        alt="Conecta FAPES"
        class="h-full w-auto max-h-28 object-contain transition-all duration-300"
      />
    </div>
  </ClientOnly>
</template>

<script setup lang="ts">
const colorMode = useColorMode()

defineProps<{
  collapsed: boolean
}>()

const isDark = computed(() => colorMode.value === 'dark')
</script>
