<template>
  <div class="inc-exp-container">
        <div>
          <h4>Income</h4>
          <p id="money-plus" class="money plus">+ksh{{ income }}</p>
        </div>
        <div>
          <h4>Expense</h4>
          <p id="money-minus" class="money minus">-ksh{{ expenses }}</p>
        </div>
      </div>

</template>

<script setup>
const props = defineProps({
  income:{
    type: Number,
    required: true
  },
  expenses:{
    type: Number,
    required: true
  },
});
</script>
