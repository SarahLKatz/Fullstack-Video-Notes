## Workshop Review: Sorting

#### Bubble Sort
In Jasmine, 'toBe' is strict equality, so when comparing two arrays, you need to use 'toEqual' to see if their contents are the same.

```
function bubbleSort(array){
  var sorted = false; // this flag tells us that the array is not sorted - we will set this as true before we make a pass, and if we make a swap, that sets it to false (if it never gets set to false, the array is sorted and we can stop the for loops)
  for (var end = array.length; end > 0 && !sorted; end--) { // passes - decrement so that we don't already go through ones we've already sorted
    sorted = true;
    for (var j = 0; j < end; j++) { // bubbling
      if (!inOrder(array, j)) {
        swap(array,j)
        sorted = false
      }
    }
  }
  
  return array;
}

function inOrder(array, index) { // pure function - doesn't rely on or influence external scope
  if (index === array.length - 1) return true;
  return array[index] < array[index+1]
}

function swap(array, index) { // side effects!
  var oldLeftValue = array[index];
  array[index] = array[index+1];
  array[index+1] = oldLeftValue
}
```

#### Merge Sort
```
function mergeSort(array) {
  if (array.length < 2) return array;
  var splits = split(array),
      left = splits[0]
      right = splits[1]
  return merge(mergeSort(left), mergeSort(right))
}

function split(array) {
  var center = array.length/2;
  var left = array.slice(0,center); // if you give slice a fractional number (eg 2.5), it only uses the integer - so Math.floor() is not necessary
  var right = array.slice(center);
  return [left, right];
}

function merge(left, right) {
  var merged = [];
      leftIdx = 0;
      rightIdx = 0;

  while (leftIdx < left.length && rightIdx < right.length) {
    if (left[leftIdx] < right[rightIdx]) {
      merged.push(left[leftIdx]);
      leftIdx++;
    } else {
      merged.push(right[rightIdx]);
      rightIdx++;
    }
  }

  for (; leftIdx < left.length; leftIdx++) merged.push(left[leftIdx]);
  for (; rightIdx < right.length; rightIdx++) merged.push(right[rightIdx]);
      
  return merged;
}
```