<template>
  <div class="balance">
    <h2>Saldo: {{ formattedBalance }}</h2>
  </div>
</template>



<script>
export default {
  props: ['transactions'],
  computed: {
    balance() {
      return this.transactions.reduce((acc, transaction) => {
        return transaction.type === 'rendimento'
          ? acc + transaction.amount
          : acc - transaction.amount;
      }, 0);
    },
    formattedBalance() {
      return `R$ ${this.balance.toFixed(2)}`;
    }
  }
};
</script>
