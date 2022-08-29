<template>
	<div class="container mt-5">
		<div class="d-flex justify-content-center">
			<img class="logo" src="./assets/logo.svg" alt="logo" />
			<h2 class="title">Todo App</h2>
		</div>
		<div class="d-flex justify-content-end">
			<input v-model="task" type="text" class="form-control" placeholder="タスクを入力してください" />
			<div class="ms-3"></div>
			<button @click="submitTask" class="btn btn-primary">追加</button>
		</div>

		<table class="table mt-5">
			<thead>
				<tr>
					<th>タスク</th>
					<th>完了</th>
					<th>編集</th>
					<th>削除</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="(task, index) in tasks" :key="index">
					<td>
						<span :class="{ completed: task.completed }">{{ task.name }}</span>
					</td>
					<td>
						<input type="checkbox" v-model="task.completed" />
					</td>
					<td>
						<div>
							<div @click="editTask(index)">
								<div :class="{ editing: this.editingTaskIndex === index }"></div>
								<span class="fa fa-pen pointer"></span>
							</div>
						</div>
					</td>
					<td>
						<div>
							<div @click="deleteTask(index)">
								<span class="fa fa-trash pointer"></span>
							</div>
						</div>
					</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<script>
export default {
	data() {
		return {
			task: '',
			editingTaskIndex: null,
			tasks: [
				{
					name: 'タスク1',
					completed: true,
				},
				{
					name: 'タスク2',
					completed: false,
				},
				{
					name: 'タスク3',
					completed: false,
				},
			],
		};
	},

	methods: {
		submitTask() {
			if (this.task.length === 0) return;

			this.editingTaskIndex === null ? this.addTask() : this.updateTask();
			this.task = '';
		},

		addTask() {
			this.tasks.push({
				name: this.task,
				completed: false,
			});
		},

		updateTask() {
			this.tasks[this.editingTaskIndex].name = this.task;
			this.editingTaskIndex = null;
		},

		deleteTask(index) {
			if (this.editingTaskIndex !== null) {
				alert('編集中は削除できません');
				return;
			}
			this.tasks.splice(index, 1);
		},

		editTask(index) {
			this.editingTaskIndex = index;
			this.task = this.tasks[index].name;
		},
	},
};
</script>

<style>
.header {
	margin: auto;
}
.logo {
	margin: 20px;
	height: 30px;
}
.title {
	margin: auto 0;
}
.pointer {
	cursor: pointer;
}
.editing {
	color: #007bff;
}
.completed {
	text-decoration: line-through;
}
</style>
