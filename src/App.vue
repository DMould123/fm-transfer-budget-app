<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeOutgoings
      :income="parseFloat(income)"
      :outgoings="parseFloat(outgoings)"
    />
    <TransfersList
      :transfers="transfers"
      @transferDeleted="handleTransferDeleted"
    />
    <AddTransfer @transferSubmitted="handleTransferSubmitted" />
  </div>
  <div class="starting-balance">
    <label for="startingBalance">Starting Balance (£):</label>
    <input
      type="number"
      id="startingBalance"
      v-model="startingBalance"
      min="0"
      step="0.01"
    />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeOutgoings from "./components/IncomeOutgoings.vue";
import TransfersList from "./components/TransfersList.vue";
import AddTransfer from "./components/AddTransfer.vue";

import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const transfers = ref([]);
const startingBalance = ref(1000);

onMounted(() => {
  const savedTransfersHistory = JSON.parse(localStorage.getItem("transfers"));
  const savedStartingBalance = parseFloat(
    localStorage.getItem("startingBalance")
  );

  if (savedTransfersHistory) {
    transfers.value = savedTransfersHistory;
  }
  if (!isNaN(savedStartingBalance)) {
    startingBalance.value = savedStartingBalance;
  }
});

// Get total transfers
const total = computed(() => {
  return (
    startingBalance.value +
    transfers.value.reduce((acc, transfer) => acc + transfer.amount, 0)
  );
});

// Get transfer income
const income = computed(() => {
  return transfers.value
    .filter((transfer) => transfer.amount > 0)
    .reduce((acc, transfer) => {
      return acc + transfer.amount;
    }, 0);
});

// Get transfer outgoings
const outgoings = computed(() => {
  return transfers.value
    .filter((transfer) => transfer.amount < 0)
    .reduce((acc, transfer) => {
      return acc + transfer.amount;
    }, 0);
});

const emit = defineEmits(["transferSubmitted"]);

// Add transfers
const handleTransferSubmitted = (transferHistoryData) => {
  transfers.value.push({
    id: generateUniqueId(),
    name: transferHistoryData.player,
    amount: transferHistoryData.amount,
  });
  saveTransfersToLocalStorage();
  toast.success("Transfer has been successfully added");
};

// Generate ID
const generateUniqueId = () => {
  return Date.now();
};

// Delete transfer
const handleTransferDeleted = (id) => {
  transfers.value = transfers.value.filter((transfer) => transfer.id !== id);
  saveTransfersToLocalStorage();
  toast.success("This transfer has been deleted");
};

// Save to localStorage
const saveTransfersToLocalStorage = () => {
  localStorage.setItem("transfers", JSON.stringify(transfers.value));
};
</script>
