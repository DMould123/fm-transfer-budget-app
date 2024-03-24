<template>
  <div class="inc-out-container">
    <div>
      <h4>Income</h4>
      <p id="money-plus" class="money plus">+£{{ props.income }}</p>
    </div>
    <div>
      <h4>Outgoings</h4>
      <p id="money-minus" class="money minus">-£{{ props.outgoings }}</p>
    </div>
  </div>
</template>

<script setup>
  import { defineProps } from 'vue'

  const props = defineProps({
    income: {
      type: Number,
      required: true
    },
    outgoings: {
      type: Number,
      required: true
    }
  })
</script>
