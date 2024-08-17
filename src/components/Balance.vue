<template>
  <h4>Your Balance</h4>
  <h1 id="balance">ksh{{ total.toLocaleString() }}</h1>
</template>

<script setup>
const props = defineProps({
  total: {
    type: Array,
    requried: true,
  },
});
</script>
