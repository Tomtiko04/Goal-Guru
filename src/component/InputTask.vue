<template>
  <form @submit="handleAddTask" class="addTaskInput">
    <i :class="[theme ? 'icon-light' : 'icon-dark']" class="input-icon">
      <MdiCircleOutline />
    </i>
    <input
      :class="[theme ? 'input-light' : 'input-dark']"
      type="text"
      placeholder="Create a new todo ..."
      v-model="formData.task" />
  </form>
</template>

<script setup>
import MdiCircleOutline from "vue-material-design-icons/CircleOutline.vue";
import { inject, reactive } from "vue";

const formData = reactive({
  task: "",
});
const tasksData = inject("tasksData");
const theme = inject("theme");

function handleAddTask(e) {
  e.preventDefault();
  if (formData.task.trim()) {
    tasksData.value.push({
      id: Math.floor(Math.random() * 70),
      task: formData.task,
      completed: false,
    });
    formData.task = "";
  }
}
</script>


<style scoped>
input {
	width: 100%;
	padding: 1.1em;
	margin-block: 2.2em 1em;
	border-radius: 4px;
	padding-left: 50px;
	font-size: 1rem;
	font-weight: 500;
}

.input-light {
	background-color: hsl(0, 0%, 98%);
	color: hsl(235, 19%, 35%);
}
.input-light::placeholder {
	color: hsl(233, 11%, 84%);
	font-size: 0.9rem;
}

.input-dark {
	background-color: hsl(237, 14%, 26%);
	color: hsl(236, 33%, 92%);
}

.input-dark::placeholder {
	color: hsl(234, 11%, 52%);
	font-size: 0.9rem;
}

.addTaskInput {
	display: flex;
	position: relative;
	align-items: center;
}

.addTaskInput i {
	position: absolute;
	top: 48px;
	z-index: 1000px;
	left: 15px;
}

.icon-light {
	color: hsl(236, 33%, 92%);
}
.icon-dark {
	color: hsl(233, 14%, 35%);
}

@media (max-width: 768px) {
  .addTaskInput i {
    top: 35px;
    left: 10px;
  }

  input {
    font-size: 0.9rem;
    padding: 0.9em;
    margin-block: 2em 1em;
    padding-left: 45px;
  }
}

@media (max-width: 480px) {
  .addTaskInput i {
    top: 35px;
    left: 8px;
  }

  input {
    font-size: 0.8rem;
    padding: 1.5em;
    margin-block: 1.8em 0.8em;
    padding-left: 40px;
  }
}

</style>
