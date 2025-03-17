<!-- Using Composition API Style -->
<script setup>
import { ref } from "vue";

const name = ref("Joe Doe");
const isActive = ref(true);

const tasks = ref([
  {
    title: "Learn Liveview",
    status: "done",
    docs: "https://www.liveview.com",
    classStatus: "done",
  },
  {
    title: "Learn VueJs",
    status: "in progress",
    docs: "https://www.vuejs.com",
    classStatus: "pending",
  },
  {
    title: "Explore Livevue",
    status: "pending",
    docs: "https://www.livevue.com",
    classStatus: "pending",
  },
  // Add more tasks, format:
  // {
  //   title: "",
  //   status: "",
  //   docs: "https://www.example.com",
  //   classStatus: "",
  // },
]);

const toggleStatus = (task) => {
  if (task.status === "done") {
    task.status = "pending";
    task.classStatus = "pending";
  } else {
    task.status = "done";
    task.classStatus = "done";
  }
};
</script>
<template>
  <h1 class="text-white text-xl font-bold py-4 w-full bg-[#035a52] text-center">
    Welcome: {{ name }}
  </h1>
  <p class="text-center">
    User:
    <span v-if="isActive">Active</span>
    <span v-else>Offline</span>
  </p>
  <h2 class="mt-8 text-2xl">Tasks</h2>
  <ul>
    <li v-for="task in tasks" :key="task.title" class="pl-2">
      • {{ task.title }} - docs:
      <a :href="task.docs" class="underline text-[14px text-green-700">here</a>
      <button
        @click="toggleStatus(task)"
        :class="task.classStatus"
        class="inline-block mx-2 px-1"
      >
        {{ task.status }}
      </button>
    </li>
  </ul>
</template>
<style scoped>
h1:nth-child(2) {
  margin-top: 2px;
  font-weight: 600;
}
h2 {
  margin-top: 1rem;
}
li {
  display: block;
  margin-block: 0.5rem;
}
button {
  display: inline-block;
  margin-left: 1rem;
}
.done {
  background: #d8e267;
}
.pending {
  background: #ffa000;
}
</style>
