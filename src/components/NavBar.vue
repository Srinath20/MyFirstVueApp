<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps(['status', 'fname', 'lname']);
const emit = defineEmits(['updateStatus', 'updateName']);

const changeStatus = () => {
  emit('updateStatus');
};

const changeName = () => {
  emit('updateName');
};
</script>

<template>
  <div class="bg-gray-50 px-6 py-4 flex flex-wrap gap-4 justify-center border-t border-gray-200">
    <a 
      href="https://google.com" 
      target="_blank"
      class="inline-flex items-center bg-gray-600 hover:bg-gray-700 text-white font-medium py-2 px-4 rounded-md transition-colors duration-300"
    >
      Visit Google
    </a>
    <button 
      @click="changeStatus" 
      class="bg-purple-600 hover:bg-purple-700 text-white font-medium py-2 px-4 rounded-md transition-colors duration-300"
    >
      Change Status
    </button>
    <button 
      @mouseover="changeName" 
      class="bg-teal-600 hover:bg-teal-700 text-white font-medium py-2 px-4 rounded-md transition-colors duration-300"
    >
      Hover for Full Name
    </button>
    <div class="mt-2 text-lg font-semibold">
      <p>Name: {{ fname }} {{ lname }}</p>
      <p>Status: {{ status }}</p>
    </div>
  </div>
</template>