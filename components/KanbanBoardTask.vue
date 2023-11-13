<script setup lang="ts">
import type { Task } from "~/types";

defineProps<{
  task: Task;
}>();
</script>

<template>
  <div
    class="task bg-white p-2 rounded shadow-sm mb-2 max-w-[300px] overflow-hidden h-full flex gap-1"
    :title="new Date(task.createdAt).toLocaleDateString()"
  >
    <DragHandle />
    <span>{{ task.title }}</span>
  </div>
</template>

<style>
.sortable-drag .task {
  transform: rotate(3deg);
}
</style>
