## Sorting

We have a built-in sort - so why are we learning sorting? We want to understand the underlying algorithms behind sorting.

#### Heuristic 
* An educated guess
* A heuristic is often not perfect, but good enough
* This is often faster than writing an algorithm
* Often you have to decide if a heuristic is good enough for a situation or if you need to spend the time with an algorithm
* Most famous example: traveling salesman problem - using some logic (like throwing in some geography knowledge when you're making decisions) greatly reduces the time it takes to reach an answer

#### Algorithm
* Step by step instructions (deterministic)
* Complete (gets you an answer)
* Finite (...given enough time)
* Efficient (doesn't waste time getting you the correct answer)
* Correct (the answer isn't just close, it's true)
* Downside: some problems are very hard/slow

#### Big O Notation
How do we determine the performance of an algorithm? Big O.
* A comparitive way to classify algorithms.
* A measure of overall performance in relation to input size.
* Big O is an upper bound on the time - it's the worst case scenario. (It might be better)
* Includes just the highest order term.
* __Big Ω (Omega)__ - best possible runtime - often we don't care about this.
* __Big ϴ (Theta)__ - our range/average case - this is where we're likely to find our algorithm.
* Big O is a simplification, but when there are multiple ways to write an algorithm, this helps you decide which is the best approach. At larger inputs, a difference in Big O can mean a big difference in performance. (If two algorithms have the same Big O, we really don't know much - because it's theoretical)

*Most Common Big O*
* O(1) - eg, looking up a single element in an array - constant time
* O(log n) - eg, binary search tree - as the input grows, the increase in the amount of time it takes decreases (the amount of time still increases, but not as much)
* O(n) - eg, looping through an array - as the input grows, the time increases linearly
* O(n log n) - best known runtime for a sorting algorithm (eg, merge sort)
* O(n^2) - eg, selection sort, bubble sort - nested loops
* O(2^n)
* O(n!)

### Sorting
.sort() often implements Quick Sort - it randomly picks a pivot position and sorts around that position. However, for smaller arrays, it uses selection sort (On^2) because the setup for quick sort is larger, so you'll often see worse runtime on smaller arrays.

##### Bubble Sort
- Compares two elements at a time, swaps if necessary, then compares the next two
- eg - [6,5,7,1] - will compare 6 & 5 and swap to get [5,6,7,1], then compare 6 & 7, then compares 7 & 1 and swaps to get [5,6,1,7]. Next pass it comapres 5&6, then 6 & 1 and swaps to get [5,1,6,7], and because we already know that the last element is in place, it can go back to the beginning and compare 5 & 1
- If you go through the entire array and there's nothing to swap, it breaks because it's already sorted
- O(n^2) at worst case, because we're comparing every element against every other element (best case is O(n))
- Space Complexity: O(1) - sorts in place

##### Merge Sort
- Breaks the array into smaller arrays until each array is 1 element, then compares the first element of one array to the next array and puts them back together
- eg, [6,5,7,1] => [6,5]&[7,1] => [6]&[5]&[7]&[1] => compare first two to get [5,6] and last two to get [1,7], then compare those two to get [1,5,6,7]
- Runtime is O(n log n) - we go through all of the elements, because breaking things down into buckets (and putting them back together) is logarithmic.
- This can be done two ways - iteratively and recursively
  - Iteratively:
    1. Divide array of n elements into arrays of 1 element
    2. Merge neighboring arrays in sorted order
    3. Repeat step two until there's only one array
  - Recursively: 
    1. If an array is one element, good job, it's sorted!
    2. Otherwise, split the array and merge sort each half
    3. Merge combined halves into a sorted whole
- Best runtime of this is O(n log n) - no matter how sorted or unsorted this is, we always go through the same process
- Space Complexity: O(n) - for each element in our array, we need another array

##### Stable vs. Unstable
- In ECMAScript, browsers can implement this however they want, but by default, it is unstable
- Stable sorts preserve order of "equal" elements. With unstable sort, there's no guarantee as to the order of equal elements.
- Examples of stable sorts: bubble, merge, insertion, bucket
- Examples of unstable sorts: quick, heap, selection, shell

##### In-Place Sorting
- Only uses a small, constant amount of extra space (O(1) space complexity) to achieve its goal;
- As a consequence, in-place sorting arrays mutate the original array
- Examples of in-place sorts: Bubble, heap, insertion, shell
- Examples of not in-place sorts: Merge (O(n)), Quick (O(n log n | n)), Tim (O(n)), Cube (O(n))

#### What about JavaScript?
- ECMAScript doesn't require sort to be in-place, but it does require it to be mutative.
- V8 default sort is unstable and not in-place.