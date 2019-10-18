tldr: This is a tutorial that explains the basics of Svelte by building a simple Tic Tac Toe game. You can find the [demo](TODO) or [clone the repo](TODO) if you're just interested in the final application.

Let's jump right into it:

```
npx degit sveltejs/template svelte-tic-tac-toe
cd svelte-tic-tac-toe

npm install
npm run dev
```

This already sets up your "Hello World" on http://localhost:5000/

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
