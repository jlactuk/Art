<template>
    <form action="#" @submit.prevent="CreateAndCheckPost" class="bg-white p-6 rounded-lg shadow-lg max-w-md min-w-80 mx-auto mt-10 absolute top-1/2 left-1/2 -translate-y-1/2 -translate-x-1/2">
      <h3 class="text-xl font-bold text-center mb-4">Добавить пост</h3>
      
      <article class="grid gap-4">
        <input type="text" placeholder="Title post" v-model="title" class="border border-gray-300 p-3 rounded-lg w-full focus:outline-none focus:ring-2 focus:ring-blue-500">
        <textarea type="text" placeholder="Text" v-model="text" class="h-fit border border-gray-300 p-3 rounded-lg w-full focus:outline-none focus:ring-2 focus:ring-blue-500 resize-none"></textarea>
      </article>
      
      <input type="submit" value="Submit Data" class="mt-4 bg-green-500 text-white p-3 rounded-lg w-full cursor-pointer hover:bg-green-600 transition duration-300">
    </form>
  </template>
  
  <script lang="ts" setup>
    import { ref } from 'vue';
  
    const title = ref('');
    const text = ref('');
    const emit = defineEmits(['createPost']);
  
    function CreateAndCheckPost() {
      if (title.value.trim() === '' || text.value.trim() === '') {
        alert('Please fill in both fields');
        return;
      }
      emit('createPost', { title: title.value, text: text.value });
    }
  </script>
  