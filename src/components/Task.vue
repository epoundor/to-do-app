<template>
  <label class="flex justify-between items-center p-4" :for="id">
    <div
      class="flex items-center gap-2"
      :class="{ 'italic line-through opacity-50': task.state == State.FINISH }"
    >
      <input
        type="checkbox"
        :name="id"
        :id="id"
        :checked="task.state == State.FINISH"
        @click="change(id)"
      />
      {{ task.label }}
    </div>
  </label>
</template>

<script setup lang="ts">
enum State {
  PROGRESS,
  FINISH,
}
interface Task {
  state: State;
  label: string;
}
defineProps<{ task: Task; id: string }>();
const emit = defineEmits<{
  (event: "update", id: number): void;
}>();
const change = (id: string) => {
  emit("update", Number(id));
};
</script>
