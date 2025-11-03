<template>
  <div class="flex">
    <textarea
      v-model.trim="title"
      class="focus:bg-white focus:shadow resize-none rounded w-full border-none bg-transparent p-0.5 cursor-pointer outline-none! box-content scrollbar-thin"
      :class="{
        'h-lh': !focused,
        ' h-[3lh]': focused,
      }"
      @keydown.tab="createTask"
      @keydown.enter="createTask"
      @focus="focused = true"
      @blur="handleBlur"
      :placeholder="!focused ? '+ Add A Card' : 'Enter a title for this card'"
    ></textarea>
  </div>
</template>

<script setup lang="ts">
import { nanoid } from 'nanoid';
import type { Task } from '~/types';

const emit = defineEmits<{
  (e: 'add', payload: Task): void;
}>();

const title = ref('');
const focused = ref(false);

function createTask(e: Event) {
  if (title.value) {
    e.preventDefault();

    emit('add', {
      id: nanoid(),
      title: title.value,
      createdAt: new Date(),
    } as Task);
  }

  title.value = '';
}

function handleBlur() {
  focused.value = false;
  title.value = '';
}
</script>
