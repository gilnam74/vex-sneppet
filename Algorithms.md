# Unsorted data
## Basic solution patterns
An algorithm is a step by step instruction for solving a problem. Those steps could be organized into the patterns, basic blocks of a problem solution. Pattern, like a programming function, does a simple and obvious action. For example, traveling through each element of the list is a pattern, which could be implemented in Python with a `for loop`.

Here I will record all patterns recognized while learning algorithms.

#### Walkthrough each element in the array.
One of the simplest pattern I can imagine. Do something with each element of the array.

```Python
list = [1, 2, 3, 4]

for element in list:
    print element

# Result: 1, 2, 3, 4
```

#### Compare each element of the array.
Let's imagine we have an array [1, 2, 3, 4] and we need to compare each number with the rest. To find the biggest one, for example.

The first and most robust implementation would be to loop through each element and within each cycle loop through each element again. This would be a nested loop, with time complexity O(n^2).

```Python
list = [1, 2, 3, 4]

for element in list:
    print element

# Result: 1, 2, 3, 4
```