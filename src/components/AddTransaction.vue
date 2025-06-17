<template>
  <h3>Add New Transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        type="text"
        id="text"
        v-model="text"
        placeholder="Enter text...."
        :class="{ error: textError }"
        @input="textError = false"
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        v-model="amount"
        placeholder="Enter Amount...."
        :class="{ error: amountError }"
        @input="amountError = false"
      />
      <button class="btn">Add Transaction</button>
    </div>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref("");

const textError = ref(false);
const amountError = ref(false);

const toast = useToast();
const emit = defineEmits(['transactionSubmitted']);

const onSubmit = () => {
  if (!text.value) {
    toast.error("Text field cannot be empty");
    textError.value = true;
  }

  if (!amount.value) {
    toast.error("Amount field cannot be empty");
    amountError.value = true;
  }

  if (!text.value || !amount.value) {
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };
  emit('transactionSubmitted', transactionData);

  text.value = '';
  amount.value = '';
  textError.value = false;
  amountError.value = false;
};
</script>
