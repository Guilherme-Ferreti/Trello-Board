<template>
  <div
    class="task bg-white p-0.5 mb-0.5 rounded shadow-sm max-w-full flex"
    :title="task.createdAt.toLocaleDateString()"
  >
    <DragHandle />
    <span>{{ task.title }}</span>
  </div>
</template>

<script setup lang="ts">
import type { Task } from '~/types';

defineProps<{
  task: Task;
}>();
</script>

<style scoped>
@reference '~/assets/css/main.css';

.sortable-drag .task {
  @apply rotate-[5deg] transform;
}

.sortable-ghost .task {
  @apply relative;
}

.sortable-ghost .task::after {
  content: '';
  @apply absolute inset-0 rounded bg-slate-300;
}
</style>
