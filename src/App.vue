<template>
  <Header />
  <div class="container">
    <Balance />
    <IncomeOutgoings />
    <TransfersList />
  </div>
</template>

<script>
  import Header from './components/Header.vue'
  import Balance from './components/Balance.vue'
  import IncomeOutgoings from './components/IncomeOutgoings.vue'
  import TransfersList from './components/TransfersList.vue'
  export default {
    components: {
      Header,
      Balance,
      IncomeOutgoings,
      TransfersList
    }
  }
</script>
