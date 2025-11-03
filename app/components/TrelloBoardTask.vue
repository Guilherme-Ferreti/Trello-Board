<template>
  <div
    class="task bg-white p-0.5 mb-0.5 rounded shadow-sm max-w-full flex break-word focus:outline"
    :title="task.createdAt.toLocaleDateString()"
    tabindex="0"
    @focus="focused = true"
    @blur="focused = false"
  >
    <DragHandle />
    <span>{{ task.title }}</span>
  </div>
</template>

<script setup lang="ts">
import type { Task } from '~/types';

const props = defineProps<{
  task: Task;
}>();

const emit = defineEmits<{
  (e: 'delete', payload: string): void;
}>();

onKeyStroke(['Backspace', 'Delete'], () => {
  if (focused.value) {
    emit('delete', props.task.id);
  }
});

const focused = ref(false);
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
