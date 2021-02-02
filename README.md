# Python-Interview-Tricks

This is a thorough list of all of the useful Python data structures and tricks to know for interviews. I will add to this over time as I find more useful features.

For Python >= 3.6

### Arrays
#### Common Operations
Most of us are familiar with arrays, how to define them, and the common operations we use them for 
```python
arr = [1, 2]
arr.append(1)
arr.pop()
```
Append is an O(1) operation, pop is an O(1) operation, but only for the last index. Poppping anywhere else is an O(n) since the list has to be shifted accordingly. 

#### Defining Arrays
What about if we want to quickly define an array of some size n? Python makes this very easy and you can often use this as starter code for most dynamic programming problems when setting up an array
```python
n = 5
arr = [0] * n 
#arr = [0, 0, 0, 0, 0]
```

To quickly define a 2D matrix in the List[List[int]] format that most coding questions need, you can use a similar approach

```python
rows, cols = 5, 5
#Note _ in the for loop just means we don't care about the index and are not going to use it so we don't name it
arr = [[0] * cols for _ in range(rows)]
"""
arr = [[0,0,0,0,0],
       [0,0,0,0,0],
       [0,0,0,0,0],
       [0,0,0,0,0],
       [0,0,0,0,0]]
"""
```

#### Slicing
Python lets us easily slice the array via a colon
```python
arr = [1, 2, 3, 4]
#follow convention of [start:end] where end is exclusive. If both are empty, entire array is returned
arr[:] # [1, 2, 3, 4]
arr[:2] # [1, 2] (from index 0 to 1)
#negative numbers refer to indices from the end - these are useful if you don't know the size of the array
arr[:-1] #[1, 2, 3] (index from start up to and not including the last element)
```

If you want to control the window with which you slice, i.e. pick from every kth index, rather than choosing indices, we specify this via another colon
```python
arr = [1, 2, 3, 4]
#follow convention of [start:end:sliceLength] where end is exclusive. 
arr[::] == arr[::1] #(both are equivalent and return the entire array)
arr[::2] # [1, 3] (means start from index 0, and skip an element until the end, so we choose index 0, then index 2 and then it stops since index 4 is not in arr

#negative numbers allow us to slice from the back of the list to the front
arr[::-1] #[4, 3, 2, 1]
arr[::-2] #[4, 2]
```


### Sorting Arrays and Lambda Functions
If we want to sort an array, we can simple call .sort() on the array, which performs mergesort. This takes O(nlogn) time and O(n) space
```python
arr = [1, 5, 3, 2]
arr.sort() #arr is now [1, 2, 3, 5]
```
What about sorting more complicated arrays, like arrays of tuples (often found in [interval problems](https://leetcode.com/problems/merge-intervals/)? We can use lambda functions!

A lambda function is an annonymous inner function which we can pass in to they key parameter of sort. They key parameter is how we let the sort method know what to sort by. In this case, if we called sort directly, it would raise an exception since we can't directly sort tuples. But since each element in array is a tuple, we can pass in a lambda function to tell the sort method what numerical value to choose to sort by.

```python
arr = [(1, 2), (3, 4), (2, 5), (3, 6)]
#x when called on the array above is going to be each tuple. The lambda function takes in a tuple and return a value (first index of the tuple)
arr.sort(key = lambda x: x[0])
arr #[(1, 2), (2, 5), (3, 6), (3, 4)] Note for values that are the same, it will not necessarily make sure they are sorted by their last value
```

This allows us to sort in more complex ways very quickly. For example, given a set of coordinates as tuples, we could sort them by their euclidean distance (ignoring the square root for readability here) as such
```python
arr = [(1, 2), (3, 4), (2, 5), (3, 6)]
arr.sort(key = lambda x: x[0]**2 + x[1]**2)
arr #[(1, 2), (3, 4), (2, 5), (3, 6)]
```

If you want to sort from largest to smallest, set the reverse argument to True.
```python
arr = [1, 2, 3, 4]
arr.sort(reverse = True)
arr # [4, 3, 2, 1]
```

Note that the sort function will **change the original array.** It does not create a copy and return a new sorted array, it performs the operation on the reference to the array.






