<template>
  <div>
    <h1>Controle de Despesas</h1>
    
    <label for="year">Escolha o Ano:</label>
    <select v-model="currentYear" id="year">
      <option v-for="year in years" :key="year" :value="year">
        {{ year }}
      </option>
    </select>
    
    <label for="month">Escolha o Mês:</label>
    <select v-model="currentMonth" id="month">
      <option v-for="(month, index) in monthNames" :key="index" :value="month">
        {{ month }}
      </option>
    </select>
    
    <BalanceDisplay :transactions="currentTransactions" />
    <AddTransaction @add-transaction="addTransaction" />
    <TransactionListDisplay :transactions="currentTransactions" />
  </div>
</template>

<script>
import AddTransaction from './components/AddTransaction.vue';
import TransactionListDisplay from './components/TransactionListDisplay.vue';
import BalanceDisplay from './components/BalanceDisplay.vue';

export default {
  components: { BalanceDisplay, AddTransaction, TransactionListDisplay },
  data() {
    return {
      monthNames: [
        'Janeiro', 'Fevereiro', 'Março', 'Abril', 
        'Maio', 'Junho', 'Julho', 'Agosto', 
        'Setembro', 'Outubro', 'Novembro', 'Dezembro'
      ],
      months: {}, // Mapeia anos e meses para transações
      years: [],
      currentMonth: 'Janeiro',
      currentYear: new Date().getFullYear()
    };
  },
  computed: {
    currentTransactions() {
      const yearKey = `${this.currentYear}`;
      return this.months[yearKey]?.[this.currentMonth] || [];
    }
  },
  methods: {
    addTransaction(transaction) {
      const yearKey = `${this.currentYear}`;
      if (!this.months[yearKey]) {
        this.months[yearKey] = {};
      }
      if (!this.months[yearKey][this.currentMonth]) {
        this.months[yearKey][this.currentMonth] = [];
      }
      this.months[yearKey][this.currentMonth].push(transaction);
      this.saveTransactions();
    },
    saveTransactions() {
      localStorage.setItem('transactions', JSON.stringify(this.months));
    },
    loadTransactions() {
      const storedTransactions = JSON.parse(localStorage.getItem('transactions'));
      if (storedTransactions) {
        this.months = storedTransactions;
      }
    },
    generateYears() {
      const currentYear = new Date().getFullYear();
      this.years = Array.from({ length: 5 }, (_, i) => currentYear - i);
    }
  },
  mounted() {
    this.loadTransactions();
    this.generateYears();
  }
};
</script>

<style>
/* Estilos para o App */
h1 {
  text-align: center;
  color: #333;
}

label {
  display: block;
  margin-top: 10px;
}
</style>
