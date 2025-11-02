<template>
  <div
    class="bg-white p-0.5 mb-0.5 rounded shadow-sm max-w-full"
    :title="task.createdAt.toLocaleDateString()"
  >
    <span>{{ task.title }}</span>
  </div>
</template>

<script setup lang="ts">
import type { Task } from '~/types';

defineProps<{
  task: Task;
}>();
</script>
