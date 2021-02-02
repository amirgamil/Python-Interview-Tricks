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
Python lets us easily slice the array in the following

