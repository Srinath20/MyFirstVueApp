<script setup>
import { ref, onMounted, watch } from 'vue';
import Navbar from '@/components/NavBar.vue';
import TaskEditModal from './components/TaskEditModal.vue';

const name = ref("Srinath");
const status = ref("active");
const tasks = ref([]);
const fname = ref("Dummy");
const lname = ref("Test");
const newTask = ref('');

const isModalOpen = ref(false);
const editIndex = ref(null);
const editTask = ref('');

const displayedTasks = ref([]);
const showAllTasks = ref(false);

watch(tasks, (newTasks) => {
  if (showAllTasks.value) {
    displayedTasks.value = [...newTasks];
  } else {
    displayedTasks.value = newTasks.slice(0, 10);
  }
}, { deep: true });

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
    updateDisplayedTasks();
  }
};

const deleteTask = (index) => {
  const actualIndex = showAllTasks.value ? index : index;
  tasks.value.splice(actualIndex, 1);
  updateDisplayedTasks();
};

const openEditModal = (index) => {
  const actualIndex = showAllTasks.value ? index : index;
  editIndex.value = actualIndex;
  editTask.value = tasks.value[actualIndex];
  isModalOpen.value = true;
};

const handleUpdateTask = ({ index, task }) => {
  tasks.value[index] = task;
  closeModal();
  updateDisplayedTasks();
};

const closeModal = () => {
  isModalOpen.value = false;
  editIndex.value = null;
  editTask.value = '';
};

const updateDisplayedTasks = () => {
  if (showAllTasks.value) {
    displayedTasks.value = [...tasks.value];
  } else {
    displayedTasks.value = tasks.value.slice(0, 10);
  }
};

onMounted(async () => {
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await res.json();
    tasks.value = data.map(task => task.title);

    updateDisplayedTasks();
  } catch (error) {
    console.log("Error while fetching data: " + error);
  }
});

const showMore = () => {
  displayedTasks.value = [...tasks.value];
  showAllTasks.value = true;
};

const showLess = () => {
  displayedTasks.value = tasks.value.slice(0, 10);
  showAllTasks.value = false;
};

const updateStatus = () => {
  status.value = status.value === 'active' ? 'pending' : status.value === 'pending' ? 'inactive' : 'active';
};

const updateName = () => {
  fname.value = "Gollapinni";
  lname.value = "Srinath";
};
</script>

<template>
  <div class="min-h-screen bg-gray-100 py-8 px-4 sm:px-6 lg:px-8">
    <Navbar 
      :status="status" 
      :fname="fname" 
      :lname="lname" 
      @updateStatus="updateStatus" 
      @updateName="updateName" 
    />
    
    <div class="max-w-3xl mx-auto bg-white rounded-lg shadow-md overflow-hidden">
      <div class="bg-gradient-to-r from-blue-500 to-indigo-600 py-6 px-8">
        <h1 class="text-4xl font-bold text-white">{{ fname }} {{ lname }}</h1>
        <h2 class="text-3xl font-medium text-blue-100 mt-2">{{ name }}</h2>
        
        <div class="mt-4 flex justify-center">
          <button 
            v-if="tasks.length > 10 && !showAllTasks" 
            @click="showMore" 
            class="bg-gray-200 hover:bg-gray-300 text-gray-800 font-medium py-2 px-4 rounded-md transition-colors duration-300"
          >
            Show All ({{ tasks.length }})
          </button>
          <button 
            v-if="showAllTasks" 
            @click="showLess" 
            class="bg-gray-200 hover:bg-gray-300 text-gray-800 font-medium py-2 px-4 rounded-md transition-colors duration-300"
          >
            Show Less
          </button>
        </div>
      </div>
      
      <div class="p-6">
        <form @submit.prevent="addTask" class="mb-8">
          <label for="newT" class="block text-sm font-medium text-gray-700 mb-1">Add Task</label>
          <div class="flex">
            <input 
              type="text" 
              id="newT" 
              name="nT" 
              v-model="newTask" 
              class="flex-1 rounded-l-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 px-4 py-2 border"
              placeholder="Enter a new task..."
            />
            <button 
              type="submit" 
              class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-2 px-4 rounded-r-md transition-colors duration-300"
            >
              Add
            </button>
          </div>
        </form>
        
        <div class="bg-gray-50 rounded-lg p-4">
          <h2 class="text-xl font-semibold text-gray-800 mb-4">Tasks:</h2>
          <ul class="space-y-2">
            <template v-for="(task, index) in displayedTasks" :key="index">
              <li class="flex items-center justify-between bg-white p-3 rounded-md shadow-sm border border-gray-200 hover:shadow-md transition-shadow duration-200">
                <span class="flex-1 text-gray-700">{{ task }}</span>
                <div class="flex space-x-2">
                  <button @click="openEditModal(index)" class="text-blue-600 hover:text-blue-800 px-2 py-1 rounded hover:bg-blue-100 transition-colors duration-200">
                    Edit
                  </button>
                  <button @click="deleteTask(index)" class="text-red-600 hover:text-red-800 px-2 py-1 rounded hover:bg-red-100 transition-colors duration-200">
                    Delete
                  </button>
                </div>
              </li>
            </template>
            <li v-if="displayedTasks.length === 0" class="text-center py-4 text-gray-500">
              No tasks to display
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>

  <TaskEditModal 
    :isOpen="isModalOpen"
    :task="editTask"
    :taskIndex="editIndex"
    @update="handleUpdateTask"
    @close="closeModal"
  />
</template>