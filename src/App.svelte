<script>
  import Board from "./Board.svelte";

  export let name;
</script>

<style>
  h1 {
    color: purple;
  }

  :global(body) {
    /* this will apply to <body> */
    margin: 0;
    font-size: 14px;
    color: #333;
  }

  :global(h1) {
    font-size: 48px;
    color: #666e78;
    font-weight: bold;
  }

  :global(strong) {
    font-size: 54px;
    color: #000;
  }

  :global(button) {
    padding: 8px 12px;
    border-radius: 4px;
    color: #fff;
    background: #6495ed;
    font-weight: bold;
  }

  :global(button):hover {
    background: #000;
  }
</style>

<Board />
