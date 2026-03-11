<script setup>
import { defineProps, defineEmits, computed } from "vue";

const props = defineProps({
  title: String,
  value: String,
  category: String,
});

const emit = defineEmits([
  "update:title",
  "update:value",
  "update:category",
  "add-expense",
  "clear-all",
]);

// v-model helpers to keep parent values in sync
const modelTitle = computed({
  get: () => props.title,
  set: (v) => emit("update:title", v),
});

const modelValue = computed({
  get: () => props.value,
  set: (v) => emit("update:value", v),
});

// Format value as currency for display
const modelValueFormatted = computed({
  get: () => {
    if (!props.value) return "";
    const num = parseFloat(props.value);
    if (isNaN(num)) return "";
    return "R$ " + num.toLocaleString("pt-BR", {
      minimumFractionDigits: 2,
      maximumFractionDigits: 2,
    });
  },
  set: (v) => {
    // Remove tudo que não é número ou ponto/vírgula
    const cleaned = v.replace(/[^0-9.,]/g, "").replace(/,/g, ".");
    emit("update:value", cleaned || "");
  },
});

const modelCategory = computed({
  get: () => props.category,
  set: (v) => emit("update:category", v),
});
</script>

<template>
  <h2>Nova despesa</h2>
  <p>
    <label>Título</label>
    <input v-model="modelTitle" class="input" placeholder="Descricao" />
  </p>
  <p>
    <label>Valor</label>
    <input v-model="modelValueFormatted" class="input" placeholder="R$ 0,00" />
  </p>
  <p>
    <label>Categoria</label>
    <select v-model="modelCategory" class="input">
      <option value="food">Comida</option>
      <option value="transport">Transporte</option>
      <option value="other">Outros</option>
    </select>
  </p>
  <div class="row">
    <button class="small-btn" @click="$emit('add-expense')">Adicionar</button>
    <button class="small-btn" @click="$emit('clear-all')">Limpar tudo</button>
  </div>
</template>

<style scoped>
h2 {
  background: #2a6df4;;
  color: white;
  padding: 0.5rem;
  border-radius: 6px 6px 0 0;
  text-align: center;
  margin: -1rem -1rem 1rem;
}

.input {
  width: 100%;
  padding: 0.4rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.row {
  display: flex;
  gap: 0.5rem;
  justify-content: space-between;
  margin-top: 1rem;
}

.small-btn {
  flex: 1;
}
</style>
