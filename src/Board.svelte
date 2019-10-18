<script>
  import { onMount, afterUpdate } from "svelte";

  const initialBoard = Array.from(new Array(9));

  let board = initialBoard;

  onMount(() => {
    try {
      board =
        JSON.parse(window.localStorage.getItem("tictactoe")) || initialBoard;
      console.log("state board", board);
    } catch (e) {}
  });

  afterUpdate(function() {
    window.localStorage.setItem("tictactoe", JSON.stringify(board));
  });

  let currentPlayer = "x";
  let winningPlayer = "";

  function handleFieldClick(i) {
    board[i] = currentPlayer;

    // switch player
    currentPlayer = currentPlayer === "x" ? "o" : "x";

    const winningCombination = checkWinningCondition(board);

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

    // and reset the board
    board = initialBoard;
  }

  // create chunks of the board to get rows
  // const rows = board
  //   .map((_, i) => i)
  //   .reduce((acc, x) => {
  //     if (x % 3 === 0) {
  //       acc.push([x]);
  //       return acc;
  //     } else {
  //       console.log(typeof acc, acc.length);
  //       acc[acc.length - 1].push(x);
  //       return acc;
  //     }
  //   }, []);

  // way simpler
  const rows = [[0, 1, 2], [3, 4, 5], [6, 7, 8]];
</script>

<style>
  div {
    height: 25px;
    display: flex;
  }
  span {
    cursor: pointer;
    width: 25px;
    height: 25px;
    display: inline-flex;
    /* text-align: center; */
    justify-content: center;
    align-items: center;
    border: 1px solid #aaa;
    box-shadow: inset 0 0 10px rgba(200, 200, 200, 0.8);
    color: #000;
    font-weight: 600;
  }
</style>

<h1>
  next player
  <strong>{currentPlayer}</strong>
</h1>

{#each rows as row}
  <div>
    {#each row as index}
      <span on:click={() => handleFieldClick(index)}>
        {!!board[index] ? board[index] : '  '}
      </span>
    {/each}
  </div>
{/each}

{#if winningPlayer}
  <h1>
    winner
    <strong>{winningPlayer}</strong>
  </h1>
{/if}

<button on:click={clearState}>start over</button>
