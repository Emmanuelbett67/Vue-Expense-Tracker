<template>
  <h3>History</h3>
  <transition-group name="list" tag="ul" id="list" class="list" v-if="transactions.length">
    <li
      v-for="t in transactions"
      :key="t.id"
      :class="t.amount < 0 ? 'minus' : 'plus'"
    >
      {{ t.text }} <span> ksh {{ t.amount.toLocaleString() }}</span>
      <button @click="deleteTransaction(t.id)" class="delete-btn">
        x
      </button>
    </li>
  </transition-group>
  <p v-else class="no-transactions-message">No transactions yet. Add some above!</p>
</template>

<script setup>
const props = defineProps({
  transactions: {
    type: Array,
    requried: true,
  },
});

const emit = defineEmits(['transactionDeleted']);

const deleteTransaction = (id) => {
  emit('transactionDeleted', id);
};
</script>
