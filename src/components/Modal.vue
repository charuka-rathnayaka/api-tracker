<script setup lang="ts">
  import { ref, defineProps, onBeforeMount,   defineEmits  } from 'vue';

  const { show } = defineProps({
    show: Boolean,
  });

  const emit = defineEmits();
  const sessionName = ref('');

  const loadingData = ref(false);

  const startSession = () => {
    // Emit the sessionName to the parent component

    emit('start-session', sessionName.value);
  };
</script>



<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-container" style="max-height: 80vh; overflow-y: auto;">
        <div class="modal-header">
          <h3>Track API Activities</h3>
        </div>

        <div class="modal-body">
            <label for="session-name" :style="{ marginRight: '30px' }">Session Name:</label>
            <input v-model="sessionName" id="session-name" type="text" />
        </div>

        <div class="modal-footer">
          <slot name="footer">

            <button
              class="modal-default-button"
              @click="startSession"
            >Start Session</button>
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
}

.modal-body {
  margin: 20px 0;
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
table {
  width: 100%;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

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
.table-heading {
  background-color: #f2f2f2;
  font-weight: bold;
  padding: 8px;
}
.table-h {
  margin: 10px;
  text-align: center;
}

.table-data {
  padding: 8px;
  border-bottom: 1px solid #ddd;
  float: center;
  text-align: left;
  
}
.loading-spinner {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
}

.spinner {
  border: 4px solid rgba(66, 185, 131, 0.2);
  border-top: 4px solid #42b983; /* Greenish color */
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}



</style>