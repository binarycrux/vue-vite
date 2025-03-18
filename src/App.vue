<!-- Using Composition API Style -->
<script setup>
import { ref, onMounted, computed } from "vue";

const name = ref("Guest");
const isActive = ref(true);
const newTask = ref("");
const inputError = ref(false);
const inputStyle = ref("border-[0.5px] px-2 py-[3px] w-[80%] outline-0");
const inputBorder = ref("border-[#035a52]");
const inputBorderDanger = ref("border-red-700");

// Array of tasks
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

// Disable to fetch tasks from an online fake Rest api
// onMounted(async () => {
//   try {
//     const response = await fetch("https://jsonplaceholder.typicode.com/todos");
//     const data = await response.json();
//     tasks.value = data.slice(0, 10).map((todo) => ({
//       title: todo.title,
//       status: todo.completed ? "done" : "pending",
//       classStatus: todo.completed ? "done" : "pending",
//     }));
//   } catch (error) {
//     console.log("Error fetching tasks!");
//   }
// });

//toggle task status
const toggleStatus = (task) => {
  if (task.status === "done") {
    task.status = "pending";
    task.classStatus = "pending";
  } else {
    task.status = "done";
    task.classStatus = "done";
  }
};

// add new task to the task array
const addTask = () => {
  if (newTask.value !== "") {
    tasks.value.push({
      title: newTask.value,
      status: "pending",
      classStatus: "pending",
    });
    inputError.value = false;
    newTask.value = "";
  } else {
    inputError.value = true;
  }
};

// delete task from the array by passing the task id/index
const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

// clear all tasks
const clearTasks = () => {
  tasks.value = [];
};

// Live clock
const currentTime = ref("");

const updateClock = () => {
  const date = new Date();
  let h = date.getHours();
  let m = date.getMinutes();
  let s = date.getSeconds();
  let session = "AM";

  if (h == 0) {
    h = 12;
  }

  if (h > 12) {
    h = h - 12;
    session = "PM";
  }

  h = h < 10 ? "0" + h : h;
  m = m < 10 ? "0" + m : m;
  s = s < 10 ? "0" + s : s;

  currentTime.value = h + ":" + m + ":" + s + " " + session;
};

onMounted(() => {
  updateClock();
  setInterval(updateClock, 1000);
});
</script>
<template>
  <h1 class="text-white text-xl font-bold py-4 w-full bg-[#035a52] text-center">
    Welcome: {{ name }}
  </h1>
  <p class="user-status text-right pr-2">
    User:
    <span v-if="isActive">Online</span>
    <span v-else>Offline</span>
    <span><b class="font-bold"> Time:</b> {{ currentTime }}</span>
  </p>
  <main class="pl-4">
    <form @submit.prevent="addTask" class="w-[99%] sm:w-[50%]">
      <label for="newtask">Add new activity*</label><br />
      <div class="w-full">
        <input
          type="text"
          name="newtask"
          id="newtask"
          v-model="newTask"
          :class="[inputError ? inputBorderDanger : inputBorder, inputStyle]"
        />
        <button type="submit" class="bg-[#035a52] px-4 py-1 text-white w-[20%]">
          Add
        </button>
      </div>
      <template v-if="inputError"
        ><span class="text-red-700">Can't be blank!</span></template
      >
    </form>
    <h2 class="mt-10 text-xl">Your Tasks:</h2>
    <ul>
      <li v-for="(task, index) in tasks" :key="task.title" class="pl-2">
        {{ index + 1 }}. {{ task.title }}
        <span v-if="task.docs"
          >- docs:
          <a :href="task.docs" class="underline text-[14px] text-green-700"
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
    <button @click="clearTasks" class="bg-red-500 px-2 py-1 text-white mt-4">
      Clear All Tasks
    </button>
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
b {
  font-weight: 600;
  display: inline-block;
  margin-left: 0.5rem;
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
