<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transfer in transfers"
      :key="transfer.id"
      :class="transfer.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transfer.name }} <span>£{{ transfer.amount.toFixed(2) }}</span
      ><button @click="deleteTransfer(transfer.id)" class="delete-btn">
        x
      </button>
    </li>
  </ul>
</template>

<script setup>
  import { defineProps } from 'vue'

  const emit = defineEmits(['transferDeleted'])

  const props = defineProps({
    transfers: {
      type: Array,
      required: true
    }
  })

  const deleteTransfer = (id) => {
    emit('transferDeleted', id)
  }
</script>
