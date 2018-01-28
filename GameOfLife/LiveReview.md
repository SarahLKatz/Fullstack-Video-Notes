## Game of Life: Live Review (1704)

##### Approach We're Taking:
_Brute force - look at each cell & its neighbors, figure out what each cell should be, and then update them all_  
1. Think of current state as an array of cells, then map over the array to create a new array with the new state
2. How do we construct this state? Based on each cell, we look at the neighborhood, come up with a total, and determine what the next state should be

'forEachCell' applies a function to each cell on the board
```
forEachCell: function (iteratorFunc) {
  for (let col = 0; col < this.width; col++) {
    for (let row = 0; row < this.height; row++) {
      let theCell = getCell(row,col);
      iteratorFunc(theCell, row, col);
    }
  } 
}
```

We write a 'getCell' function because we will want to get the cell somewhat regularly
```
getCell: function (row, col) {
  let theCell = document.getElementById(col + '-' + row);
  if (!theCell) return null;
  theCell.row = row;
  theCell.col = col;
  return theCell
}
```

We want to add one event listener that will apply to all cells (we do this inside 'setup board events'): - this stops us from needing to attach many many event listeners, which can be a problem on a large board and/or slow connection
```
window.board.addEventListener('click', (e) => onCellClick.call(e.target))
```

We need to get the cells in our cell's neighborhood:
```
neighborhood: function(cell) {
  let neighbors = [];
  for (let col = cell.col - 1; col <= cell.col + 1; col++) {
    for (let row = cell.row - 1; row <= cell.row + 1; row++) {
      let isCell = (row === cell.row && col === cell.col);
      if (!isCell) {
        let theCell = this.getCell(row, col)
        if (theCell) {
          neighbors.push(theCell)
        }
      }
    }
  } 
  return neighbors;
}
```

Now that we have the neighbors, we can get the next state:
```
getNextState: function(cell, row, col) {
  var livingNeighbors = this.neighborhood(cell)
    .map((el) => el.dataset.status === 'alive' ? 1 : 0)
    .reduce((sum, alive) => sum + alive, 0)
  if(cell.dataset.status === 'alive') {
    return (livingNeighbors === 2 || livingNeighbors === 3)
  } else {
    return (livingNeighbors === 3)
  }
}
```

Now we have the neighborhood and the new state - now we just need to apply it
```
applyState: function(nextState) {
  this.forEachCell((cell, row, col) => {
    let theStatus = nextState[col][row] ? 'alive' : 'dead';
    cell.className = theStatus;
    cell.dataset.status = theStatus;
  })
}
```

We have all the logic - so where do we apply it? In the step function
```
step: function() {
  let nextState = new Array(this.width).fill('').map(el => []); // we make each element of the array an array
  this.forEachCell((cell, row, col) => {
    nextState[row][col] = this.getNextState(cell, row, col) // this adds the nextState for each cell to the array
  })
  this.applyState(nextState)
}
```

Bind the event listener to the step button (in setup board events)
``` 
window.step_btn.addEventListener('click', (e) => this.step()) 
```

Next we need to enable autoplay:
```
enableAutoPlay: function() {
  if this.interval {
    clearInterval(this.interval)
    this.interval === null
  } else {
    this.interval === setInterv(() => this.step(), 250)
  }
}
```
In the board setup:
``` 
window.play_btn.addEventListener('click', (e) => this.enableAutoPlay()) 
```

To clear the board:
```
clear: function() {
  if this.interval {
    clearInterval(this.interval)
    this.interval === null
  }
  this.applyState(new Array(this.width).fill('').map(el => [])) // we're just setting a next state of all empty arrays
}
```

Randomize!
```
randomize: function(){
  let random = new Array(this.width).fill('')
    .map(el => new Array(this.height).fill('')
      .map(cell => Math.random() >= 0.5)
    )

  this.applyState(random)
}
```