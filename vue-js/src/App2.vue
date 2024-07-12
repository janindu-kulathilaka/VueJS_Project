<script setup>
import { ref, onMounted } from 'vue';

const name = ref('Janindu Kulathilaka');
const status = ref('active');
const tasks = ref(['task 1', 'task 2', 'task 3']);
const newTask = ref('');

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'pending';
  } else if (status.value === 'pending') {
    status.value = 'inactive';
  } else {
    status.value = 'active';
  }
};

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  } else {
    alert('Task cannot be empty');
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log('Error:', error);
  }
});
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">user is active</p>
  <p v-else-if="status === 'pending'">user is pending</p>
  <p v-else>user is not active</p>
  <br />

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task:</label>
    <input type="text" id="newTask" v-model="newTask" />
    <button type="submit">Add</button>
  </form>
  <br />

  <h3>
    Tasks
    <ul>
      <li v-for="(task, index) in tasks">
        <span>{{ task }}</span>
        <button @click="deleteTask(index)">x</button>
      </li>
    </ul>
  </h3>
  <br />
  <!-- <a v-bind:href="link">Click for Google</a> -->
  <a :href="link">Click for Google</a>
  <br />
  <!-- <button v-on:click="toggleStatus">Change Status</button> -->
  <button @click="toggleStatus">Change Status</button>
</template>
