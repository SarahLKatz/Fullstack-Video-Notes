## Data Structures II (Corey)

Abstract data types are not language-specific.
An abstract data type is what we conceptually want to build, a data structure is an implementation of that thing we wanted to build.

#### Dictionary/Map
* We want to be able to relate two pieces of information - a key (eg, a word) and a value (eg, a definition)
* Need to be able to quickly modify, access, and delete a piece of information
* Keys must be unique
* Get and set values for key, add new pairs, delete pairs - want to be able to do all of this in O(1) runtime
* Implementation: Hash Table
  * Translate any key into an array index via a function (called hashing)
  * Example (not great) hashing function:
  ```
    function hash(keyString) {
      var hashed = 0;
      for (var i = 0; i < keyString.length; i++) {
        hashed += keyString.charCodeAt(i);
      }
      return hashed % 9 // We modulo 9 here because that's the number of buckets in our array
    }
  ```
  * Hashing the same string must always result in the same value
  * Two strings could has to the same value, which could lead to collisions. How do we handle collisions?
    1. Open Addressing: If a bucket is full, move to the next available bucket.
    2. Separate Chaining: Every bucket stores a secondary data structure (for example, a linked list), and collisions create new entries in that data structure. If we are going to use a linked list in a bucket, why wouldn't we just start with a linked list and skip the hash table? A linked list of all these values could get very long - the hash table shortens the linked list (and the lookup time) by dividing it up into buckets. Finding the right bucket is O(1), and then you're traversing a much smaller linked list. We store a key/value pair in the linked list so that we know which node represents which key.
      * Association List - special type of linked list that allows us to store key/value pairs
      * If you want to use a regular linked list, you can just store an array ([key,value]) as the value in the linked list. This, however, is a little more complicated, because you need to remember that the 0th index is the key and the 1st index is the value
      * We also could just store a struct (which is like an object but cannot be modified). Every time you want to modify, you'd have to create a new struct (but our linked list could be modified to point to the new struct)
* Implementing in JavaScript: JS Engines (like V8) implement most objects as structs, but sometimes it will be a hash table. We don't really know which one will happen behind the scenes. But it's important to remember that either one can happen. This is why objects have an O(1) lookup time.

#### Trees
* We will be linking things via some roots, going along branches, and ending in leaves. **A tree is an abstract data type**.
* A tree has a root node. Each node contains a value and has relationships to children (ie branches) with values, each of which is its own tree. Every final node (ie has no children) is known as a leaf.
* Height: how far to the furthest leaf. Level: how many connections from any given point on the tree. Depth: inverse of height.
* Degenerate Tree: each tree only has one child. (A linked list is a type of this - each node has a value and one child)
* Binary Tree: a tree where each node has a maximum of two children.
  *Binary Search Tree: a binary tree where everything to the left of a node is smaller and everything to the right is equal to or larger than the node (you can also do the reverse, as long as it is consistent, but this is the most common way to do it). To find the minimum value, just keep going left until you have nowhere else to go. Runtime of the search algorithm is O(log n). BST is an abstract data type.
    * Operations: Insert, Find, Delete (very complicated)
    * Implementing in JS: Linked Tree Data Structure - each node has a reference to a left child and a right child, which by default are null. (Can also be done with an array - at every index is a root, root index * 2 + 1 is left child, and next is a right child)
    * Tree Traversal:
      * Breadth First - By Level. The easiest way to do this is with a queue.
      * Depth First - By Branch. Three types:
        * In-order - Process left nodes, then current nodes, then right nodes. This gives us the numbers in order (from smallest to largest). Runtime on this is O(n) because we go through every single element. Generally, this is the most useful strategy. However, you don't want to create a new tree using this method - it will just give you a linked list.
        * Pre-order - Process root, then left nodes, then right nodes. This is what you would use to copy a tree - you can easily build the exact same tree from this method. (Breadth first search can also recreate the same tree).
        * Post-order - Process left, then right, then root. This has its own benefits at lower levels - it's great for deletion (not necessary in JS, which does garbage collection for you.

#### Graphs
* Trees are actually a type of graph.
* Two types of graphs: 
  * Undirected - by default, there's a mutual connection between the two nodes (eg - Facebook - for someone to be your friend, you have to be their friend too)
  * Directed (we're not talking about this right now)
* The point of a graph is to be able to draw connections between nodes.
* Graphs are very hard to optimize.