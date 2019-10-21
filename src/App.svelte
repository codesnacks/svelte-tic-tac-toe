<script>
  import { onMount, afterUpdate } from "svelte";

  const initialBoard = Array.from(new Array(9));
  let board = [...initialBoard];
  let winningCombination = null;

  let nextPlayer = "x";
  let winningPlayer = "";

  // split the board into columns to render them
  const rows = [[0, 1, 2], [3, 4, 5], [6, 7, 8]];

  onMount(() => {
    try {
      board =
        JSON.parse(window.localStorage.getItem("tictactoe")) || initialBoard;
      getWinner();
    } catch (e) {}
  });

  afterUpdate(function() {
    window.localStorage.setItem("tictactoe", JSON.stringify(board));
  });

  function handleClick(i) {
    if (board[i] || winningCombination) {
      return;
    }

    board[i] = nextPlayer;

    // switch player
    nextPlayer = nextPlayer === "x" ? "o" : "x";

    getWinner();
  }

  function getWinner() {
    winningCombination = checkWinningCondition(board);

    if (winningCombination) {
      winningPlayer = getWinningPlayer(winningCombination, board);
    }
  }

  function getWinningPlayer(winningCombination, board) {
    return board[winningCombination[0]];
  }

  function checkWinningCondition(board) {
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

    const get = index => board[index];

    return possibleWinningCombinations
      .filter(combination => {
        return (
          !!get(combination[0]) &&
          get(combination[0]) === get(combination[1]) &&
          get(combination[0]) === get(combination[2])
        );
      })
      .pop();
  }

  function clearState() {
    // remove the state from local storage
    localStorage.removeItem("tictactoe");

    // reset the board
    board = [...initialBoard];

    // reset the next player
    nextPlayer = "x";

    // reset the winningCombination
    winningCombination = null;
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
  h1 {
    font-size: 48px;
    color: #d3d0cb;
    font-weight: bold;
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
  .winner {
    background: #6e8898;
  }
</style>

<button on:click={clearState}>start over</button>

<h1>
  next player
  <strong>{nextPlayer}</strong>
</h1>

{#each rows as row}
  <div class="row">
    {#each row as index}
      <button
        class="square {!!winningCombination && winningCombination.includes(index) ? 'winner' : ''}"
        on:click={() => handleClick(index)}>
        {!!board[index] ? board[index] : '  '}
      </button>
    {/each}
  </div>
{/each}

{#if winningPlayer}
  <h1>
    winner
    <strong>{winningPlayer}</strong>
  </h1>
{/if}
