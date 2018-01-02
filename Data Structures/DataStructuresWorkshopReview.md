## Data Structures I & II Workshop Review

##### Queue (ADT):
```
// Using Arrays:
function Queue() {
  this.data = [];
  this.head = 0; // this is where you dequeue from
  this.tail = 0; // this is where you enqueue
}

Queue.prototype.enqueue = function(value) {
  this.data[this.tail] = value;
  this.tail++;
}

Queue.prototype.dequeue = function() {
  if (this.tail - this.head === 0) return;
  var value = this.data[this.head];
  if (this.head > 99) { // this is for occasional (aka amortized) garbage collection
    this.data = this.data.slice(this.head);
    this.tail = this.tail - this.head;
    this.head = 0;
  }
  this.head++;
  return value;
}

Queue.prototype.size = function() {
  return this.tail - this.head;
}

/**
Can also do this with a LinkedList - 
this.data = new LinkedList()
... .enqueue = this.data.addToTail(value)
... .dequeue = this.data.removeHead()
... .size = function () {
  var sum = 0;
  var currentNode = this.data.head;
  while (currentNode) {
    sum++;
    currentNode = currentNode.next;
  }
  return sum
}
**/

```

##### LinkedList:
```
'use strict'; // sets "this" by default to undefined

function Node(value, previous, next) {
  this.value = value;
  this.next = next || null;
  this.previous = previous || null;
}

function LinkedList() {
  this.head = null;
  this.tail = null;
}

LinkedList.prototype.addToTail = function(val) {
  var newNode = new Node(val, this.tail);
  if (this.tail) this.tail.next = newNode;
  else this.head = newNode;
  this.tail = newNode;
}

LinkedList.prototype.removeTail = function() {
  if (!this.tail) return;
  var value = this.tail.value;
  this.tail = this.tail.previous;
  if (this.tail) this.tail.next = null;
  else this.head = null;
  return value;
}

LinkedList.prototype.addToHead = function(val) {
  var newNode = new Node(val, null, this.head);
  if (this.head) this.head.previous = newNode;
  else this.tail = newNode;
  this.head = newNode;
}

LinkedList.prototype.removeHead = function() {
  if (!this.head) return;
  var value = this.head.value;
  this.head = this.head.next;
  if (this.head) this.head.previous = null;
  else this.tail = null;
  return value;
}

// utility function for search
function isFn (maybeFn) {
  return typeof maybeFn === 'function';
}


LinkedList.prototype.search = function(predicate) {
  var isCorrect = (isFn(predicate)) ? predicate : function(value) {
    return value === predicate;
  }
  var currentNode = this.head;
  while (currentNode) {
    if (isCorrect(currentNode.value)) return currentNode.value;
    currentNode = currentNode.next;
  }
  return null;
}

```

##### Hash Table:
```
function HashTable(size) {
  this.buckets = new Array(size || 25);
}

Object.defineProperty(HashTable.prototype, 'numBuckets', {
  get: function () { return this.buckets.length }
});

HashTable.prototype.set = function(key, value) {
  if (typeof key !== 'string') throw new TypeError('Keys must be strings')
  var index = this.hash(key);
  if (!this.buckets[index]) this.buckets[index] = new LinkedList();
  this.buckets[index].addToHead({ key: key, value: value })
}

HashTable.prototype.get = function(key) {
  var index = this.hash(key);
  return this.buckets[index].search(function(LLNodeVal) {
    return LLNodeVal.key === key
  }).value;
}

HashTable.prototype.hasKey = function(key) {
  var index = this.hash(key);
  return Boolean(this.buckets[index]..search(function(LLNodeVal) {
    return LLNodeVal.key === key
  }));
}

HashTable.prototype.hash = function(key) {
  var sum = 0;
  for (var i = 0; i < key.length; i++) {
    sum += key.charCodeAt(i)
  }
  return sum % this.numBuckets;
}

```

##### Binary Search Tree:
```
function BinarySearchTree(value) {
  this.value = value;
  this.magnitude = 1;
}

BinarySearchTree.prototype.insert = function(value) {
  this.magnitude++;
  var direction = value < this.value ? 'left' : 'right';
  if (!this[direction]) this[direction] = new BinarySearchTree(value)
  else this[direction].insert(value)
}

BinarySearchTree.prototype.contains = function(value) {
  if (this.value === value) return true;
  var direction = value < this.value ? 'left' : 'right';
  if (!this[direction]) return false;
  return this[direction].contains(value)
}

BinarySearchTree.prototype.depthFirstForEach = function(iterator, order) {
  if (order === 'pre-order') iterator(this.value);
  if (this.left) this.left.depthFirstForEach(iterator, order);
  if (!order || order === 'in-order') iterator(this.value);
  if (this.right) this.right.depthFirstForEach(iterator, order);
  if (order === 'post-order') iterator(this.value);
}

BinarySearchTree.prototype.breadthFirstForEach = function(iterator) {
  var queue = [this];
  var tree;
  while (queue.length) {
    tree = queue.shift();
    iterator(tree.value);
    if (tree.left) queue.push(tree.left)
    if (tree.right) queue.push(tree.right)
  }
}

BinarySearchTree.prototype.size = function() {
  return this.magnitude;
}

```