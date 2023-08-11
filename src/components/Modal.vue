<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps<{
  show: Boolean,
}>()

const emit = defineEmits();
const sessionName = ref('');
const loadingData = ref(false);
const errorMessage = ref('');
const isPopoverVisible = ref(false);

const startSession = () => {
  if (sessionName.value.trim().length === 0) {
    errorMessage.value = 'Session name cannot be empty';
    isPopoverVisible.value = true;
    return; // Don't proceed if validation fails
  }
  // Emit the sessionName to the parent component
  emit('start-session', sessionName.value);
};

const cancelSession = () => {
  emit('cancel-session'); // Emit an event to close the modal
};
</script>



<template>
  <Transition name="modal">
    <div v-if="props.show" class="modal-mask">
      <div class="modal-container" style="max-height: 80vh; overflow-y: auto;">
        <div class="modal-header">
          <h3>Track API Activities</h3>
        </div>

        <div class="modal-body">
          <div class="form-group">
            <label for="session-name">Session Name:</label>
            <input v-model="sessionName" id="session-name" type="text" />
          </div>
          <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
        </div>
        
        <div class="modal-footer">
          <slot name="footer">
            <button class="modal-cancel-button" @click="cancelSession">Cancel</button>
            <button class="modal-default-button" @click="startSession">Start Session</button>
          </slot>
        </div>
      </div>
    </div>
  </Transition>
</template>


<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  transition: opacity 0.3s ease;
}

.modal-container {
  width: auto;
  margin: auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
  text-align: left; /* Align the header to the left */
}

.modal-body {
  margin: 20px 0;
  margin-bottom: 30px;
}

.modal-default-button {
  background-color: #42b983;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  float: right;
}

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

.modal-cancel-button {
  background-color: #ccc;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  float: left;
  margin-right: 10px; /* Adjust margin as needed */
}
.form-group {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 10px;
  align-items: center;
}

/* Adjust the styles for the input field, label, and error message as needed */
.form-group label {
  margin: 0;
  grid-column: 1;
}

.form-group input {
  grid-column: 2;
  width: 100%;
  border: 2px solid #ccc; /* Add a border */
  padding: 8px; /* Add some padding for better spacing */
  border-radius: 4px; /* Add rounded corners */
}


.error-message {
  grid-column: span 2; /* Span both columns */
  color: #f44336;
  font-size: 14px;
  margin-top: 5px;
}
</style>