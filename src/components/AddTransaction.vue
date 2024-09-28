<template>
  <form @submit.prevent="handleSubmit">
    <input type="text" v-model="description" placeholder="Descrição" required />
    <input type="number" v-model="amount" placeholder="Valor" required />
    <select v-model="type">
      <option value="despesa">Despesa</option>
      <option value="rendimento">Rendimentos</option>
    </select>
    <button type="submit">Adicionar</button>
  </form>
</template>


<script>
export default {
  data() {
    return {
      description: '',
      amount: '',
      type: 'despesa'
    };
  },
  methods: {
    handleSubmit() {
      const transaction = {
        description: this.description,
        amount: parseFloat(this.amount),
        type: this.type,
        date: new Date().toLocaleDateString()
      };
      this.$emit('add-transaction', transaction);
      this.description = '';
      this.amount = '';
    }
  }
};
</script>
