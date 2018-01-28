## Game of Life

This workshop uses "this" and .bind()
_this_ is the call site or context of where a function is invoked.

_.bind_ changes the "this" context of a given function. '.bind' does NOT invoke the function.

#### Manipulating the DOM
JavaScript was originally created for manipulating the DOM.

Can add Event Handlers using JavaScript. Events in JS propogate down and the bubble up. When you click, it registers at the top level of the DOM tree and then goes down the tree looking for click listeners, and when it finds one, it will execute the first click listener it finds.

"Lexical this" - with arrow functions, the "this" inside the function is the same as the "this" outside the function.

#### Cellular Automata
* Science of Computable Modeling
* Discrete, rule-based systems
* Using computers to model the thin barrier between order and chaos
* Wolfram, "A New Kind of Science"
  * Looks at a series of rules - a black and white grid, and each square's state (black or white) is determined by its three parent squares

**Conway's Game of Life** - a simple animation game based on a simple means of simulating life and ecosystems.
##### Rules:
* Cells are currently on or off (alive or dead)
* Each step, grid updates all at once
* Currently Alive Cell:
  * "Underpopulation" - dies given fewer than 2 live neighbors
  * "Overcrowding" - dies given greater than 3 live neighbors
  * Otherwise, lives on
* Currently Dead Cell:
  * "Birth" - comes to life given exactly 3 live neighbors
  * Otherwise, remains dead