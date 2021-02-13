# Python-Interview-Tricks
This is a thorough list of all of the useful Python data structures and tricks to know for interviews. I will add to this over time as I find more useful features. 

For Python >= 3.6


-[Arrays](#arrays)
-[Sorting Arrays and Lambda Functions](#sorting-arrays-and-lambda-functions)
-[Strings](#strings)
-[Dictionaries](#dictionaries)
-[Priority Queues and Heaps](#priority-queues-and-heaps)
-[Advanced Data Structures](#advanced-data-structures)

### Arrays
#### Common Operations
Most of us are familiar with arrays, how to define them, and the common operations we use them for 
```python
arr = [1, 2]
arr.append(1) #[1, 2, 1]
arr.pop() #[1, 2]
#can also concatenate with a +. If this is only one element it's an O(1) operation, otherwise, it becomes an O(n) operation
arr = arr + [5] #[1, 2, 5]
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

#### Operations
Other useful operations are min and max, if you pass in an array these will run in O(n) time
```python
arr = [1, 2, 3, 4]
min(arr) #1
max(arr) #4
```

A common edge case is when you check if an array is empty, you can do this very easily with
```python
arr = []
if not arr:
  print("It's empty")
```

Using the in operation to check if an element is an array is O(n) each time since it performs linear search. It's often more useful to convert
the array into a set (more on this below), and then you can access elements in O(1).


### Sorting Arrays and Lambda Functions
If we want to sort an array, we can simple call .sort() on the array, which performs mergesort. This takes O(nlogn) time and O(n) space
```python
arr = [1, 5, 3, 2]
arr.sort() #arr is now [1, 2, 3, 5]
```
What about sorting more complicated arrays, like arrays of tuples (often found in [interval problems](https://leetcode.com/problems/merge-intervals/))? We can use lambda functions!

A lambda function is an annonymous inner function which we can pass in to the key parameter of sort. The key parameter is how we let the sort method know what to sort by. In this case, if we called sort directly, it would raise an exception since we can't directly sort tuples. But since each element in array is a tuple, we can pass in a lambda function to tell the sort method what numerical value to choose to sort by.

```python
arr = [(1, 2), (3, 4), (2, 5), (3, 6)]
#x when called on the array above is going to be each tuple. The lambda function takes in a tuple and return a value (first index of the tuple)
arr.sort(key = lambda x: x[0])
arr #[(1, 2), (2, 5), (3, 6), (3, 4)] Note for values that are the same in different tuples, it will not necessarily make sure they are sorted by their last value
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

Note that the sort function will **change the original array.** It does NOT create a copy and return a new sorted array, it performs the operation directly on the array. Keep this in mind if you can't modify or corrupt the original array.


### Strings
String in Python are immutable. Calling an operation on a string will return a new copy. Here are some useful tricks to know
```python
s = "hi"
#split, takes in a string and converts to an array based on a delimiter. If you pass in nothing, it will split every character including spaces/special characters
arr = s.split() ["h","i"]

#concatenation - this is slow, there's a better way with join!
s = "ab"
s += c #abc

#join, takes in an array and returns a string by joining all of the characters in the array with a common character
s = ["a", "b", "c", "d"]
#specify the character to join elements in the array with in the quotations.
a = "".join(s) # "abcd"

#NOTE - string concatenation in Python is an O(n) operation! If you're building a string, it's better to store 
#the characters in an array, append each new character to the array - which is O(1) - and then use join once at 
#the end which is O(n)

#in operation, checks whether a substring is in the string
name = "John"
"hn" in name #True
"nh" in name #False - has to be a substring, not just contain the characters out of orders
```

One very useful library in Python to quickly format strings is the Counter library in collections. If you initialize a Counter with a string, it will 
return a dictionary with keys as the unique characters it finds and values as the frequency of the character. This is very useful in questions where 
you have to parse strings before you can accomplish tasks like in [Task Scheduler](https://leetcode.com/problems/task-scheduler/).

```python
import collections.Counter
string = "AAAABBBCCD"
stringMap = Counter(string) # {"A":4, "B":3, "C":2, "D":1}
stringMap.values() #[4, 3, 2, 1]
```
The most common types of questions you'll find involving strings will be on sliding windows, palindromes, and anagrams. A quick and dirty optimization for some of these questions is to notice whether **there are a limited number of characters as input**. If for example, all characters are lowecase, you can easily get O(1) space by having a size 26 array (for every character) rather than storing elements in a hashmap, which we're moving onto next.

### Dictionaries
Dictionaries in Python serve the purpose of hashmaps. They come in two forms: dictionaries and sets. Dictionaries store key-vale pair 
s, sets are a collection of unordered elements (where you don't necessarily have keys or need keys).

Defining a dictionary in Python is straightforward and most of us know how to do it. But there are some fancy tricks. The first is using a defaultdict. A defaultdict is a Python dictionary which handles keys that have not been already defined.
```python
import collections.defaultdict

ordinaryDict = {}
#we pass in the type that will be stored in values. Let's consider a map of characters to integers, we store ints as the value so we pass in int
coolDict = collections.defaultDict(int)

#this raises an exception since the key "a" is not defined in the dictionary
ordinaryDict["a"] += 1 

#this works like a charm!
ordinaryDict["a"] += 1
```

Defaultdicts are incredibly useful when you need to keep track of frequency of elements or create a map but you don't want to have to keep checking if a key
is already in the dictionary, and adding it if it's not. In particular they're useful for 
1. creating adjacency lists
you can specify a defaultdict to take a list as a value which is very handy. Here's a snippet from [Course Schedule](https://leetcode.com/problems/course-schedule/) to quickly create an adjacency list
```python
"""
:type numCourses: int
:type prerequisites: List[List[int]]
:rtype: bool
"""
adjList = collections.defaultdict(list)
for (course, preq) in prerequisites:
  adjList[course].append(preq)
        
```
Notice how I don't have to check if a course in the adjancey list, I can just append it because defaultdict handles values that haven't been defined

2. sliding window problems where you keep track of character:frequency
Imagine you're trying to count the number of characters that belong to certain ones
```python
string = "some string"
toCount = ["a", "b", "c"]
mapChars = collections.defaultdict(int)
for char in string:
  if char in toCount:
    mapChars[char] += 1
```

Notice that toCount is an array which means when we call in, it will take O(n) where n = length of toCount. We can do better by using a HashSet. A set is
the same as a dictionary, but it only stores elements, not mappings.
```python
string = "some string"
toCount = set()
toCount.add("a")
toCount.add("b")
toCount.add("c")
#now when we call the in operation, it will take O(1) time!
#we can also directly convert an array into a set by passing it into the set, which takes O(n) time
arr = ["a", "b", "c"]
arr = set(arr)
```

Sets are useful when we need to store states but don't have/care about mappings. This is often helpful with situations like storing visited states in a depth first or breadth first search. Note also that you can store tuples, you're not limited to single elements. 
```python
visited = set()
for row in range(rows):
       for col in range(cols):
              visited.add((row, col))
```
Anything that is immutable can be stored in sets and and can be used as a key in an dictionary. If you want to store an array as a key in a dictionary, you'll want to first convert it into a tuple. This is a technique used in [Group Anagrams](https://leetcode.com/problems/group-anagrams/)

```python
cache = collections.defaultdict(list)
for word in strs:
  arr = [0] * 26
  for char in word:
    #since characters are all lowercase, we can associate characters with indices in the array by the difference in their unicodes
    arr[ord(char) - ord("a")] += 1
 #convert arr into a tuple before accessing it as the key. Since we're using a defaultdict, we can append directly and succintly!
cache[tuple(arr)].append(word)

return cache.values()
```

### Priority Queues and Heaps
We can use heaps very easily in Python by importing the heapq library. As a quick reminder, heaps are complete binary trees that maintain some relative ordering between elements such that we always have access to the smallest element (min heap at index 0) or largest element (max heap at index 0) in O(1). Their add/pop operations are all O(logn). Recall the [heapify operation is O(n)](https://www.geeksforgeeks.org/building-heap-from-array/) for n unordered elements (i.e. we don't have to loop through n elements and add each one which would be O(nlogn)).


```python
import heapq
minHeap = []
#to turn our array into a minHeap, we need to call the heapify operation
#note you can call heapify on an array already filled with elements as described above
heapq.heapify(minHeap) #pass in minHeap as the parameter

#add an element
heapq.heappush(minHeap, 5)
heapq.heappush(minHeap, 8)
heapq.heappush(minHeap, 2)

#access to min element
minHeap[0] # would give us 2

#to remove the minimum element
heapq.heappop() #now our minHeap would look like [5, 8]

#to replace the minimum element with some other value
heapq.heapreplace(minHeap, 6) #now our minHeap would look like [6, 8]
```

Python does not directly have a max heap data structure we can use. The easiest way to use one is to use the min heap operation above but:
1. multiply (i.e. encode) all your values by -1 before pushing them
2. multiply (i.e. decode) all your valyes by -1 when you access them for a comparison

The reason this works is that larger values when multiplier by -1 will be smaller and "pushed to the top." Imagine a tree with -1 and -5 in a min heap. -5 is smaller so would be stored at the root, but (as long as we remember to encode and decode it) this means it has the properties of a max heap! The only thing is **do not forget to encode/decode (i.e. * by -1) all your values before you push/pop.**



```python
import heapq
maxHeap = []
heapq.heapify(maxHeap)

#to avoid confusion, I'd suggest defining custom add/pop operations 
def push(maxHeap, element):
  heapq.heappush(maxHeap, element * -1)

def peek(maxHeap):
  return maxHeap[0] * -1

def pop(maxHeap):
  return heapq.heappop(maxHeap) * -1

def replace(maxHeap, element):
  return heapq.heapreplace(maxHeap, element * -1)

#add an element
push(minHeap, 5)
push(minHeap, 8)
push(minHeap, 2)

#access to max element
peek(maxHeap) #8

#to remove the maximum element
pop(maxHeap) #8

#to replace the maximum element with some other value
replace(maxHeap, 3) 
```
Note you don't have to define custom functions but it can help avoid mistakes where you forget to multiply by -1 when pushing or popping.

What about if you need to store more information in the heaps with each value. You can use tuples! The important thing to remember is that **the first element of a tuple is what Python will order the heap by**. You'll often need this for some more advanced scheduling questions like [Car Pooling](https://leetcode.com/problems/car-pooling/)


```python
import heapq
minHeap = []
heapq.heapify(minHeap)

#add an element
heapq.heappush(minHeap, (5, "code1"))
heapq.heappush(minHeap, (8, "code2"))
heapq.heappush(minHeap, (3, "code3"))

#access to min element
minHeap[0] # would give us (3, "code3")

#to remove the minimum element
heapq.heappop() #now our minHeap would look like [(5, "code1"), (8, "code2)]

#you can use the rest of the functions in a similar fashion
```


### Advanced Data Structures
