<template>
  <div class="transaction-list">
    <h2>Lista de Transações</h2>
    <div class="scrollable">
      <ul>
        <li v-for="transaction in transactions" :key="transaction.id">
          {{ transaction.description }} - {{ transaction.amount }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    transactions: Array,
  },
};
</script>

<style scoped>
.scrollable {
  max-height: 400px; /* Altura máxima que você deseja para a rolagem */
  overflow-y: auto; /* Permite rolagem vertical */
  border: 1px solid #ccc; /* Borda opcional */
  padding: 10px; /* Espaçamento interno */
}
</style>
