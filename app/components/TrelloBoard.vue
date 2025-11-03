<template>
  <div>
    <draggable
      v-model="columns"
      class="flex gap-1 overflow-x-auto items-start"
      group="columns"
      item-key="id"
      :animation="150"
      handle=".drag-handle"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="bg-gray-200 p-1.25 rounded min-w-16">
          <header class="font-bold mb-1 flex items-center">
            <DragHandle />
            {{ column.title }}
          </header>
          <div>
            <draggable
              v-model="column.tasks"
              group="tasks"
              item-key="id"
              :animation="150"
            >
              <template #item="{ element: task }: { element: Task }">
                <TrelloBoardTask :task="task" />
              </template>
            </draggable>
            <footer>
              <button class="text-gray-500">+ Add a Card</button>
            </footer>
          </div>
        </div>
      </template>
    </draggable>
  </div>
</template>

<script setup lang="ts">
import draggable from 'vuedraggable';
import { nanoid } from 'nanoid';
import type { Column, Task } from '~/types';

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
