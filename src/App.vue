<script setup>
import FooterComponent from "./components/FooterComponent.vue";
import FormComponent from "./components/forms/FormComponent.vue";
import ListComponent from "./components/forms/ListComponent.vue";
import HeaderComponent from "./components/HeaderComponent.vue";

import { computed, ref } from "vue";

/* Abrir formulário */

const isFormOpen = ref(false);

function toggleForm() {
  isFormOpen.value = !isFormOpen.value;
}

/* lista */
const expenses = ref([
  { id: 1, title: "Cafe", value: 6, category: "food" },
  { id: 2, title: "Onibus", value: 4.5, category: "transport" },
  { id: 3, title: "Lanche", value: 12, category: "food" },
]);

/* formulario */
const title = ref("");
const value = ref("");
const category = ref("");

/* filtro */
const filter = ref("all");

/* função que filtra automaticamente */
const filtered = computed(() => {
  if (filter.value === "all") {
    return expenses.value;
  }
  return expenses.value.filter((item) => item.category === filter.value);
});

/* calcula o total */
const total = computed(() => {
  return expenses.value.reduce((sum, item) => sum + Number(item.value || 0), 0);
});

/* adicionar item na lista */
function addExpense() {
  if (!title.value.trim() || !value.value.trim()) {
    alert("Preencha tudo");
    return;
  }
  expenses.value.push({
    id: Date.now(),
    title: title.value,
    value: value.value,
    category: category.value || "other",
  });
  title.value = "";
  value.value = "";
  category.value = "";
}

/* remover item da lista */
function removeExpense(id) {
  expenses.value = expenses.value.filter((item) => item.id !== id);
}

/* limpar apenas formulario */
function clearAll() {
  title.value = "";
  value.value = "";
  category.value = "";
}
</script>
<template>
  <div class="app">
    <header>
      <!-- <button @click="toggleForm">...</button> -->
      <HeaderComponent />
    </header>
    <main>
      <ListComponent
        :filtered="filtered"
        :total="total"
        @remove-expense="removeExpense"
      />
      <section :class="['form', { hidden: !isFormOpen }]" @click.self="toggleForm">
        <div class="modal">
          <FormComponent
            v-model:title="title"
            v-model:value="value"
            v-model:category="category"
            @add-expense="addExpense"
            @clear-all="clearAll"
          />
        </div>
      </section>
    </main>
    <footer>
      <FooterComponent
        v-model:filter="filter"
        @open-form="toggleForm"
      />
    </footer>
  </div>
</template>
<style scoped>
.app {
  display: grid;
  min-height: 100vh;
  width: 100%;
  grid-template-columns: 40%;
  grid-template-rows: auto 1fr auto;
  justify-content: center;
  padding: 0 1rem;
}

header {
  position: relative;
  overflow: hidden;
  text-align: center;
  padding: 1rem;
  margin: 1rem 0;
  font-size: 1.25rem;
  border-radius: 0.5rem;
  color: white;
  box-shadow: 0 0 0.5rem black;
  background-color: rgb(22, 19, 127);
}


main {
  height: 100%;
}

footer {
  width: 100%;
  position: relative;
  overflow: hidden;
  text-align: center;
  padding: 1rem;
  margin: 1rem 0;
}

/* popup overlay */
.form {
  position: fixed;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.3);
  z-index: 1000;
}

.form.hidden {
  display: none;
}

.modal {
  background: #eee;
  border-radius: 12px;
  padding: 1.5rem;
  width: 90%;
  max-width: 400px;
  box-shadow: 0 0 0.7rem rgba(0, 0, 0, 0.4);
}

@media (max-width: 900px) {
  .app {
    grid-template-columns: 100%;
  }
}
</style>
