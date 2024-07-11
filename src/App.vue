<script setup>
import { onMounted, ref } from 'vue'

const name = ref('Alaa Tourkey');
const status = ref('active');
const tasks = ref(['one', 'two', 'three']);
const link = ref('https://google.com');
const count = ref(0);
const newTask = ref('');

const changeStatus = () => {
  switch (status.value) {
    case 'active':
      status.value = 'pending';
      break;
    case 'pending':
      status.value = 'not active';
      break;
    default:
      status.value = 'active';
  }
};

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value.trim());
    newTask.value = '';
  }
};
const deleteTask = (index) => {
  tasks.value.splice(index, 1)
}

console.log(`count ${count.value}`);
count.value++;
console.log(`count ${count.value}`);


onMounted(async () => {
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/todos`)
    const data = await response.json();
    tasks.value = data.map( (task)=>task.title )
  }
  catch {
    console.log('erorrrrrrrrrrrrrrrrr');
  }
})
</script>

<template>
  <div class="container ">
    <h1>{{ name }}</h1>

    <form @submit.prevent="addTask">
      <label for="newTask">New Task:</label>
      <input v-model="newTask" id="newTask" placeholder="Write here">
      <button type="submit">Submit</button>
    </form>

    <h3>Tasks</h3>
    <ul>
      <li v-for="(task, index) in tasks" :key="task">
        <span>{{ task }}</span>
        <button @click="deleteTask(index)">x</button>
      </li>
    </ul>

    <div class="status">
      <h2 v-if="status === 'active'">User is active</h2>
      <h2 v-else-if="status === 'pending'">User is pending</h2>
      <h2 v-else>User is not active</h2>
    </div>

    <button @click="changeStatus">Change Status</button>

    <a :href="link">Google</a>
  </div>
</template>

<style>
.container {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: auto;
  padding: 1rem;
}

h1 {
  color: rgb(221, 134, 19);
  text-align: center;
}

form {
  background-color: cornflowerblue;
  padding: 1.5rem;
  border-radius: 5px;
  margin-bottom: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

label {
  margin-bottom: 0.5rem;
  font-weight: bold;
}

input {
  padding: 0.5rem;
  border: none;
  border-radius: 3px;
  margin-bottom: 1rem;
  width: 100%;
}

button {
  padding: 0.5rem 1rem;
  background-color: darkblue;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

button:hover {
  background-color: navy;
}

h3 {
  margin-top: 1.5rem;
  text-align: center;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  background-color: rgb(61, 59, 59);
  padding: 0.5rem;
  margin-bottom: 0.5rem;
  border-radius: 3px;
}

.status {
  margin-top: 1.5rem;
  text-align: center;
}

a {
  display: block;
  margin: 1.5rem 0;
  text-align: center;
  color: darkblue;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>
