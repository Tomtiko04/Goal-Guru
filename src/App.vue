<template>
  <main>
    <Header />
    <InputTask />
    <TaskList />
  </main>
</template>

<!-- <script setup>
import { provide, ref, watch } from "vue";
import Header from "@/component/Header.vue";
import InputTask from "@/component/InputTask.vue";
import TaskList from "@/component/Tasks.vue";

const tasksData = ref([
  { id: 1, task: "Complete online Javascript course", completed: false, deleted: false },
  { id: 2, task: "Jog around the park 3x", completed: false, deleted: false },
  { id: 3, task: "Read for 1 hour", completed: false, deleted: false },
  { id: 4, task: "Pick up groceries", completed: false, deleted: false },
  { id: 5, task: "Complete Todo App on Frontend Mentor", completed: false , deleted: false}
]);

const theme = ref(true);

provide('tasksData', tasksData);
provide('theme', theme);

function applyThemeClass() {
  if (theme.value) {
    document.body.classList.add('light');
    document.body.classList.remove('dark');
  } else {
    document.body.classList.add('dark');
    document.body.classList.remove('light');
  }
}

applyThemeClass();

watch(theme, () => {
  applyThemeClass();
});
</script> -->

<script setup>
import { provide, ref, watch } from "vue";
import Header from "@/component/Header.vue";
import InputTask from "@/component/InputTask.vue";
import TaskList from "@/component/Tasks.vue";

const storedTasks = localStorage.getItem('tasksData');
const tasksData = ref(storedTasks ? JSON.parse(storedTasks) : [
  { id: 1, task: "Complete online Javascript course", completed: false, deleted: false },
  { id: 2, task: "Jog around the park 3x", completed: false, deleted: false },
  { id: 3, task: "Read for 1 hour", completed: false, deleted: false },
  { id: 4, task: "Pick up groceries", completed: false, deleted: false },
  { id: 5, task: "Complete Todo App on Frontend Mentor", completed: false , deleted: false}
]);

const theme = ref(true);

provide('tasksData', tasksData);
provide('theme', theme);

watch(tasksData, (newTasksData) => {
  localStorage.setItem('tasksData', JSON.stringify(newTasksData));
}, { deep: true });

function applyThemeClass() {
  if (theme.value) {
    document.body.classList.add('light');
    document.body.classList.remove('dark');
  } else {
    document.body.classList.add('dark');
    document.body.classList.remove('light');
  }
}

applyThemeClass();

watch(theme, () => {
  applyThemeClass();
});
</script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  overflow: hidden;
  font-family: "Josefin Sans", sans-serif;
}

body {
  font-family: "Josefin Sans", sans-serif;
  /* background-image: url("./assets/bg-desktop-light.jpg");
  background-repeat: no-repeat; */
  /* background-color: #fff; */
}

.light {
  background-color: hsl(0, 0%, 98%);
}

.dark {
  background-color: hsl(235, 21%, 11%);
}

main {
  width: 35vw;
  margin: 0 auto;
  padding-top: 5em;
}

input{
  border: none;
  outline: none;
}

@media (max-width: 768px) {
  main {
    width: 70vw;
    padding-top: 3em;
  }
}

@media (max-width: 480px) {
  main {
    width: 90vw;
  }
}

</style>
