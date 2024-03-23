<template>
  <h3>Add new transfer</h3>
  <form id="form">
    <div class="form-control">
      <label for="player">Player</label>
      <input type="text" id="player" placeholder="Enter player..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Transfer Fee <br />
        (outgoing - transfer fee, incoming - transfer fee)</label
      >
      <input type="number" id="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transfer</button>
  </form>
</template>
