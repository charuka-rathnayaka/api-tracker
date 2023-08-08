<script setup lang="ts">
import { computed } from "vue";
type ApiRecorderProps = {
  size?: number;
  color?: string;
};

const props = withDefaults(defineProps<ApiRecorderProps>(), {
  size: 16,
  color: "skyblue",
});

const fontSize = computed(() => `${props.size}px`);
</script>

<template>
  <button id="apiRecorder">
    <slot />
  </button>
</template>

<style>
#apiRecorder {
  padding: 1rem 2rem;
  cursor: pointer;
  border: none;
  background-color: v-bind(color);
  font-size: v-bind(fontSize);
}
</style>
