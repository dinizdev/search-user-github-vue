<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { Icon } from '@iconify/vue';

const user = ref('');
const api = 'https://api.github.com/users/';
const isLoading = ref(false);
const isModal = ref(true);
const hasError = ref(false);
const errorMessage = ref('');
const userData = ref(null);

const handleUser = async () => {
  try {
    isLoading.value = true;
    const response = await axios.get(api + user.value);
    userData.value = response.data;
    isModal.value = false;
    isLoading.value = false;
    hasError.value = false;
  } catch (error) {
    console.error(error);
    isLoading.value = false;
    hasError.value = true;
    errorMessage.value = 'Failed to fetch user data. Please try again.';
  }
};

const handleBackModal = () => {
  setTimeout(() => {
    isModal.value = true;
    userData.value = null;
    user.value = '';
    hasError.value = false;
    errorMessage.value = '';
  }, 2000);
};
</script>

<template>
  <div class="bg-slate-800 h-screen flex justify-center items-center text-white">
    <form v-if="isModal" @submit.prevent="handleUser" id="sw"
      class="flex items-center flex-col shadow-2xl shadow-blue-600 backdrop-blur-md gap-5 bg-slate-900 p-20 text-center">
      <h1 class="text-2xl underline">Github Search Users</h1>
      <label for="user" class="flex gap-4">Insert your user below
        <Icon class="text-2xl" icon="ep:arrow-up-bold" :rotate="2" />
      </label>
      <input type="text" v-model="user" class="p-2 outline-none bg-slate-600">
      <button class="flex items-center justify-center gap-5 text-xl bg-slate-800 p-2 pointer hover:bg-slate-700"
        type="submit">
        Generate
        <Icon icon="mdi:bottle-tonic-skull" class="text-2xl" />
      </button>

      <p class="font-bold text-red-600" v-show="hasError">{{ errorMessage }}</p>
    </form>
    <div v-else class="flex flex-col justify-center items-center">
      <h1>User Information</h1>
      <ul v-if="userData">
        <li v-for="(value, key) in userData" :key="key">
          <strong>{{ key }}:</strong> {{ value }}
        </li>
      </ul>
      <a href="#" @click="handleBackModal">Back</a>
    </div>
  </div>
</template>
