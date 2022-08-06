# Introduction
A simplified explanation of Computer Science for people who have no clue about how a computer works.

Despite this course is oriented on the Computer Graphic field (and Houdini in particular) it could be useful for anybody else who decided to learn how to code from the beginning.

Everybody can write code at some level even without any relevant knowledge. The majority of questions you might have while you write your first line of code already answered online. So it is possible to produce working code for your daily needs without proper education.

But if you would like to have better control over the computer with a code you will need to understand how it works at the lowest possible level. This knowledge is called Computer Science and here we will make the first step in mastering it.



# How a computer works

# Data structures and algorithms
Data Structure is a collection of values organized in a certain way. The algorithm is a set of instructions that manipulates Data Structure. A combination of Algorithms and Data Structures is a Computer Program.

## Data Structures
Data Structure is a way to store things in computer memory. Those things could be different types, like numbers, text, or images.

The data structure is a collection of values.

#### Arrays
Is a way to store a number of items. 

```c
// Pseudocode of array
integer_array = [0, 1, 2, 3, 3, 4]
string_array =  ["apple", "banana", "strawberry"]
mixed_array =   ["beef", 147.5, [1, 2, 3], True]
```

#### Hash Tables
A Hash table is a way to store the key-value pairs.

```c
// Pseudocode of hash table
hash_table = {"height": 180, "weight": 74, "temperature": 36.6}
```

If in the array we use the indexes of elements to access them (get or set values), in hash tables we utilize keys for this. The key presents an address in memory where the value is located. To be able to use keys as a memory address we need to convert it from text to indexes and this is can be done by hash functions.

The **hash function** generates a value of fixed length for each input. The resulting value, random pattern, will be always the same for the same input. Such operations, when you apply it several times without changing the result, are called **idempotent operations**.

```c
// Pseudocode of hash function
hash_function("Hello, World!") = 0A0A9F2A6772942557AB5355D76AF442F8F65E01
```
It's practically impossible to get the source value from the result.

#### Stacks and Queues
The linear data structures which allow traversing data sequentially, one by one. Only one element can be directly reached. The only difference is how data can be removed. You can access only the first or the last element.

You can think of stacks as plates when each new piece of data is added on top of another, and if you need to retrieve the data you can only access the top one (latest added). It is called LIFO — Last In First Out. Example of usage: undo operations.

Queues can be imagined as a line to the entrance. The first person arrived will enter the first. It is called FIFO — First In First Out. Example of usage: a printer will process the tasks in order of receiving from users. 

## Algorithms
The algorithm is a step by step instruction for solving a problem. The algorithm manipulates Data Structure.

### Time and space complexity: Big O notation
The relationship of input size to the number of steps the algorithm takes to run characterizes the algorithm's time complexity.

### Basic solution patterns
An algorithm is a step by step instruction for solving a problem. Those steps could be organized into the patterns, and basic blocks of a problem solution. Pattern, like a programming function, does a simple and obvious action. For example, traveling through each element of the list is a pattern, which could be implemented in Python with a `for loop`.

Here I will record all patterns recognized while learning algorithms.

#### Manipulating numbers
Say we have an integer 123, how we can manipulate it? For example, to check if it is a palindrome number, we need to compare the first and last digits, and if it was a string we can just take the first and the last items of the string: 

```Python
string = `123`

first_digit = string[0]
last_digit = string[-1]

print first_digit, last_digit

# result: 1, 3
```

Ok, strings are easy, but how can you get the first or last digits from the integer? Sure with math, modulo and division will do the trick:
 
`first_digit = integer/number_of_digits`, `last_digit = integer % 10`

E.g. `1234 % 10 = 4, 1234 % 100 = 34, 1234 % 1000 = 234, 1234 % 10000 = 1234 `

We can get the number of digits in any number by checking how many times you can multiply 1 by 10 before it becomes greater than original number:

```Python
def get_number_length(integer):
    
    length = 1
    while integer > length * 10:
        length *= 10
    
    return length
```

#### Walkthrough each element in the array.
One of the simplest patterns I can imagine. Do something with each element of the array.

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

In the second loop, with the help of the first value of the `range()` function, we skip elements, that we already check in the first loop. But its still O(n^2). Can we do better?