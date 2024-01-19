<template>
  <Header />
  <div class="container">
    <Balance :total="Number(total)" />
    <IncomeExpense :income="Number(income)" :expense="Number(expense)" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleDeleteTransaction"
    />
    <AddTransaction @transactionSubmit="handleTransaction" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";

import { ref, computed, onMounted } from "vue";

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const transactions = ref([]);

const total = computed(() => {
  return transactions.value.reduce((acc, trans) => {
    return acc + trans.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((trans) => trans.amount > 0)
    .reduce((acc, trans) => {
      return acc + trans.amount;
    }, 0)
    .toFixed(2);
});
const expense = computed(() => {
  return transactions.value
    .filter((trans) => trans.amount < 0)
    .reduce((acc, trans) => {
      return acc + trans.amount;
    }, 0)
    .toFixed(2);
});

const handleTransaction = (transactionData) => {
  transactions.value.push({
    id: crypto.randomUUID(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  saveTransactionToLocalstorage();
  notifyAddSuccess();
};

const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter((t) => t.id !== id);
  notifyDeleteSuccess();
  saveTransactionToLocalstorage();
};

const notifyDeleteSuccess = () => {
  toast("Deleted seccesfully", {
    theme: "auto",
    type: "success",
    autoClose: 2000,
    transition: "slide",
    dangerouslyHTMLString: true,
  }); // ToastOptions
};
const notifyAddSuccess = () => {
  toast("Added seccesfully", {
    theme: "auto",
    type: "success",
    autoClose: 2000,
    transition: "slide",
    dangerouslyHTMLString: true,
  }); // ToastOptions
};

const saveTransactionToLocalstorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
