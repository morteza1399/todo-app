<template>
  <div
    class="flex items-center my-6 h-14 rounded dark:bg-dark-100 dark:text-white-200 bg-white-400 text-dark-100"
  >
    <AppCircle />
    <input
      autocomplete="off"
      class="w-full h-full rounded ml-2 focus:outline-none dark:bg-dark-100 bg-white-400 caret-blue-200"
      type="text"
      name="new-todo"
      id="new-todo"
      placeholder="Create a new todo..."
      v-model="task"
      @keyup.enter="addedTask"
    />
  </div>
</template>

<script>
import AppCircle from "./AppCircle.vue";
import { mapState, mapActions } from "pinia";
import { useTodoStore } from "../pinia/todo";

export default {
  name: "AppInputBox",
  data() {
    return {
      task: "",
      spaceRegex: /^\s+$/,
    };
  },
  components: {
    AppCircle,
  },
  computed: {
    ...mapState(useTodoStore, ["all_tasks"]),
  },
  methods: {
    ...mapActions(useTodoStore, ["postTasks"]),
    addedTask() {
      this.task = this.truncateString(this.task, 20);
      let tasks = this.all_tasks.map((item) => item.name);
      this.task = this.task.trim();
      if (
        this.task.length === 0 ||
        this.spaceRegex.test(this.task) ||
        tasks.includes(this.task)
      )
        return;
      this.postTasks({
        name: this.task,
        isCompleted: false,
      })
        .then(() => {
          this.task = "";
        })
        .catch((err) => {
          alert(err.message);
        });
    },
    truncateString(str, num) {
      if (str.length > num) {
        return str.slice(0, num) + "...";
      } else {
        return str;
      }
    },
  },
};
</script>
