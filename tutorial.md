tldr: This is a tutorial that explains the basics of Svelte by building a simple Tic Tac Toe game. You can find the [demo](TODO) or [clone the repo](https://github.com/codesnacks/svelte-tic-tac-toe) if you're just interested in the final application.

Let's jump right into it:

### Setup

```
npx degit sveltejs/template svelte-tic-tac-toe
cd svelte-tic-tac-toe

npm install
npm run dev
```

This already sets up your "Hello World" application on http://localhost:5000/

If you look at the folder structure you'll discover a `src` folder with a `main.js` and an `App.svelte` file. `App.svelte` contains the `App` component, which we will extend in this first part of the tutorial.

So let's open this file:

```svelte
<script>
	export let name;
</script>

<style>
	h1 {
		color: purple;
	}
</style>

<h1>Hello {name}!</h1>
```

As you can see this component consists of thee sections:

- script
- style
- markup

Each of these sections is optional, but we'll need them for our game.

### Global Styles

Let's first drop in some global styles to make the whole application and little bit more appealing later on. We'll start with a font and some colors:

```svelte
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
</style>
```

### The Board

Let's start with writing some markup and CSS to create our board and clean up the rest of the file. We'll need three `rows` with three `squares` each. We'll use a flexbox for the rows to display the squares next to each other.

```svelte
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
  <button class="square" />
  <button class="square" />
  <button class="square" />
</div>
<div class="row">
  <button class="square" />
  <button class="square" />
  <button class="square" />
</div>
<div class="row">
  <button class="square" />
  <button class="square" />
  <button class="square" />
</div>
```

This already gives us a nice board with the needed squares as clickable buttons. Cool! But of course nothing happens when we click the buttons. So let's add an event handler. We do this by adding the script section again to the top of the file. And adding the handler to the markup of one of the buttons.

```svelte
  <script>
    function handleClick() {
      console.log("clicked");
    }
  </script>

  /* ... style and other markup ... */

  <button class="square" on:click={handleClick} />
```

So far so good! Now we need to pass some arguments to the clickHandler. We do this by wrapping an anonymous function around the `handleClick` function and pass the needed argument.

```svelte
  <script>
    function handleClick(i) {
      console.log("clicked", i);
    }
  </script>

  /* ... style and other markup ... */

  <button class="square" on:click={() => handleClick(1)} />
```

Perfect! So let's add an index to all of the squares, that we can pass to the `handleClick` function.

```svelte
<script>
  function handleClick(i) {
    console.log("clicked", i);
  }
</script>

/* ... styles ... */

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
```

We can now distinguish between all of the buttons, when we click them. To save the state of the clicked buttons we'll add a JS representation of the board in the script section. It'll be a simple array with a length of 9. It'll contain undefined if no player has made a move on that square, otherwise it'll contain the symbol of the player `x` or `o`.

We'll also add a `nextPlayer` variable, to know who's turn it is. This variable will just be `x` or `o`.

```svelte
<script>
  // creates an array with 9 undefined entries
  let board = Array.from(new Array(9));
  // player x is going to start
  let nextPlayer = "x";

  function handleClick(i) {
    console.log("clicked", i);
  }
</script>
```

To show whose turn it is, we'll add a headline to the markup, that contains the nextPlayer variable. To output a JS variable in the markup a set of curly braces is needed.

```
<h1>
  next player
  <strong>{nextPlayer}</strong>
</h1>
```

Let's now get to the fun part of actually writing the symbol of the player to the board and alternating between the players.

To make this work, we first need to adjust the square to actually reflect the state of the `board` variable:

```svelte
<div class="row">
  <button class="square" on:click={() => handleClick(0)}>
    {!!board[0] ? board[0] : ''}
  </button>
  <button class="square" on:click={() => handleClick(1)}>
    {!!board[1] ? board[1] : ''}
  </button>
  <button class="square" on:click={() => handleClick(2)}>
    {!!board[2] ? board[2] : ''}
  </button>
</div>
<div class="row">
  <button class="square" on:click={() => handleClick(3)}>
    {!!board[3] ? board[3] : ''}
  </button>
  <button class="square" on:click={() => handleClick(4)}>
    {!!board[4] ? board[4] : ''}
  </button>
  <button class="square" on:click={() => handleClick(5)}>
    {!!board[5] ? board[5] : ''}
  </button>
</div>
<div class="row">
  <button class="square" on:click={() => handleClick(6)}>
    {!!board[6] ? board[6] : ''}
  </button>
  <button class="square" on:click={() => handleClick(7)}>
    {!!board[7] ? board[7] : ''}
  </button>
  <button class="square" on:click={() => handleClick(8)}>
    {!!board[8] ? board[8] : ''}
  </button>
</div>
```

This is quite tedious, but we'll come up with a nicer solution later on.

We'll now focus on change the `board` with the click handler.

```
  function handleClick(i) {
    // set the symbol of the "current" player on the board
    board[i] = nextPlayer;

    // alternate between players
    nextPlayer = nextPlayer === "x" ? "o" : "x";
  }
```

This already gives us a fully working Tic Tac Toe Board!

Now let's make the markup of the board a bit more flexible. We'll introduce a `rows` variable in the script section to get this done:

```svelte
  // split the board into columns to render them
  const rows = [[0, 1, 2], [3, 4, 5], [6, 7, 8]];
```

In the markup we iterate over the rows and sqaures. We can use the `#each` tag to do this:

```svelte
{#each rows as row}
  <div class="row">
    {#each row as index}
      <button class="square" on:click={() => handleClick(index)}>
        {!!board[index] ? board[index] : '  '}
      </button>
    {/each}
  </div>
{/each}
```

### Winning Condition

One of the problems our game still has is that you can continue after a player has won. That's because we didn't implement any winning condition yet. So let's do this now.

We have to check after every move, if the winning condition is met. So we'll add this to the `handleClick` function and implement the `checkWinningCondition` function.

But let's start with defining the winning conditions themselves:

```svelte
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
```

`possibleWinningCombinations` now contains all three in a row combinations by the index of the squares. Let's use this in our `checkWinningConditions` function.

```svelte
  function checkWinningCondition() {
    return possibleWinningCombinations
      .filter(combination => {
        return (
          !!board[combination[0]] &&
          board[combination[0]] === board[combination[1]] &&
          board[combination[0]] === board[combination[2]]
        );
      })
      // will contain the winning combination or undefined
      .pop();
  }

  function handleClick(i) {
    // set the symbol of the "current" player on the board
    board[i] = nextPlayer;

    // alternate between players
    nextPlayer = nextPlayer === "x" ? "o" : "x";

    // and let's output the winning combination if there is any
    console.log(checkWinningCondition());
  }
```

So as soon as you have three in a row the application will not log the winning combination. Quite cool! But let's make this a bit more obvious by highlighting the squares.

---

### Displaying the winner

So `winningPlayer` is either In this case we need some conditional rendering.

```
{#if winningPlayer}
  <h1>
    winner
    <strong>{winningPlayer}</strong>
  </h1>
{/if}
```

### component composition

To keep our code a bit cleaner we can create separate components to separate concerns. We can for example think of an **application** that contains the **board**, which consists of 9 **squares**. Additionally we can have some **controls** to reset the board and some kind of **display** that shows whose turn it is.

To make this work we need to pass data from one component down to its children. To do that, we need to declare properties, generally shortened to props.

Let's start by creating the Board-component and passing down the necessary props. We'll create a new file called `Board.svelte`.

```


```

### Saving the state

If you're actually following along this tutorial you might have noticed, that the board is cleared every time you make a code change. This is because the application is automatically updated and reloaded to reflect the latest changes, which in turn discards the state of your application.

So how do we solve that? Let's do this in a very simple fashion and use the localStorage of your browser to persist the state.

We will use the _lifecycle hooks_ of svelte for this. First we will read the state as soon as the application is mounted. And second we will update the state whenever a change was done. Let's start with the board:

```
<script>
  import { onMount } from "svelte";

  // instead of initializing the board directly we will hvae it as a variable
  const initialBoard = [
    [false, false, false],
    [false, false, false],
    [false, false, false]
  ];

  // and we will assign the initial board to the actual board
  let board = initialBoard;

  onMount(() => {
    try {
      board =
        JSON.parse(window.localStorage.getItem("tictactoe")) || initialBoard;
      console.log("state board", board);
    } catch (e) {}
  });
```

---

# NOTES

// Note that the array has no property keys apart from length.
new Array(9)

---

If you're interested in Svelte related topics, then my upcoming free **Svelte for Beginners Course** might be interesting for you. You can subscribe to my mailing list under TODO to be notified as soon as it's available.

Thanks for reading! If you have any questions or suggestions just drop me a line in the comments!

---

animate the `winner` headline when winning to demonstrate the animation capabilities
