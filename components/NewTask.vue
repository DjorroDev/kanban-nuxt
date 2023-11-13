<script setup lang="ts">
import type { Task } from "~/types";
import { nanoid } from "nanoid";

const emit = defineEmits<{
  add: [task: Task];
}>();

const title = ref("");
const focused = ref(false);

function createTask(e: Event) {
  if (title.value.trim()) {
    e.preventDefault();
    emit("add", {
      title: title.value.trim(),
      createdAt: new Date(),
      id: nanoid(),
    });
  }

  title.value = "";
}

function cancelTask(e: Event) {
  (e.target as HTMLInputElement).blur();
  title.value = "";
}
</script>

<template>
  <div>
    <textarea
      name="new-task"
      cols="10"
      :rows="focused ? 5 : 1"
      v-model="title"
      @keydown.enter="createTask"
      @keydown.tab="createTask"
      @keydown.esc="cancelTask"
      class="focus:bg-white focus:shadow resize-none bg-inherit rounded w-full border-none px-1 py-1 h-fit focus:px-3"
      :placeholder="focused ? 'Enter a title for task' : '+ Add a Card'"
      @focus="focused = true"
      @blur="focused = false"
    ></textarea>
  </div>
</template>
