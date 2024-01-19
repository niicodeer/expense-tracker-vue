<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="handleSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="text" type="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        v-model="amount"
        type="text"
        id="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add Transaction</button>
  </form>
</template>
<script setup>
import { ref } from "vue";
import { toast } from "vue3-toastify";

const text = ref("");
const amount = ref("");

const emit = defineEmits(["transactionSubmit"]);

const notifyError = () => {
  toast("Both fields must be filled", {
    theme: "auto",
    type: "error",
    autoClose: 2000,
    transition: "slide",
    dangerouslyHTMLString: true,
  }); // ToastOptions
};
const handleSubmit = () => {
  if (!text.value || !amount.value) {
    notifyError();
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };
  emit("transactionSubmit", transactionData);

  text.value = "";
  amount.value = "";
};


</script>
