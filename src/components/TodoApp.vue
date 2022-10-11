<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue Todo App</h2>
    <div class="d-flex">
      <input
        type="text"
        v-model="task"
        class="form-control"
        placeholder="Enter task"
      />
      <button
        @click="submitTask"
        class="btn btn-outline-light bg-secondary rounded-1"
      >
        SUBMIT
      </button>
    </div>
    <table class="table table-bordered text-center mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col">#</th>
          <th scope="col">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ finished: task.status === 'Finished' }">{{
              task.name
            }}</span>
          </td>
          <td
            class="pointer text-white"
            :class="{
              'bg-danger': task.status === 'To-do',
              'bg-warning': task.status === 'In-progress',
              'bg-success': task.status === 'Finished',
            }"
            style="width: 120px"
          >
            <span @click="changeStatus(index)">{{ task.status }}</span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "TodoApp",
  data() {
    return {
      task: "",
      editedTask: null,
      availableStatus: ["To-do", "In-progress", "Finished"],
      tasks: [
        {
          name: "Steal Banana from store",
          status: "To-do",
        },
        {
          name: "Steal Comedy from store",
          status: "In-progress",
        },
      ],
    };
  },
  methods: {
    submitTask() {
      if (this.task.length == 0) {
        alert("Empty task cannot be submitted");
        return;
      }
      if (this.editedTask == null) {
        this.tasks.push({ name: this.task, status: "To-do" });
        console.log(this.tasks);
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = "";
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      this.tasks[index].status = this.availableStatus[(newIndex + 1) % 3];
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
  },
};
</script>

<style>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
