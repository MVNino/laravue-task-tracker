<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li :class="transaction.amount < 0 ? 'minus' : 'plus'" v-for="transaction in transactions" :key="transaction.id">
      {{ transaction.name }} <span>${{ transaction.amount }}</span><button class="delete-btn" @click="onDelete(transaction.id)" >x</button>
    </li>
  </ul>
</template>

<script setup>
const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  }
}) 

// Emit event for transaction removal
const emit = defineEmits(['transactionDeleted'])

const onDelete = (id) => {
  emit('transactionDeleted', id)
}
</script>