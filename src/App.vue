<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';
import { ApiRecorder } from "./index";

// Function to fetch IP address
async function getIpAddress() {
  try {
    const response = await axios.get('https://api64.ipify.org?format=json');
    return response.data.ip;
  } catch (error) {
    console.error('Failed to get IP address:', error);
    return '';
  }
}

const ipAddress = ref(''); // Store the IP address

async function fetchIpAddress() {
  ipAddress.value = await getIpAddress();
}
declare function changePaqArray(object: any): void;
async function changePaqObject(obj: any[]) {
  changePaqArray(obj)
  return
}
</script>

<template>
  <div>
    <button @click="fetchIpAddress">Fetch IP Address</button>
    <p>IP Address: {{ ipAddress }}</p>
    <ApiRecorder :isRunning="false" :changePaqObject="changePaqObject" />
     <!-- <p v-else>_paq object is not available yet</p> -->
  </div>
</template>
