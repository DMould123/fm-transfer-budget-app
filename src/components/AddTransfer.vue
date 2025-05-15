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
import { ref, defineEmits } from "vue";
import { useToast } from "vue-toastification";

const player = ref("");
const amount = ref("");
const toast = useToast();
const emit = defineEmits(["transferSubmitted"]);

const onSubmit = () => {
  if (!player.value || !amount.value || isNaN(amount.value)) {
    toast.error("Please complete both fields with valid values");
    return;
  }

  const transferHistoryData = {
    player: player.value,
    amount: parseFloat(amount.value),
  };

  emit("transferSubmitted", transferHistoryData);

  player.value = "";
  amount.value = "";
};
</script>
