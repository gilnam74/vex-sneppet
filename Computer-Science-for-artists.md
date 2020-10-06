# Introduction
Despite this course is oriented on the Computer Graphic field (and Houdini in particular) it could be useful for anybody else who decided to learn how to code from the beginning.

Everybody can write code at some level even without any relevant knowledge. The majority of questions you might have while you write your first line of code already answered online. So it is possible to produce working code for your daily needs without proper education.

But if you would like to have better control over the computer with a code you will need to understand how it works at the lowest possible level. This knowledge is called Computer Science and here we will make the first step in mastering it

# How a computer works

# Data structures and algorithms
## Data Structures
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



## Algorithms
