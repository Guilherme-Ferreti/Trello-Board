<template>
  <div class="flex items-start overflow-x-auto gap-1 scrollbar-thin pb-1">
    <draggable
      v-model="columns"
      class="flex gap-1 items-start"
      group="columns"
      item-key="id"
      :animation="150"
      handle=".drag-handle"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 p-1.25 rounded min-w-20">
          <header class="font-bold mb-1 flex items-center">
            <DragHandle />
            <input
              v-model="column.title"
              class="title-input bg-transparent focus:bg-white rounded px-0.25 w-4/5"
              type="text"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
              @keydown.backspace="deleteColumn(column)"
            />
          </header>
          <div>
            <draggable
              v-model="column.tasks"
              :group="{
                name: 'tasks',
                pull: alt ? 'clone' : true,
              }"
              item-key="id"
              :animation="150"
              handle=".drag-handle"
            >
              <template #item="{ element: task }: { element: Task }">
                <div>
                  <TrelloBoardTask
                    :task="task"
                    @delete="deleteTaskFromColumn(task, column)"
                  />
                </div>
              </template>
            </draggable>
            <footer>
              <NewTask @add="addTaskToColumn($event, column)" />
            </footer>
          </div>
        </div>
      </template>
    </draggable>
    <button
      class="bg-gray-200 whitespace-nowrap p-0.5 rounded opacity-50 cursor-pointer"
      @click="createColumn"
    >
      + Add Another Column
    </button>
  </div>
</template>

<script setup lang="ts">
import draggable from 'vuedraggable';
import { nanoid } from 'nanoid';
import type { Column, Task } from '~/types';

const alt = useKeyModifier('Alt');

const columns = useLocalStorage<Column[]>(
  'trello-board',
  [
    {
      title: 'Select for Development',
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
    { title: 'In Progress', id: nanoid(), tasks: [] },
    { title: 'Complete', id: nanoid(), tasks: [] },
  ],
  {
    serializer: {
      read: (value) => {
        return JSON.parse(value).map((column: Column) => {
          column.tasks = column.tasks.map((task: Task) => {
            task.createdAt = new Date(task.createdAt);

            return task;
          });

          return column;
        });
      },
      write: (value) => JSON.stringify(value),
    },
  },
);

function addTaskToColumn(task: Task, column: Column) {
  column.tasks.push(task);
}

function deleteTaskFromColumn(task: Task, column: Column) {
  column.tasks = column.tasks.filter((t) => t.id !== task.id);
}

function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: '',
    tasks: [],
  };

  columns.value.push(column);

  nextTick(() =>
    (document.querySelector('.column:last-of-type .title-input') as HTMLInputElement).focus(),
  );
}

function deleteColumn(column: Column) {
  if (column.title) {
    return;
  }

  columns.value = columns.value.filter((c) => c.id !== column.id);
}
</script>
