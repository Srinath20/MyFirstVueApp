<script setup>
import { defineProps, defineEmits, ref, watch } from 'vue';

const props = defineProps({
  isOpen: Boolean,
  task: String,
  taskIndex: Number
});

const emit = defineEmits(['update', 'close']);

const editedTask = ref('');

watch(() => props.task, (newTask) => {
  editedTask.value = newTask;
});

const updateTask = () => {
  if (editedTask.value.trim() !== '') {
    emit('update', {
      index: props.taskIndex,
      task: editedTask.value
    });
    emit('close');
  }
};

const closeModal = () => {
  emit('close');
};
</script>

<template>
  <div 
    v-if="isOpen" 
    class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50 p-4"
  >
    <div 
      class="bg-white rounded-lg shadow-xl w-full max-w-md transform transition-all"
    >
      <!-- Modal Header -->
      <div class="bg-gradient-to-r from-blue-500 to-indigo-600 px-6 py-4 rounded-t-lg">
        <h3 class="text-xl font-medium text-white">Edit Task</h3>
      </div>
      
      <!-- Modal Body -->
      <div class="p-6">
        <div class="mb-4">
          <label for="editedTask" class="block text-sm font-medium text-gray-700 mb-1">Task:</label>
          <input 
            type="text" 
            id="editedTask" 
            v-model="editedTask"
            class="w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 px-4 py-2 border"
          />
        </div>
      </div>
      
      <!-- Modal Footer -->
      <div class="px-6 py-4 bg-gray-50 flex justify-end space-x-3 rounded-b-lg border-t border-gray-200">
        <button 
          @click="closeModal"
          class="px-4 py-2 bg-gray-200 text-gray-800 rounded-md hover:bg-gray-300 transition-colors duration-300"
        >
          Cancel
        </button>
        <button 
          @click="updateTask"
          class="px-4 py-2 bg-indigo-600 text-white rounded-md hover:bg-indigo-700 transition-colors duration-300"
        >
          Save Changes
        </button>
      </div>
    </div>
  </div>
</template>