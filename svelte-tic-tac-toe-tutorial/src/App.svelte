<script>
  // creates an array with 9 undefined entries
  let board = Array.from(new Array(9));
  // player x is going to start
  let nextPlayer = "x";

  // split the board into columns to render them
  const rows = [[0, 1, 2], [3, 4, 5], [6, 7, 8]];

  const possibleWinningCombinations = [
    // rows
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    // columns
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    // diagonals
    [0, 4, 8],
    [6, 4, 2]
  ];

  function checkWinningCondition() {
    return possibleWinningCombinations
      .filter(combination => {
        return (
          !!board[combination[0]] &&
          board[combination[0]] === board[combination[1]] &&
          board[combination[0]] === board[combination[2]]
        );
      })
      .pop();
  }

  function handleClick(i) {
    // set the symbol of the "current" player on the board
    board[i] = nextPlayer;

    // alternate between players
    nextPlayer = nextPlayer === "x" ? "o" : "x";

    console.log(checkWinningCondition());
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

{#each rows as row}
  <div class="row">
    {#each row as index}
      <button class="square" on:click={() => handleClick(index)}>
        {!!board[index] ? board[index] : '  '}
      </button>
    {/each}
  </div>
{/each}
