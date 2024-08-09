<template>
	<div :class="[theme ? 'task-field-light' : 'task-field-dark']">
		<ul>
			<li
				v-for="(taskData, index) in filteredTasks"
				:key="taskData.id"
				class="task-item-list"
				@click.stop="toggleCancelIcon(taskData)">
				<div class="task-item">
					<span v-if="!taskData.completed" @click.stop="onComplete(taskData)" :class="[theme ? 'task-icon-light' : 'task-icon-dark']">
						<MdiCircleOutline />
					</span>
					<span v-else @click.stop="onComplete(taskData)" class="task-icon-complete">
						<MdiCheckCircleOutline />
					</span>
					<div>
						<span v-if="!taskData.completed" :class="[theme ? 'item-light': 'item-dark']">{{ taskData.task }}</span>
						<span v-else :class="[theme ? 'item-strike-light' : 'item-strike-dark']">
							<s>{{ taskData.task }}</s>
						</span>
					</div>
				</div>
				<div>
					<MdiClose
						v-if="activeTask === taskData"
						class="cancel-icon"
						@click.stop="handleDelete(index)" />
				</div>
			</li>
		</ul>

    <div v-if="currentFilter === 'all' && filteredTasks.length === 0" class="notification-message">
				<p>No task on your TODO</p>
			</div>

			<div v-else-if="currentFilter === 'active' && filteredTasks.length === 0" class="notification-message">
				<p>You have completed all your tasks</p>
			</div>

			<div v-else-if="currentFilter === 'completed' && filteredTasks.length === 0" class="notification-message">
				<p>You have not complete your tasks</p>
			</div>

		<div class="panel">
			<div class="total-task">
				<span v-if="totalTasksLeft < 1">{{ totalTasksLeft }} task left</span>
				<span v-else>{{ totalTasksLeft }} tasks left</span>
			</div>
			<div :class="[theme ? 'panel-operation-light' : 'panel-operation-dark']">
				<span @click="filterTasks('all')" :class="{ active: currentFilter === 'all' }">All</span>
				<span @click="filterTasks('active')" :class="{ active: currentFilter === 'active' }"
					>Active</span
				>
				<span @click="filterTasks('completed')" :class="{ active: currentFilter === 'completed' }"
					>Completed</span
				>
			</div>
			<div @click="handleClearAll" :class="[theme ? 'clear-button-light' : 'clear-button-dark']">
				<span>Clear Completed</span>
			</div>
		</div>
	</div>
</template>

<script setup>
import { inject, ref, computed, onMounted, onBeforeUnmount } from "vue";
import MdiCircleOutline from "vue-material-design-icons/CircleOutline.vue";
import MdiCheckCircleOutline from "vue-material-design-icons/CheckCircleOutline.vue";
import MdiClose from "vue-material-design-icons/Close.vue";

const tasksData = inject("tasksData");
const theme = inject("theme")

const currentFilter = ref("all");
const activeTask = ref(null);

function onComplete(task) {
	task.completed = !task.completed;
}

function handleDelete(index) {
	filteredTasks.value.splice(index, 1);
}

function handleClearAll() {
	tasksData.value = tasksData.value.filter((task) => !task.completed);
}

function filterTasks(filter) {
	currentFilter.value = filter;
}

const filteredTasks = computed(() => {
	if (currentFilter.value === "active") {
		return tasksData.value.filter((task) => !task.completed);
	} else if (currentFilter.value === "completed") {
		return tasksData.value.filter((task) => task.completed);
	} else {
		return tasksData.value;
	}
});

const totalTasksLeft = computed(() => {
	return tasksData.value.filter((task) => !task.completed).length;
});

function toggleCancelIcon(task) {
	activeTask.value = activeTask.value === task ? null : task;
}

function handleClickOutside(event) {
	if (!event.target.closest(".task-item-list")) {
		activeTask.value = null;
	}
}

onMounted(() => {
	document.addEventListener("click", handleClickOutside);
});

onBeforeUnmount(() => {
	document.removeEventListener("click", handleClickOutside);
});
</script>

<style scoped>
.task-field-light {
	margin-block-start: 0.7em;
	background-color: hsl(0, 0%, 98%);
	width: 100%;
	border-radius: 4px;
	box-shadow: 0 10px 30px rgba(0, 0, 0, 0.25);
}

.task-field-dark {
	margin-block-start: 0.7em;
	background-color: hsl(237, 14%, 26%);
	width: 100%;
	border-radius: 4px;
	box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
}

.task-field-light ul li {
	border-bottom: 1px solid hsl(233, 11%, 84%);
	padding: 0.7em;
}

.task-field-dark ul li {
	border-bottom: 1px solid hsl(233, 11%, 84%);
	padding: 0.7em;
}

.task-item-list {
	display: flex;
	align-items: center;
	gap: 1.1rem;
	cursor: pointer;
	justify-content: space-between;
}

.task-item {
	display: flex;
	align-items: center;
	gap: 1.1rem;
	cursor: pointer;
}

.task-icon-light {
	color: hsl(236, 33%, 92%);
	margin-left: 0.5em;
	display: inline-block;
}

.task-icon-dark {
	color: hsl(233, 14%, 35%);
	margin-left: 0.5em;
	display: inline-block;
}

.task-icon-complete {
	margin-left: 0.5em;
	display: inline-block;
	color: hsl(236, 33%, 92%);
	transition: margin-left 0.3s ease;
	background: linear-gradient(to right, hsl(192, 100%, 67%), hsl(280, 87%, 65%));
	-webkit-text-fill-color: transparent;
	border-radius: 50px;
}

.task-icon:hover {
	transition: margin-left 0.3s ease;
	background: linear-gradient(to right, hsl(192, 100%, 67%), hsl(280, 87%, 65%));
	-webkit-text-fill-color: transparent;
	border-radius: 50px;
}

.cancel-icon {
	color: hsl(233, 11%, 84%);
	cursor: pointer;
}

.cancel-icon:hover {
	color: red;
}

.item-light {
	color: hsl(235, 19%, 35%);
	font-weight: 500;
	font-size: 0.9rem;
}
.item-dark {
	color:hsl(236, 33%, 92%);
	font-weight: 500;
	font-size: 0.9rem;
}

.item-strike-light {
	color: hsl(233, 11%, 84%);
	font-weight: 400;
	font-size: 0.9rem;
}

.item-strike-dark {
	color:  hsl(234, 11%, 52%);
	font-weight: 400;
	font-size: 0.9rem;
}

.active {
	color: hsl(220, 98%, 61%);
	font-weight: 600;
}

.panel {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding-block: 1em;
	padding-inline: 1.2em;
}

.total-task {
	font-size: 0.8rem;
	color: hsl(236, 9%, 61%);
	font-weight: 400;
}

.panel-operation-light, .panel-operation-dark {
	display: flex;
	column-gap: 0.7rem;
	font-size: 0.9rem;
	font-weight: 500;
	color: hsl(236, 9%, 61%);
	cursor: pointer;
}

.clear-button-light, .clear-button-dark {
	font-size: 0.8rem;
	font-weight: 500;
	color: hsl(236, 9%, 61%);
	cursor: pointer;
	font-weight: 500;
}

.clear-button-light:hover,
.panel-operation-light span:hover {
	color: hsl(236, 33%, 92%);
}

.clear-button-dark:hover,
.panel-operation-dark span:hover {
	color: hsl(236, 33%, 92%);
}


.notification-message {
	display: flex;
	justify-content: center;
	align-items: center;
	padding-block: 3em;
	color: hsl(237, 14%, 26%);
	font-weight: 500;
}

@media (max-width: 768px) {
	.task-item-list {
		gap: 0.7rem;
	}

	.task-icon-light,
	.task-icon-dark {
		margin-left: 0.3em;
	}

	.item-light,
	.item-dark,
	.item-strike-light,
	.item-strike-dark {
		font-size: 0.8rem;
	}

	.panel {
		flex-direction: column;
		align-items: flex-start;
		gap: 1rem;
	}

	.total-task {
		font-size: 0.8rem;
	}

	.panel-operation-light,
	.panel-operation-dark {
		font-size: 0.8rem;
	}

	.clear-button-light,
	.clear-button-dark {
		font-size: 0.8rem;
	}

	.notification-message {
		padding-block: 2em;
	}
}

@media (max-width: 480px) {
	.task-item-list {
		gap: 0.5rem;
	}

	.item-light,
	.item-dark,
	.item-strike-light,
	.item-strike-dark {
		font-size: 0.75rem;
	}

	.panel {
		padding-inline: 0.8em;
	}

	.total-task {
		font-size: 0.7rem;
	}

	.panel-operation-light,
	.panel-operation-dark {
		font-size: 0.7rem;
	}

	.clear-button-light,
	.clear-button-dark {
		font-size: 0.7rem;
	}

	.notification-message {
		padding-block: 1.5em;
	}
}
</style>
