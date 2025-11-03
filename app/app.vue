<template>
  <div class="p-2.5 h-screen bg-teal-600 overflow-auto">
    <header class="flex gap-1 text-white items-center mb-2.5">
      <IconBrandNuxt class="size-3" />
      <h1 class="text-4xl">Trello Board</h1>
    </header>
    <main>
      <TrelloBoard />
    </main>
  </div>
</template>

<script setup lang="ts">
import { IconBrandNuxt } from '@tabler/icons-vue';

useSeoMeta({
  title: 'Trello Board',
});
</script>
