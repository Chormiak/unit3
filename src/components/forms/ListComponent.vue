<script setup>
import { defineProps, defineEmits } from "vue";
const props = defineProps({
  filtered: Array,
  total: Number,
});
defineEmits("remove-expense");

const categoryMap = {
  food: "Comida",
  transport: "Transporte",
  other: "Outros",
};

function getCategoryLabel(cat) {
  return categoryMap[cat] || cat;
}
</script>
<template>
  <!-- top summary with current balance -->
  <h2>Lista do dia</h2>
  <div class="summary">SALDO ATUAL: R$ {{ props.total }}</div>

  <!-- cards instead of table -->
  <div class="list-cards">
    <div v-for="item in props.filtered" :key="item.id" class="card">
      <div class="card-content">
        <span class="card-title">{{ item.title }}</span>
        <span class="card-category">{{ getCategoryLabel(item.category) }}</span>
        <span class="card-value">R$ {{ item.value }}</span>
      </div>
      <button class="small-btn remove-btn" @click="$emit('remove-expense', item.id)">X</button>
    </div>
  </div>
</template>

<style scoped>
h2 {
  font-size: 1.15rem;
  text-align: center;
  margin-bottom: 1rem;
  color: #555;
}

.summary {
  text-align: center;
  font-weight: bold;
  margin-bottom: 1rem;
  font-size: 1.2rem;
  background: #f0f8ff; /* light highlight */
  padding: 0.5rem;
  border-radius: 6px;
  border: 1px solid #aac;
}

.list-cards {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.card {
  border: 1px solid #ccc;
  padding: 0.75rem;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #fafafa;
  gap: 1rem;
  transition: box-shadow 0.2s, transform 0.1s;
}

.card:hover {
  box-shadow: 0 0.5rem 1rem rgba(0,0,0,0.1);
  transform: translateY(-2px);
}

.card-content {
  display: flex;
  flex-direction: row;
  gap: 1.5rem;
  align-items: center;
  flex: 1;
}

.card-title {
  font-weight: bold;
  min-width: 100px;
}

.card-category {
  font-size: 0.9rem;
  color: #555;
  flex: 1;
  text-align: center;
}

.card-value {
  font-size: 0.9rem;
  color: #555;
  min-width: 80px;
  text-align: right;
}

.remove-btn {
  background: #999;
  color: white;
  border: none;
  padding: 0.3rem 0.5rem;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.remove-btn:hover {
  background: #777;
}</style>
