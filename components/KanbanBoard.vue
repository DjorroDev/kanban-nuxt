<script setup lang="ts">
import { nanoid } from "nanoid";
import type { Column, Task } from "~~/types";
import draggable from "vuedraggable";

const columns = useLocalStorage<Column[]>("kanbanBoard", [
  {
    id: nanoid(),
    title: "Planning",
    tasks: [
      { id: nanoid(), title: "Create the landing page", createdAt: new Date() },
      { id: nanoid(), title: "Add Menu bar", createdAt: new Date() },
    ],
  },
  {
    id: nanoid(),
    title: "In Progress",
    tasks: [{ id: nanoid(), title: "Add Main features", createdAt: new Date() }],
  },
  {
    id: nanoid(),
    title: "finish",
    tasks: [],
  },
]);

const alt = useKeyModifier("Alt");

function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: "",
    tasks: [],
  };
  columns.value.push(column);

  nextTick(() => {
    (document.querySelector(".column:last-of-type .title-input") as HTMLInputElement).focus();
  });
}
</script>

<template>
  <div class="flex flex-col md:flex-row h-[80vh] md:items-start items-center overflow-x-auto gap-4">
    <draggable
      v-model="columns"
      group="columns"
      :animation="250"
      handle=".drag-handle"
      item-key="id"
      class="flex flex-col md:flex-row gap-4 items-start"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
          <header class="flex items-end font-bold mb-4">
            <DragHandle />
            <input
              class="title-input bg-transparent focus:bg-white rounded ml-1 px-1 w-4/5"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
              @keydown.backspace="
                column.title === '' ? (columns = columns.filter((c) => c.id !== column.id)) : null
              "
              type="text"
              name="title"
              v-model="column.title"
            />
          </header>
          <draggable
            v-model="column.tasks"
            :group="{ name: 'task', pull: alt ? 'clone' : true }"
            :animation="250"
            handle=".drag-handle"
            item-key="id"
          >
            <template #item="{ element: task }: { element: Task }">
              <div>
                <KanbanBoardTask :task="task" />
              </div>
            </template>
          </draggable>
          <footer>
            <NewTask @add="column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>
    <button @click="createColumn" class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-80">
      + Add Another Column
    </button>
  </div>
</template>
