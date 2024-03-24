<template>
  <h3>Add new transfer</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="player">Player</label>
      <input
        type="text"
        id="player"
        v-model="player"
        placeholder="Enter player..."
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Transfer Fee <br />
        (outgoing - transfer fee, incoming - transfer fee)</label
      >
      <input
        type="number"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transfer</button>
  </form>
</template>

<script setup>
  import { ref } from 'vue'
  import { useToast } from 'vue-toastification'

  const player = ref('')
  const amount = ref('')
  const toast = useToast()

  const onSubmit = () => {
    if (!player.value || !amount.value) {
      toast.error('Please complete both fields')
      return
    }
    console.log(player.value, amount.value)
    player.value = ''
    amount.value = ''
  }
</script>
