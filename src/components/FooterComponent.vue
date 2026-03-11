<script setup>
import { defineProps, defineEmits } from "vue";
const props = defineProps({ filter: String });
const emit = defineEmits(["update:filter", "open-form"]);

function setFilter(category) {
  emit("update:filter", category);
}

function toggleForm() {
  emit("open-form");
}
</script>
<template>
  <nav>
    <button
      :class="['small-btn', { active: props.filter === 'all' }]"
      @click="setFilter('all')"
    >
      Tudo
    </button>
    <button
      :class="['small-btn', { active: props.filter === 'food' }]"
      @click="setFilter('food')"
    >
      Comida
    </button>
    <button
      :class="['small-btn', { active: props.filter === 'transport' }]"
      @click="setFilter('transport')"
    >
      Transporte
    </button>
    <button
      :class="['small-btn', { active: props.filter === 'other' }]"
      @click="setFilter('other')"
    >
      Outros
    </button>
    <button class="open-form-btn" @click="toggleForm()">+</button>
  </nav>
</template>
<style scoped>
nav {
  /* width: 100%; */
  display: flex;
  justify-content: center;
  align-items: end;
  gap: 0.5rem;
}

button {
  flex: 1;
  padding: 0.5rem;
  border-radius: 0.5rem;
  height: 2rem;
  background-color: #ccc;
  color: #333;
  box-shadow: 0 0 0.3rem rgba(0, 0, 0, 0.2);
  transition: scale 0.05s;
  border: none;
  font-weight: 500;
}

.open-form-btn {
  flex: 0 0 3.5rem;
  height: 3.5rem;
  background-color: #2a6df4;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  box-shadow: 0 0 0.8rem rgba(42, 109, 244, 0.4);
  font-weight: bold;
}

.open-form-btn:hover {
  background-color: #1f4fc7;
  scale: 1.1;
}

button:hover {
  scale: 1.05;
  background-color: #aaa;
}
button:active {
  scale: 0.95;
  opacity: 70%;
}

/* indicate selected filter */
.small-btn.active {
  background-color: #888;
  color: white;
}
</style>
