<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transfer in transfers"
      :key="transfer.id"
      :class="transfer.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transfer.name }} <span>£{{ transfer.amount.toFixed(2) }}</span
      ><button class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
  import { defineProps } from 'vue'

  const props = defineProps({
    transfers: {
      type: Array,
      required: true
    }
  })
</script>
