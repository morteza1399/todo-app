<template>
  <tfoot class="text-sm dark:text-gray-100">
    <tr class="flex justify-between">
      <td class="py-4 ml-6 font-bold text-gray-400">{{ messageItemsLeft }}</td>
      <td class="py-4 hidden sm:flex">
        <AppFilter />
      </td>
      <td
        class="py-4 mr-6 cursor-pointer font-bold dark:hover:text-white-100 text-gray-400 hover:text-blue-300"
        @click="clearCompletedTodoItem"
      >
        Clear Completed
      </td>
    </tr>
  </tfoot>
</template>

<script>
import AppFilter from "./AppFilter.vue";
import { mapState, mapActions } from "pinia";
import { useTodoStore } from "../pinia/todo";

export default {
  name: "AppTableFooter",
  components: {
    AppFilter,
  },
  computed: {
    ...mapState(useTodoStore, ["all_tasks"]),
    activeTodoItems() {
      return this.all_tasks.filter((item) => {
        return item.isCompleted === false;
      });
    },
    messageItemsLeft() {
      if (this.activeTodoItems.length > 1) {
        return `${this.activeTodoItems.length} items left`;
      } else {
        return `${this.activeTodoItems.length} item left`;
      }
    },
  },
  methods: {
    ...mapActions(useTodoStore, ["deleteTasks"]),
    clearCompletedTodoItem() {
      for (const item of this.all_tasks) {
        if (item.isCompleted) {
          this.deleteTasks(item);
        }
      }
    },
  },
};
</script>
