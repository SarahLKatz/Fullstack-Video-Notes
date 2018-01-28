## Game of Life: Review Videos

#### Video 1: Boilerplate Intro
The table body comes from our javascript - it's not in our HTML.  
Using template literals for our HTML makes it cleaner - this is an ES6 feature!

#### Video 2: On Cell Click
onCellClick - on click event handler - it takes in an event and executes that event when the cell is clicked

The logic for changing the status is already given to us.
```
var onCellClick = function(e) {
  if (this.dataset.status == 'dead') {
    this.className = 'alive';
    this.dataset.status = 'alive';
  } else {
    this.className = 'dead';
    this.dataset.status = 'dead';
  }
}
```

How do we set an onCellClick for each cell? We loop through the entire board, and then assign the click event to each cell (the first cell is already done for us, but we can get rid of that because our for loop includes that cell). This is our forEachCell method.
```
forEachCell: function (iteratorFunc) {
  var cell;
  for (var i = 0; i < this.height; i++) {
    for (var j = 0; j < this.width; j++) {
      cell = document.getElementById(`${j}-${i})
      iteratorFunc(cell, i, j); 
    }
  }
}
```

Here we use forEachCell by passing it a function that adds the onCellClick function:
```
this.forEachCell(function(cell,i,j) {
  cell.click = onCellClick;
})
```

In forEachCell, we used "document.getElementById" heightxwidth times (in our example, that's 12x12), which means we're traversing the DOM many times (here, 144 times). What if we could just traverse the DOM once? We know that all of the cells are td's, so we can grab them by tag name
```
documents.getElementByTagName('td') // returns an HTML collection
var tags = Array.from(documents.getElementByTagName('td')) // this converts it to an array (new ES6 feature)
```
The new forEachCell method:
```
forEachCell: function (iteratorFunc) {
  var self = this; // We do this because the "this" context doesn't go into the forEach, so we need to assign it to a variable and then run the getCoordsOfCell method off of this new variable, not the 'this' object
  Array.from(documents.getElementByTagName('td')).forEach(function(cell){
    var coords = self.getCoordsOfCell(cell);
    iteratorFunc(cell, coords[0], coords[1]); 
  })
}
```
As seen above, we need to be able to get the coordinates of a cell:
```
getCoordsOfCell: function(cell) {
  var cellId = cell.id; // This will be a string (eg - '0-0')
  var idSplit = cellId.split('-') // Returns ['0', '0']

  return idSplit.map(function(str){
    return parseInt(str, 10);
  })
}
```

#### Video 3: Utility Methods
getCoordsOfCell and forEachCell are both utility methods we plan on using several times.  
There are also additional utility methods that we need. Our code to change the cell's status is a little clunky - so let's create a getCellStatus, a setCellStatus, and a toggleCellStatus function.  

```
getCellStatus: function(cell){
  return cell.getAttribute('data-status');
}
```

```
setCellStatus: function(cell, status){
  cell.className = status;
  cell.setAttribute('data-status', status)
}
```

Now that we have some utility functions, we can use them in our onCellClick function:
```
var gameOfLifeObj = this;
var onCellClick = function(e) {
  if (gameOfLifeObj.getCellStatus(this) == 'dead') { // Here, 'this' is the cell that is currently executing the onCellClick function
    gameOfLifeObj.setCellStatus(this, 'alive');
  } else {
    gameOfLifeObj.setCellStatus(this, 'dead');
  }
}
```

toggleCellStatus uses a lot of the logic on onCellClick
```
toggleCellStatus: function(cell){
  if (this.getCellStatus(cell) == 'dead') { // Here, we're calling this method directly on the GOL object, so we can use 'this'
    this.setCellStatus(cell, 'alive');
  } else {
    this.setCellStatus(cell, 'dead');
  }
}
```

Now we can just put this function into our onCellClick:
```
var gameOfLifeObj = this;
var onCellClick = function(e) {
  gameOfLifeObj.toggleCellStatus(this)
}
```

#### Video 4: Step Fn, Part 1
We need to set up the button events in the setupBoardEvents function:
```
document.getElementById("step-btn").addEventListener('click', function(e){
  gameOfLifeObj.step()
})
```

We call our step function - so we have to create a step function.
The step function needs to iterate through our entire board, go through each cell, and at each cell, determine how many alive neighbors, and then figure out (based on the current status and the number of alive neighbors) what the next status should be.
```
step: function() {
  var gameOfLifeObj = this;
  // We need to access each cell - so we use forEachCell
  this.forEachCell(function(cell,x,y){
    // We want to find a cell's neighbors. For this, we're using a utility function that we will create (see below), 'getNeighbors'
    // Once we have the neighbors, we want to know if they're alive - so we create a utility function, getAliveNeighbors (which actually calls getNeighbors, so we don't have to call it separately)
    gameOfLifeObj.getAliveNeighbors(cell)
  })
}
```

getNeighbors function - takes in the cell and finds all of the surrounding neighbors to see if they're alive or dead. We know that each cell will have up to 8 neighbors (8 for most cells, fewer for edge cases), so we can find the neighbors with just the x and y coordinates - we don't need iteration
```
getNeighbors: function(cell){
  var neighbors = [];
  var thisCellCoords = this.getCoordsOfCell; // this gives us the cell's x and y value
  cellX = thisCellCoords[0];
  cellY = thisCellCoords[1];

  // We want to find the cells directly to the left and right of our cell
  neighbors.push(this.selectCell(cellX+1, cellY));
  neighbors.push(this.selectCell(cellX-1, cellY));

  // Next we find the three directly above our current cell
  neighbors.push(this.selectCell(cellX+1, cellY-1));
  neighbors.push(this.selectCell(cellX, cellY-1));
  neighbors.push(this.selectCell(cellX+1, cellY-1));

  // Last we find the three directly below our current cell
  neighbors.push(this.selectCell(cellX+1, cellY+1));
  neighbors.push(this.selectCell(cellX, cellY+1));
  neighbors.push(this.selectCell(cellX+1, cellY+1));

  return neighbors.filter(function(neighbor) {
    return neighbor !== null
  });
}
```

selectCell function - returns a cell based on the x and y coordinates
```
selectCell: function(x,y) {
  return document.getElementById(`${x}-${y}`)
}
```

getAliveNeighbors
```
getAliveNeighbors: function(cell) {
  var gameOfLifeObj = this;
  var allNeighbors = this.getNeighbors(cell)
  return allNeighbors.filter(function(neighbor){
    return gameOfLifeObj.getCellStatus(neighbor) === 'alive'
  })
}
```

#### Video 5: Step Fn, Part 2
Step Function:
```
step: function() {
  var gameOfLifeObj = this;
  var cellsToToggle = []; // We create an array for cells that need to be toggled because we want to be able to go through each cell before we start changing things
  this.forEachCell(function(cell,x,y){
    var countLiveNeighbors = gameOfLifeObj.getAliveNeighbors(cell).length;
    if (gameOfLifeObj.getCellStatus(cell) === 'alive') {
      if (countLiveNeighbors !== 2 && countLiveNeighbors !== 3) {
        cellsToToggle.push(cell);
      }
    } else {
      if (countLiveNeighbors === 3) {
        cellsToToggle.push(cell);
      }
    }
  })

  cellsToToggle.forEach(function(cellToToggle){ // Now that we've gone through all of the cells, we can start toggling cells
    gameOfLifeObj.toggleCellStatus(cellToToggle)
  })
}
```

#### Video 5: Clear, Play, Randomize - Buttons
One of the easiest functions to implement is the clear board - it just toggles each cell to dead
```
clearBoard: function(){
  this.forEachCell(function(cell){
    this.setCellStatus(cell, "dead")
  }).bind(this)
  this.stopAutoPlay();
}
```
Event listener (in setupBoardEvents):
```
document.getElementById("clear-btn").addEventListener('click', function(e){
  gameOfLifeObj.clearBoard()
})
```

Reset Random visits each individual cell and randomizes set cell status - 50% alive, 50% dead
```
document.getElementById("reset-btn").addEventListener('click', function(e){
  gameOfLifeObj.resetRandom()
})
```
```
resetRandom: function(){
  this.forEachCell(function(cell){
    if (Math.random() > .5) {
      this.setCellStatus(cell, 'alive')
    } else {
      this.setCellStatus(cell, 'dead')
    }
  }).bind(this)
}
```

"Play" is our autoplay function - it shows generation after generation without having to press 'step'
```
document.getElementById("play-btn").addEventListener('click', function(e){
  gameOfLifeObj.enableAutoPlay()
})
```
```
enableAutoPlay: function(){
  // If there's already an interval, setting an interval will cause odd behavior. We want to check if there is an interval set, and if there is, we clear it before setting the interval
  if (this.stepInterval) {
    this.stopAutoPlay();
  }
  this.stepInterval = setInterval(this.step().bind(this),500)
},
stopAutoPlay: function(){
  clearInterval(this.stepInterval);
  this.stepInterval = null;
}
```