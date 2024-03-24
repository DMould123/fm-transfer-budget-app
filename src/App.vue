<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeOutgoings :income="income" :outgoings="outgoings" />
    <TransfersList :transfers="transfers" />
    <AddTransfer />
  </div>
</template>

<script setup>
  import Header from './components/Header.vue'
  import Balance from './components/Balance.vue'
  import IncomeOutgoings from './components/IncomeOutgoings.vue'
  import TransfersList from './components/TransfersList.vue'
  import AddTransfer from './components/AddTransfer.vue'

  import { ref, computed } from 'vue'

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

  // Get transfer incomes
  const income = computed(() => {
    return transfers.value
      .filter((transfer) => transfer.amount > 0)
      .reduce((acc, transfer) => {
        return acc + transfer.amount
      }, 0)
      .toFixed(2)
  })

  // Get transfer outgoings
  const outgoings = computed(() => {
    return transfers.value
      .filter((transfer) => transfer.amount < 0)
      .reduce((acc, transfer) => {
        return acc + transfer.amount
      }, 0)
      .toFixed(2)
  })
</script>
