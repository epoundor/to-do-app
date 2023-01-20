<script setup lang="ts">
import { computed, Ref, ref } from "vue";
import Task from "./components/Task.vue";
const isWritting = ref(false);
const newTask = ref("");
const panel = ref("all");
enum State {
  PROGRESS,
  FINISH,
}
interface Task {
  state: State;
  label: string;
}
const tasks: Ref<Task[]> = ref([]);
tasks.value.push(
  {
    state: State.PROGRESS,
    label: "Task 1",
  },
  {
    state: State.PROGRESS,
    label: "Task 1",
  }
);

const addTodo = () => {
  isWritting.value = true;
};
const add = () => {
  tasks.value.push({
    state: State.PROGRESS,
    label: newTask.value,
  });
  newTask.value = "";
  isWritting.value = false;
};
const update = (id: number) => {
  if (tasks.value[id].state == State.FINISH) {
    tasks.value[id].state = State.PROGRESS;
    return;
  }
  tasks.value[id].state = State.FINISH;
};
const filteredTasks = computed(() => {
  if (panel.value == "all") {
    return tasks.value;
  }
  if (panel.value == "done") {
    return tasks.value.filter((el) => el.state === State.FINISH);
  }
  if (panel.value == "todo") {
    return tasks.value.filter((el) => el.state === State.PROGRESS);
  }
});
</script>

<template>
  <div class="fixed flex flex-col inset-0 bg-white items-center justify-center">
    <div class="shadow-md flex-col flex rounded overflow-hidden w-80 h-96">
      <div class="relative bg-violet-200">
        <div class="flex text-white px-6 py-10 gap-2">
          <span class="font-bold text-6xl">{{
            tasks.filter((el) => el.state == State.FINISH).length
          }}</span>
          <div>
            <div class="font-semibold">Tasks</div>
            <div>/{{ tasks.length }}</div>
          </div>
        </div>
        <div
          v-if="!isWritting"
          @click="addTodo"
          class="-bottom-6 right-5 rounded-full aspect-square absolute p-6 bg-blue-500 text-white leading-4"
        ></div>
      </div>
      <div v-if="isWritting" class="flex p-4 items-stretch border-b">
        <form class="flex gap-4" @submit.prevent="add">
          <input
            v-model="newTask"
            type="text"
            class="border-blue-400 border rounded"
          />
          <button
            class="px-2 py-1 rounded text-white bg-blue-500"
            type="submit"
          >
            Add
          </button>
        </form>
      </div>
      <div class="flex px-4 items-stretch border-b gap-4 h-10">
        <div
          class="py-1 aspect-square"
          :class="{ 'border-b border-blue-400': panel == 'all' }"
          @click="panel = 'all'"
        >
          Tout
        </div>
        <div
          class="py-1 aspect-square"
          :class="{ ' border-b border-blue-400': panel == 'done' }"
          @click="panel = 'done'"
        >
          Faits
        </div>
        <div
          class="py-1 aspect-square"
          :class="{ ' border-b border-blue-400': panel == 'todo' }"
          @click="panel = 'todo'"
        >
          Non Faits
        </div>
      </div>
      <div class="flex-1 overflow-scroll">
        <template v-for="(task, i) in filteredTasks">
          <Task :task="task" :id="i + ''" @update="update" />
        </template>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
