<script>
  // creates an array with 9 undefined entries
  let board = Array.from(new Array(9));
  // player x is going to start
  let nextPlayer = "x";

  function handleClick(i) {
    console.log("clicked", i);

    // alternate between players
    nextPlayer = nextPlayer === "x" ? "o" : "x";
  }
</script>

<style>
  @import url("https://fonts.googleapis.com/css?family=Shadows+Into+Light&display=swap");

  :global(*),
  :global(button) {
    font-family: "Shadows Into Light", cursive;
    background: #2e5266;
    color: #e2c044;
    text-align: center;
    font-size: 48px;
  }
  .row {
    height: 45px;
    display: flex;
    justify-content: center;
  }
  .square {
    padding: 0;
    width: 45px;
    height: 45px;
    font-size: 24px;
    border: 1px solid #d3d0cb;
  }
</style>

<div class="row">
  <button class="square" on:click={() => handleClick(0)} />
  <button class="square" on:click={() => handleClick(1)} />
  <button class="square" on:click={() => handleClick(2)} />
</div>
<div class="row">
  <button class="square" on:click={() => handleClick(3)} />
  <button class="square" on:click={() => handleClick(4)} />
  <button class="square" on:click={() => handleClick(5)} />
</div>
<div class="row">
  <button class="square" on:click={() => handleClick(6)} />
  <button class="square" on:click={() => handleClick(7)} />
  <button class="square" on:click={() => handleClick(8)} />
</div>
