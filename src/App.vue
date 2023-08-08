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
declare var _paq: any;
_paq = window._paq;
console.log("paq",_paq)

// Function to handle button click
async function fetchIpAddress() {
  ipAddress.value = await getIpAddress();
}
</script>

<template>
  <div>
    <button @click="fetchIpAddress">Fetch IP Address</button>
    <p>IP Address: {{ ipAddress }}</p>
    <ApiRecorder :isRunning="false" :_paq="_paq"/>
  </div>
</template>
