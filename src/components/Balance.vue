<template>
  <h4>Transfer Balance</h4>
  <h1 id="balance">
    £{{
      props.total.toLocaleString(undefined, {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      })
    }}
  </h1>
</template>

<script setup>
  import { defineProps } from 'vue'

  const props = defineProps({
    total: {
      type: Number,
      required: true
    }
  })
</script>
