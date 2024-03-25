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
</template>

<script setup>
  import Header from './components/Header.vue'
  import Balance from './components/Balance.vue'
  import IncomeOutgoings from './components/IncomeOutgoings.vue'
  import TransfersList from './components/TransfersList.vue'
  import AddTransfer from './components/AddTransfer.vue'

  import { ref, computed, defineEmits, onMounted } from 'vue'
  import { useToast } from 'vue-toastification'

  const toast = useToast()

  const transfers = ref([])

  onMounted(() => {
    const savedTransfersHistory = JSON.parse(localStorage.getItem('transfers'))

    if (savedTransfersHistory) {
      transfers.value = savedTransfersHistory
    }
  })

  // Get total transfers
  const total = computed(() => {
    return transfers.value.reduce((acc, transfer) => {
      return acc + transfer.amount
    }, 0)
  })

  // Get transfer income
  const income = computed(() => {
    return transfers.value
      .filter((transfer) => transfer.amount > 0)
      .reduce((acc, transfer) => {
        return acc + transfer.amount
      }, 0)
  })

  // Get transfer outgoings
  const outgoings = computed(() => {
    return transfers.value
      .filter((transfer) => transfer.amount < 0)
      .reduce((acc, transfer) => {
        return acc + transfer.amount
      }, 0)
  })

  const emit = defineEmits(['transferSubmitted'])

  // Add transfers
  const handleTransferSubmitted = (transferHistoryData) => {
    transfers.value.push({
      id: generateUniqueId(),
      name: transferHistoryData.player,
      amount: transferHistoryData.amount
    })

    saveTransfersToLocalStorage()

    toast.success('Transfer has been successfully added')
  }

  // Generate ID
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000)
  }

  // Delete transfer
  const handleTransferDeleted = (id) => {
    transfers.value = transfers.value.filter((transfer) => transfer.id !== id)

    saveTransfersToLocalStorage()

    toast.success('This transfer has been deleted')
  }

  // Save to localStorage
  const saveTransfersToLocalStorage = () => {
    localStorage.setItem('transfers', JSON.stringify(transfers.value))
  }
</script>
