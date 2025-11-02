<template>
  <div class="flex gap-1 overflow-x-auto items-start">
    <div
      v-for="column in columns"
      class="bg-gray-200 p-1.25 rounded min-w-16"
      :key="column.id"
    >
      <header>{{ column.title }}</header>
      <ul>
        <li
          v-for="task in column.tasks"
          :key="task.id"
        >
          {{ task.title }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup lang="ts">
import { nanoid } from 'nanoid';
import type { Column } from '~/types';

const columns = ref<Column[]>([
  {
    title: 'Backlog',
    id: nanoid(),
    tasks: [
      {
        title: 'Create marketing landing page',
        createdAt: new Date(),
        id: nanoid(),
      },
      {
        title: 'Develop cool new feature',
        createdAt: new Date(),
        id: nanoid(),
      },
      { title: 'Fix page nav bug', createdAt: new Date(), id: nanoid() },
    ],
  },
  { title: 'Selected for Dev', id: nanoid(), tasks: [] },
  { title: 'In Progress', id: nanoid(), tasks: [] },
  { title: 'QA', id: nanoid(), tasks: [] },
  { title: 'Complete', id: nanoid(), tasks: [] },
]);
</script>
