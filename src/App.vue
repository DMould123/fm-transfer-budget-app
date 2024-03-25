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

  import { ref, computed, defineEmits } from 'vue'
  import { useToast } from 'vue-toastification'

  const toast = useToast()

  const transfers = ref([
    { id: 1, name: 'Cristiano Ronaldo', amount: 100000000.0 },
    { id: 2, name: 'Lionel Messi', amount: -120500000.0 },
    { id: 3, name: 'Neymar Jr.', amount: 150250000.0 },
    { id: 4, name: 'Kylian Mbappe', amount: -180750000.99 },
    { id: 5, name: 'Kevin De Bruyne', amount: 90000000.0 },
    { id: 6, name: 'Virgil van Dijk', amount: -85000000.69 }
  ])

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

    toast.success('Transfer has been successfully added')
  }

  // Generate ID
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000)
  }

  // Delete transfer
  const handleTransferDeleted = (id) => {
    transfers.value = transfers.value.filter((transfer) => transfer.id !== id)

    toast.success('This transfer has been deleted')
  }
</script>
