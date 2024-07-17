<script setup>
defineProps(['data', 'update:data']);
</script>

<template>
  <div class="grid grid-cols-3 gap-3">
    <div
      v-for="(block, i) in data"
      :key="i"
      class="bg-black/5 h-20 w-20 hover:bg-black/15 text-center content-center text-3xl font-semibold"
      :class="[
        block === 0 ? 'text-blue-500' : 'text-teal-400',
        block === null && 'hover:cursor-pointer',
      ]"
      @click="block === null && $emit('update:data', i)"
      v-text="block === null ? '' : block === 1 ? 'X' : 'O'"
    />
  </div>
</template>
