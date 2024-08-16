<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses />
    <TransactionList :transactions="transactions" />
    <AddTransaction />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed } from "vue";

const transactions = ref([
  { id: 1, text: "Flowers", amount: -1500 },
  { id: 2, text: "Upkeep", amount: 2000 },
  { id: 3, text: "Pens", amount: -15 },
  { id: 4, text: "Loan", amount: 1000 },
]);

//get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

//get income


//get expenses
</script>
