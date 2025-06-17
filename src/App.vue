<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    <button @click="showClearConfirmation" class="btn clear-btn">Clear All Transactions</button>

    <!-- Confirmation Dialog -->
    <div v-if="showConfirmation" class="confirmation-overlay">
      <div class="confirmation-dialog">
        <h3>Confirm Clear All</h3>
        <p>Are you sure you want to clear all transactions? This cannot be undone.</p>
        <div class="dialog-actions">
          <button @click="confirmClear" class="btn confirm-btn">Yes, Clear All</button>
          <button @click="cancelClear" class="btn cancel-btn">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";

import { ref, computed, onMounted } from "vue";

const transactions = ref([]);
const showConfirmation = ref(false);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const toast = useToast();

//get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

//get income
const income = computed(() => {
  return transactions.value
    .filter((transactions) => transactions.amount > 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount;
    }, 0)
    .toFixed(2);
});

//get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transactions) => transactions.amount < 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount;
    }, 0)
    .toFixed(2);
});

//add transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  saveTransactionsToLocalStorage();

  toast.success("Transaction Added");
};

//generateUniqueId
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

//delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  saveTransactionsToLocalStorage();
  toast.success("Transaction deleted");
};

// Save transactions to local storage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};

// Clear all transactions confirmation
const showClearConfirmation = () => {
  showConfirmation.value = true;
};

const confirmClear = () => {
  transactions.value = [];
  saveTransactionsToLocalStorage();
  toast.success("All transactions cleared");
  showConfirmation.value = false;
};

const cancelClear = () => {
  showConfirmation.value = false;
};
</script>
