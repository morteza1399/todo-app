<template>
  <div
    class="min-h-screen min-w-max bg-no-repeat bg-top bg-contain sm:bg-auto dark:bg-dark-200 bg-white-200 sm:bg-white-400 sm:dark:bg-desktop-dark sm:bg-desktop-light dark:bg-mobile-dark bg-mobile-light"
  >
    <div class="sm:w-[30rem] sm:mx-auto w-80 mx-auto">
      <AppHeader />
      <AppInputBox />
      <AppTable />
      <AppReorderList />
    </div>
  </div>
</template>

<script>
import AppHeader from "@/components/AppHeader.vue";
import AppInputBox from "@/components/AppInputBox.vue";
import AppTable from "@/components/AppTable.vue";
import AppReorderList from "@/components/AppReorderList.vue";
import { mapActions } from "pinia";
import { useTodoStore } from "./pinia/todo";
export default {
  name: "App",
  components: {
    AppHeader,
    AppInputBox,
    AppTable,
    AppReorderList,
  },
  methods: {
    ...mapActions(useTodoStore, ["getTasks"]),
  },
  created() {
    this.getTasks();
  },
};
</script>
