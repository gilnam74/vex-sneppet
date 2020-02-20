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
Let's imagine we have an array of numbers and we need to compare each number with the rest. To find the biggest one, for example.

The first and most robust implementation would be to loop through each element and within each cycle loop through each element again. This would be a nested loop, with time complexity O(n^2). We will use the simple array of two elements to track what's going on easier.

```Python
list = [12, 4]

for element_1 in list:
    for element_2 in list:
        print '{0}:{1}'.format(element_1, element_2)

# Result: 12:12 12:4 4:12 4:4
```

First, we can see that there is a lot of redundancy, elements are compared to themselves (12:12 4:4), and they are compared twice (12:4 4:12). Let's optimize this disorder:

```Python
list = [12, 4]

for element_1 in range(len(list)):
    for element_2 in range(element_1+1, len(list)):
        print '{0}:{1}'.format(list[element_1], list[element_2])

# Result: 12:4
```

In the second loop, with the help of the first value of the `range()` function, we skipping elements, that we already check in the first loop.