
<script setup lang="ts">
import { ref, computed } from 'vue';
import axios, { AxiosRequestConfig } from 'axios';

defineProps<{
  isRunning: boolean;
  _paq: []
}>()

// Declare a variable to hold the jsonSchema
let jsonSchema: any;

// Initialize the tracking status as false
const isTracking = ref(false);
console.log("library")

// Function to toggle the tracking status
const toggleTracking = () => {
  isTracking.value = !isTracking.value;
};

function clearAndGenerateSchema(jsonObject: Record<string, any>): Record<string, any> {
  // Helper function to clear values from an object
  function clearValues(obj: Record<string, any>) {
    for (const key in obj) {
      if (typeof obj[key] === 'object' && obj[key] !== null) {
        clearValues(obj[key]); // Recurse for nested objects
      } else {
        obj[key] = {}; // Clear the value
      }
    }
  }

  // Clone the original object to avoid modifying it
  const clonedObject: Record<string, any> = JSON.parse(JSON.stringify(jsonObject));

  // Clear all values in the cloned object
  clearValues(clonedObject);

  // Generate the JSON schema
  return clonedObject;
}


axios.interceptors.request.use(
  (config) => {
    // Execute the interceptor logic only when isTracking is true
    if (isTracking.value && config.url) {
      console.log("Request is being sent:", config.method?.toUpperCase(), config.url);
      console.log("Request data:", config.data); // requestBody
      const excluded_endpoints = ['http://localhost:3000/test', 'https://api64.ipify.org?format=json'];

      // Regular expression to match URLs with IDs at the end
      const idPattern = /\/[a-f\d]{24}$/i; // Assuming IDs are 24 characters hexadecimal
      
      const possibleId = config.url?config.url.split("/").pop():"";
      // console.log("possible ", possibleId, possibleId.match(idPattern));
      
      if (!excluded_endpoints.includes(config.url?config.url:"")) {
        console.log("calling interceptor");
        
        if (config.url.match(idPattern)) {
          // URL contains an ID, remove it
          var baseUrl = config.url.replace(idPattern, '');
          console.log("base id ", baseUrl);
          baseUrl += "/:id"

          if (config.data) {
            jsonSchema = clearAndGenerateSchema(config.data);
            _paq.push(['trackEvent', 'HTTP-' + config.method, baseUrl, JSON.stringify(jsonSchema)]);
            return config;
          } else {
            _paq.push(['trackEvent', 'HTTP-' + config.method, baseUrl]);
            return config;
          }
        } else {
          console.log("base no id ", config.url);
          // URL does not contain an ID
          if (config.data) {
            jsonSchema = clearAndGenerateSchema(config.data);
            _paq.push(['trackEvent', 'HTTP-' + config.method, config.url, JSON.stringify(jsonSchema)]);
            return config;
          } else {
            _paq.push(['trackEvent', 'HTTP-' + config.method, config.url]);
            return config;
          }
        }
      }
    }

    return config;
  },
  (error) => {
    console.error("Error in request interceptor:", error);
    return Promise.reject(error);
  }
);



// Computed property to set the button text based on isTracking
const buttonText = computed(() => {
  return isTracking.value ? "Stop Tracking API" : "Track API Request";
});



interface ApiData {
  id: number;
  name: string;
  description: string;
}




</script>

<template>
<div class="bottom-right-button">
  <div class="api-tracker-container">
    <div class="topMenu">
      <button @click="toggleTracking" :class="['toggleButton', { active: isTracking }]">{{ buttonText }}</button>
    </div>
  </div>
</div>
</template>

<style scoped>
.topMenu {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #f0f0f0;
  padding: 10px;
}

.api-tracker-container {
  width: 100%;
}

.black {
  flex: 1;
  text-align: left;
  color: #000000; /* Black color for the header text */
  margin: 0;
  font-size: 24px;
}

.toggleButton {
  background-color: #008000;
  color: #ffffff;
  border: 2px solid #008000; /* Add a colored border */
  padding: 10px 20px;
  font-size: 15px;
  cursor: pointer;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Add a subtle box shadow */
  transition: background-color 0.2s ease, border-color 0.2s ease; /* Add smooth transitions */
}

.toggleButton.active {
  background-color: #d32f2f; /* Change background color to reddish one when active (isTracking=true) */
  border-color: #d32f2f; /* Change border color to match the background color */
}

.toggleButton:hover {
  background-color: #006400;
  border-color: #006400; /* Change border color on hover */
}

.toggleButton.active:hover {
  background-color: #cc0000; /* Change background color to reddish one when active (isTracking=true) */
  border-color: #d32f2f; /* Change border color to match the background color */
}
.bottom-right-button {
  position: fixed;
  bottom: 20px; /* Adjust this value to set the distance from the bottom */
  right: 20px; /* Adjust this value to set the distance from the right */
  z-index: 999; /* Ensure the button appears above other content */
}
</style>
