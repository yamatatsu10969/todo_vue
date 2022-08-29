<template>
	<div class="container mt-5">
		<div class="d-flex justify-content-center">
			<img class="logo" src="./assets/logo.svg" alt="logo" />
			<h2 class="title">Todo App</h2>
		</div>
		<div class="d-flex justify-content-end">
			<input type="text" class="form-control" placeholder="タスクを入力してください" />
			<div class="ms-3"></div>
			<button class="btn btn-primary">追加</button>
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
				<tr v-for="(task, index) in tasks">
					<td>{{ task.name }}</td>
					<td>
						<input type="checkbox" v-model="task.done" />
					</td>
					<td>
						<span class="fa fa-trash pointer"></span>
					</td>
					<td>
						<p class="fa fa-pen pointer"></p>
					</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

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
</style>

<script>
export default {
	data() {
		return {
			tasks: [
				{
					name: 'タスク１',
					done: false,
				},
			],
		};
	},
};
</script>
