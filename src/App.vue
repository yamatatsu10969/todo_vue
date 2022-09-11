<template>
  <div class="container mt-5">
    <div class="d-flex justify-content-center">
      <img class="logo" src="./assets/logo.svg" alt="logo" />
      <h2 class="title">Todo App</h2>
    </div>
    <div class="d-flex justify-content-end">
      <input v-model="task" type="text" class="form-control" placeholder="タスクを入力してください" />
      <div class="ms-3"></div>
      <button @click="submitTask" class="btn btn-primary primaryButton">
        {{ editing ? "保存" : "追加" }}
      </button>
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
          <td class="icon">
            <div class="text-center">
              <input type="checkbox" v-model="task.completed" />
            </div>
          </td>
          <td class="icon">
            <div class="text-center">
              <div @click="editTask(index)">
                <div :class="{ editing: isTaskEditingById(index) }">
                  <span class="fa fa-pen pointer"></span>
                </div>
              </div>
            </div>
          </td>
          <td class="icon">
            <div class="text-center">
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
      task: "",
      editingTaskIndex: null,
      tasks: [],
    };
  },

  mounted() {
    if (localStorage.getItem("tasks")) {
      try {
        this.tasks = JSON.parse(localStorage.getItem("tasks"));
      } catch (e) {
        localStorage.removeItem("tasks");
      }
    }
  },

  computed: {
    editing() {
      return this.editingTaskIndex !== null;
    },
  },

  methods: {
    isTaskEditingById(index) {
      return this.editingTaskIndex === index;
    },
    submitTask() {
      if (this.task.length === 0) return;

      this.editingTaskIndex === null ? this.addTask() : this.updateTask();
      this.task = "";
    },

    addTask() {
      this.tasks.push({
        name: this.task,
        completed: false,
      });
      this.saveTasks();
    },

    updateTask() {
      this.tasks[this.editingTaskIndex].name = this.task;
      this.editingTaskIndex = null;
      this.saveTasks();
    },

    deleteTask(index) {
      if (this.editingTaskIndex !== null) {
        alert("編集中は削除できません");
        return;
      }
      this.tasks.splice(index, 1);
      this.saveTasks();
    },

    editTask(index) {
      if (this.isTaskEditingById(index)) {
        this.editingTaskIndex = null;
        this.task = "";
      } else {
        this.editingTaskIndex = index;
        this.task = this.tasks[index].name;
      }
    },

    saveTasks() {
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem("tasks", parsed);
    },
  },
};
</script>

<style scoped>
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
.icon {
  width: 48px;
}
.primaryButton {
  width: 64px;
}
</style>
