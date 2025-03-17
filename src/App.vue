<!-- Using Composition API Style -->
<script setup>
import { ref } from "vue";

const name = ref("Joe Doe");
const isActive = ref(true);
const newTask = ref("");

const addTask = () => {
  if (newTask.value !== "") {
    tasks.value.push({
      title: newTask.value,
      status: "pending",
      classStatus: "pending",
    });
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};
const tasks = ref([
  {
    title: "Learn Liveview",
    status: "done",
    docs: "https://www.liveview.com",
    classStatus: "done",
  },
  {
    title: "Learn VueJs",
    status: "on progress",
    docs: "https://www.vuejs.com",
    classStatus: "pending",
  },
  {
    title: "Explore Livevue",
    status: "pending",
    docs: "https://www.livevue.com",
    classStatus: "pending",
  },
  // Add more persistent tasks, format:
  // {
  //   title: "",
  //   status: "",
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
  <p class="user-status text-right pr-2">
    User:
    <span v-if="isActive">Active</span>
    <span v-else>Offline</span>
  </p>
  <main class="pl-4">
    <form @submit.prevent="addTask">
      <label for="newtask">Add new activity*</label><br />
      <input
        type="text"
        name="newtask"
        id="newtask"
        v-model="newTask"
        class="border-[0.5px] border-[#035a52] px-2 py-[3px] w-[50%] focus:border-[0.5px] focus:outline-0"
      />
      <button type="submit" class="bg-[#035a52] px-4 py-1 text-white">
        Add
      </button>
    </form>
    <h2 class="mt-10 text-xl">Your Tasks:</h2>
    <ul>
      <li v-for="(task, index) in tasks" :key="task.title" class="pl-2">
        {{ index + 1 }}. {{ task.title }}
        <span v-if="task.docs"
          >- docs:
          <a :href="task.docs" class="underline text-[14px text-green-700"
            >here</a
          ></span
        >
        <button
          @click="toggleStatus(task)"
          :class="task.classStatus"
          class="toggle-status px-1"
        >
          {{ task.status }}
        </button>
        <span
          @click="deleteTask(index)"
          class="btn-delete text-[#ff3500] inline-block px-2 cursor-default font-extrabold bg-[#e8eae5]"
          >X</span
        >
      </li>
    </ul>
  </main>
</template>
<style scoped>
h1:nth-child(2) {
  margin-top: 2px;
  font-weight: 600;
}
h2 {
  margin-top: 1rem;
}
.user-status {
  font-weight: 600;
}
form {
  display: block;
  margin-top: 1rem;
}
li {
  display: block;
  margin-block: 0.5rem;
}
button.toggle-status {
  display: inline-block;
  margin-left: 1rem;
}
.done {
  background: #d8e267;
}
.pending {
  background: #ffa000;
}
.btn-delete {
  display: inline-block;
  margin-left: 4px;
  font-weight: bold;
}
</style>
