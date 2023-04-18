<script setup>
import {useDark, useToggle} from '@vueuse/core'

const isDark = useDark()
const toggleDark = useToggle(isDark)

</script>

<template>

<header class="dark:bg-gray-800 dark:text-white transition-all duration-500">
  <nav class="mx-auto flex max-w-7xl items-center justify-between p-6 lg:px-8">
    <div class="lg:flex lg:flex-1 lg:justify-end">
      <button @click="toggleDark()">
        <span class="material-icons-outlined">{{ isDark ? "dark" : "light" }}_mode</span>
      </button>
    </div>
  </nav>
</header>


</template>