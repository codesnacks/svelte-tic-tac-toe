<script>
  let count = 0;

  function handleClick() {
    count += 1;
  }

  let board = [
    [false, false, false],
    [false, false, false],
    [false, false, false]
  ];

  let things = [
    { id: 1, color: "#0d0887" },
    { id: 2, color: "#6a00a8" },
    { id: 3, color: "#b12a90" },
    { id: 4, color: "#e16462" },
    { id: 5, color: "#fca636" }
  ];

  let currentPlayer = "x";

  function handleFieldClick(i, j) {
    console.log(i, j);
    board[i][j] = currentPlayer;

    currentPlayer = currentPlayer === "x" ? "o" : "x";

    checkWinningCondition();
  }

  function checkWinningCondition() {
    const reduce = (acc, i) => (acc != String(i) ? acc + i : acc);
    // const rows = board.map(row => row.reduce(reduce, ""));
    const rows = board.map(row => row.join(""));

    const size = board[0].length;
    let columns = ["", "", ""];
    for (let i = 0; i < size; i++) {
      for (let j = 0; j < size; j++) {
        columns[i] = columns[i] + board[j][i];
      }
    }

    console.log(rows);
    console.log(columns);
    // console.log(currentPlayer, "won");
  }
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
    border: 1px solid #aaa;
    display: inline-block;
    text-align: center;
  }
</style>

<button on:click={handleClick}>
  Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
<h1>
  player
  <strong>{currentPlayer}</strong>
</h1>
{#each board as row, i}
  <div>
    {#each row as field, j}
      <span on:click={() => handleFieldClick(i, j)}>
        {!!field ? field : '  '}
      </span>
    {/each}
  </div>
{/each}

{#each things as thing (thing.id)}{thing.color}{/each}
