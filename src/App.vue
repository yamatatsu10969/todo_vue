<template>
  <div class="container mt-5">
    <div class="d-flex justify-content-center">
      <img class="logo" src="./assets/logo.svg" alt="logo" />
      <h2 class="title">Todo App</h2>
    </div>
    <div class="d-flex justify-content-end">
      <input v-model="taskName" type="text" class="form-control" placeholder="タスクを入力してください" />
      <div class="ms-3"></div>
      <button @click="submitTask" class="btn btn-primary primaryButton">
        {{ submitLabel }}
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
        <tr v-for="task in tasks" :key="task.id">
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
              <div @click="editTask(task.id)">
                <div :class="{ editing: isEditingTaskBy(task.id) }">
                  <span class="fa fa-pen pointer"></span>
                </div>
              </div>
            </div>
          </td>
          <td class="icon">
            <div class="text-center">
              <div @click="deleteTask(task.id)">
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
      taskName: "",
      editingTaskId: undefined,
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
    isEditingMode() {
      return this.editingTaskId !== undefined;
    },
    submitLabel() {
      return this.isEditingMode ? "保存" : "追加";
    },
  },

  methods: {
    isEditingTaskBy(id) {
      return this.editingTaskId === id;
    },
    submitTask() {
      if (this.taskName.length === 0) return;

      this.isEditingMode ? this.updateTask() : this.addTask();
      this.taskName = "";
    },

    addTask() {
      this.tasks.push({
        name: this.taskName,
        completed: false,
        id: new Date().getTime(),
      });
      this.saveTasks();
    },

    updateTask() {
      this.tasks.filter((task) => this.isEditingTaskBy(task.id))[0].name = this.taskName;
      this.editingTaskId = undefined;
      this.saveTasks();
    },

    deleteTask(id) {
      if (this.isEditingMode) {
        alert("編集中は削除できません");
        return;
      }

      this.tasks = this.tasks.filter((task) => task.id !== id);
      this.saveTasks();
    },

    editTask(id) {
      if (this.isEditingTaskBy(id)) {
        this.editingTaskId = undefined;
        this.taskName = "";
      } else {
        this.editingTaskId = id;
        this.taskName = this.tasks.filter((task) => task.id === id)[0].name;
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
