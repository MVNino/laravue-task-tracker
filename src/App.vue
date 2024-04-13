<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />

    <div class="inc-exp-container">
      <Income :income="+income" />
      <Expense :expense="+expense" />
    </div>

    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <CreateTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Balance from "./components/Balance.vue";
import Expense from "./components/Expense.vue";
import Header from "./components/Header.vue"
import Income from "./components/Income.vue";
import TransactionList from "./components/TransactionList.vue";
import CreateTransaction from "./components/CreateTransaction.vue";

import { ref, computed, onMounted } from 'vue';
import { useToast } from "vue-toastification"

const toast = useToast()

const transactions = ref([])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

// Get Total Amount
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => acc + transaction.amount, 0)
})

// Get Income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
})

// Get Expense
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
})

// Add Transaction
const handleTransactionSubmitted = (transactionData) => {
  console.log('transaction data ; ', transactionData)

  transactions.value.push({
    id: generateUniqueId(),
    name: transactionData.text,
    amount: transactionData.amount,
  })

  saveTransactionsToLocalStorage()

  toast.success('Transaction added!')
}

const generateUniqueId = () => Math.floor(Math.random() * 1000000)

// Delete Transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)

  saveTransactionsToLocalStorage()

  toast.info("Transaction deleted!")
}

// Transaction in Local Storage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>