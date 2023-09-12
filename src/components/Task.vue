<script setup>

import { ref } from 'vue'

const props = defineProps({
  taskText: String,
  showCheckBtn: Boolean,
})

const emit = defineEmits(['delete-task', 'check-task'])
const input = ref()

</script>

<template>

<div class="task">
  <div v-text="taskText" ref="input" class="text"></div>
  <img v-if="showCheckBtn" @click="emit('check-task')" src="../assets/icons/check.svg" alt="check" class="check">
  <img @click="emit('delete-task')" src="../assets/icons/delete.svg" alt="close" class="delete">
</div>

</template>

<style scoped>

img {
  width: 2.4rem;
  height: 2.4rem;
  cursor: pointer;
}

.task {
  width: 50rem;
  padding: 1.2rem 0.8rem;
  background-color: #fff;
  display: flex;
  margin-bottom: 0.8rem;
}

.text {
  flex: 1;
  align-self: center;
  outline: none;
}

.check {
  margin-left: 0.8rem;
}

.delete {
  margin-left: 0.8rem;
}

</style>

