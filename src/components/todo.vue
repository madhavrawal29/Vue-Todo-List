<script setup>
import { ref, onMounted, watch } from "vue";

const newTask = ref("");
const taskList = ref([]);

// Local Storage Code
watch(
  taskList,
  (addedToLocal) => {
    localStorage.setItem("task-list", JSON.stringify(addedToLocal));
  },
  { deep: true }
);

onMounted(() => {
  const storedItems = JSON.parse(localStorage.getItem("task-list"));
  if (storedItems) {
    taskList.value = storedItems;
  }
});

// Functions
const addTodoHandler = () => {
  if (newTask.value.trim() !== "") {
    taskList.value.push({ text: newTask.value, completed: false });
    newTask.value = "";
  }
};

const removeItemHandler = (index) => {
  taskList.value.splice(index, 1);
  taskList.value[index].completed = !taskList.value[index].completed;
};

const addDynamicClass = (index) => {
  taskList.value[index].completed = !taskList.value[index].completed;
};

const clearAllHandler = () => {
  taskList.value = [];
};
</script>

<template>
  <div class="main-wrapper">
    <h1>My Todo</h1>
    <input type="text" v-model="newTask" @keyup.enter="addTodoHandler" />
    <button class="add-task" @click="addTodoHandler">Add Task</button>
    <ul>
      <li
        v-for="(task, index) in taskList"
        :key="index"
        @click="addDynamicClass(index)"
        :class="{ 'task-clicked': task.completed }"
      >
        {{ task.text }}
        <button type="button" @click="removeItemHandler(index)">
          Remove Task
        </button>
      </li>
    </ul>
    <br />
    <button class="remove-all-btn" @click="clearAllHandler">Remove All</button>
  </div>
</template>

<style scoped>
.main-wrapper {
  position: relative;
  top: 30%;
  text-align: center;
  padding: 24px 70px;
  background: rgb(0, 0, 0);
  border-radius: 15px;

  h1 {
    font-family: times, Times New Roman, times-roman, georgia, serif;
    color: #cb9e9e;
    margin: 0;
    padding: 10px 0px 25px 0px;
    font-size: 65px;
    line-height: 65px;
    letter-spacing: -2px;
    font-weight: bold;
  }
  input {
    position: relative;
    cursor: text;
    font-size: 18px;
    line-height: 20px;
    padding: 0 16px;
    height: 48px;
    background-color: #fff;
    border: 1px solid #d6d6e7;
    border-radius: 3px;
    color: rgb(35, 38, 59);
    box-shadow: inset 0 1px 4px 0 rgb(119 122 175 / 30%);
    overflow: hidden;
    transition: all 100ms ease-in-out;
    margin: 10px;
    margin-left: -10px;
  }
  .add-task {
    display: inline-block;
    outline: 0;
    border: 0;
    cursor: pointer;
    color: #fff;
    font-weight: 500;
    border-radius: 4px;
    font-size: 18px;
    height: 48px;
    padding: 3px 15px;
    text-shadow: rgb(0 0 0 / 25%) 0px 3px 8px;
    transition: all 0.5s ease 0s;
    background: linear-gradient(
      92.88deg,
      rgb(69, 94, 181) 9.16%,
      rgb(86, 67, 204) 43.89%,
      rgb(103, 63, 215) 64.72%
    );
  }
  ul {
    list-style: "👻";
    padding: 12px;
    text-align: left;
    font-size: 18px;
    font-family: monospace;
    li {
      padding: 15px 0px 15px 10px;
    }

    button {
      position: absolute;
      right: 70px;
      display: inline-block;
      outline: 0;
      cursor: pointer;
      border: none;
      padding: 0 34px;
      height: 45px;
      line-height: 45px;
      border-radius: 7px;
      background-color: #248daf;
      color: white;
      font-weight: 400;
      font-size: 14px;
      box-shadow: 0 4px 14px 0 rgb(0 118 255 / 39%);
      transition: background 0.2s ease, color 0.2s ease, box-shadow 0.2s ease;
      :hover {
        background: rgba(0, 118, 255, 0.9);
        box-shadow: 0 6px 20px rgb(0 118 255 / 23%);
      }
    }
  }
  .remove-all-btn {
    display: inline-block;
    outline: none;
    cursor: pointer;
    font-size: 18px;
    line-height: 20px;
    font-weight: 600;
    border-radius: 8px;
    padding: 12px 100px;
    border: none;
    transition: box-shadow 0.2s ease 0s, -ms-transform 0.1s ease 0s,
      -webkit-transform 0.1s ease 0s, transform 0.1s ease 0s;
    background: linear-gradient(
      to right,
      rgb(230, 30, 77) 0%,
      rgb(227, 28, 95) 50%,
      rgb(215, 4, 102) 100%
    );
    color: #fff;
  }
}
button:hover {
  scale: 1.08 !important;
  transition: all 0.3s ease 0s !important;
}
.task-clicked {
  text-decoration: line-through;
  color: red;
}
</style>
