## Sorting: Q&A Video

### Merge Sort
###### Example: 
[3,4,1,2] - We want to break this down until we know each array is sorted  
[3,4][1,2] - Is it sorted yet? Not necessarily  
[3][4][1][2] - Each array is sorted - THIS IS OUR BASE CASE! Now we can merge (and compare adjacent arrays) 
[3,4][1,2] - Each of these was already sorted - not much to do
[1,2,3,4] - Starting at index 0, we compared 3 and 1, and 1 was smaller, so we moved to index 1 on the right and compared 2 and 3. Right index is now out of elements, so we can put in the elements from the left array.  

The "merge" function always takes in sorted arrays.