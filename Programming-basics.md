# Introduction
The goal of this section is to give artist **fast understanding** of core programming concepts to be able to follow [VEX ](vex-for-artists) or [Python](Python-for-artists) tutorials if the programming is Terra Incognita for him (or her). To explain complex things (and programming could be sophisticated for those who never have a deal with it or who consider himself an artist but not technician) we will use simplification and analogies. This is not even a first steps, **we learn how to crawl here**.

**Simplification** is a focus on the main topic ignoring all surrounding exceptions and details. **Analogies** — explaining unknown with the notions which are intuitive (familiar without any extra knowledge to a majority of humans in the world). Both simplification and analogies yields to a faster understanding but less precision. Keep in mind that you may not pass an exam on computer science with such kind of knowledge but from the other side it would be possible to start applied developing without finishing a computer science class.

At the end of the lesson, you should be comfortable with [syntax](#syntax), [data types](#data-types), [variables](#variables), [commands](#commands), [functions](#functions), [loops](#loops), [conditions](#conditions).  

## Programming learning curve
Yes, it could be complex from the beginning and the first line of code you will create by yourself without following any tutorial may take hours or days to complete. But it would be dramatically easier later very soon, several one-line-of-code scripts, then first procedure created, then you would not notice as you become a descent coder. Try hard and you will be rewarded!

# Programming theory basics
## Programming and programs
Let's define what is programming. Sure we can say that programming is a process of creating computer programs, it would be correct, but not very useful. Let's say, **programming** — is communication with a computer, it`s how you can "speak" with this device. The more accurate definition would be: **giving precise instructions to a computer** to perform specific tasks. Moreover, anything you want your computer to execute should be expressed in a way that your computer will understand. And the only thing **computers understand** is a **program** written in particular programming language. 

You can imagine programs as blocks. The program could be simple (like a lonely small block of code), or it could be more sophisticated and combined with numerous small blocks linked together. How we link blocks we will describe later in [data types](#data-types) section. 

## Syntax
As you can speak different languages, computers understand different programming languages also. The written rules for each language (the alphabet, words and sentence structure, etc) is what we can call a **syntax**. Each programming language has its own syntax. Once you learn one of them it's more and more easy to learn another. 

_VEX syntaxis examples:_  
- You should end every sentence with a semicolon: `;` 
- Comments line should start with a double slash: `//`

_Python syntaxis examples:_  
- You should separate your sentences with Tab (4 spaces)
- Comments line should start with a hash: `#`

## The good code
One of the basement law of programming speaks: 

**Code is read much more often than it is written**.

So in addition to obvious requirements that code should work, we have some more characteristics to consider. The good code:

- Works and solve the problem
- Readable and looks pleasant
- Does not contain duplicates of the same code


## Data types
The concept of data type is much more easy to understand than to explain. Let's define what is data first. **Data** is any piece of information you are dealing with inside your program (code). It's important to understand that data is not a part of the code (program) itself.  It is something that "comes inside" your program from the outer environment (everything around your program block). 

You can imagine data pieces as blocks as well, but this blocks, unlike programs, do not do anything, just holds information.

Now we can clarify links between different programs we speak above in [Programming](#programming-and-programs) section. In addition to program input (when something come inside the program), the program can produce something (data) and give it away (output data). And usually, programs modify input data and output (return) the results of the modifications. This data flow between blocks of programs can be imagined as lines between blocks and this is how we link blocks (programs) together. 

For example, if we have a program which copies files from `folder A` to a `folder B` (when user selects a file in explorer and press Ctrl + C in folder A and Ctrl + V in folder B) the **input data** here is a **name with a path of the copied file** (C:/temp/myFile.py). Giving a program file path as input we let it know what file it needs to copy, so the program can do the job. Another data examples: list of students in a class, the current time in Michigan, number of wheels in a car, etc.

Obviously, if data could be any piece of information that comes into a program we need to **sort out** (structure) somehow all varieties of this information, otherwise, we would not be able to deal with it efficiently. The number of options in which form the information could be presented is limited. A number of wheels in a car is a digit. List of students is a stack of characters (students names). Time is also a digit. And a path to a file is also a stack of characters.

**Data type** — is exact form in which any piece of information in a computer program can be represented.

_We will examine most used in VEX data types:_ 

**Integer** — a whole numbers: `int numberOfIterations = 256;`  
**Float** — a fractional number: `float PI = 3.14159;`  
**Vector** — a set of 3 floats:  `vector color = {1.0, 0.0, 0.256};`  
**String** — a text, set of characters: `string phrase = 'Hello, World!';`  
**Array** — a set of values with the same data type: `string letters[] = {'A', 'B', 'C'};`

To **process array** data type we use [**loop**](#loops) construction, the loop allows applying some actions to each element of the array.

## Variables
Variable — is a container served to store data. Imagine a variable like a box with coins (or any other volume with any other items of the same type inside).

The current variable can hold only one data type. For example, you can`t store integer and string in one variable. In other words, each data type requires its own variable.

In VEX when you create (declare) a variable you should define the **data type** and **variable name**:
```C
// Set path to the source file
string pathToFile = 'C:/temp/myFile.py';
```
Where "string" is definition of string data type for variable which has name "pathToFile" and `C:/temp/myFile.py` is a value for the variable.

## Commands
A **command** is the **smallest fraction of a program** which performs a specific task. The definition of command may look almost the same as a program definition, but the program is a more general concept. In other words, a program consists of the commands. You can imagine commands as bricks which form the block of the program.

Commands exist within the given programming language, it predefined by the language itself. You write your programs using existing commands. For example, you will use `copyfile()` Python command to copy a file from one location to another if you will write your own **copying program** with a Python language.

So basically coding process happens like this: you break your global program task into small command pieces and search for commands which could solve this pieces within a given programming language.

### Arguments and objects
Commands also can have **inputs** and **outputs** of data as we discover with programs. The input for the command is called an **argument**:

```c
// VEX command with argument example: print "Hello, World!"
printf('Hello, World!');
```

Here `Hello, World` is an argument (or input) of a VEX printf() function.

The **output** of the command it is an entity created by this command called **object**. In the case of the print command its hard to presume which object is being created. But let's imagine that you created a sphere in the scene with a Python command `createNode('sphere')`. You will get a sphere object in the scene, which you can select and do anything you need later. In addition to the object in the scene, you can create an object in the code, which you can select and do anything you need later (in the code). To create an object in the code you just need to assign a command to some variable and this variable would be our object in the code: `shereObject = createNode('sphere')`. So you can, for example, rename it: `shereObject.rename('myShpere')`. In other words you have your object in the scene and at the same time you can have an instance of this object in the code.

## Functions
What if there is no appropriate command to solve your task in a given programming language? Write and use your own!
Let`s define a **function** as a **custom command to perform a specific task** inside you program.

Now we can assume that **command** is a **built-in function** in a certain language. 

We define functions in VEX with **data type** that function will output (return) and **function name**, we place code for function in curly brackets `{ ... }`:

```C
// Define Hello World message function
string printHello(){
    printf('Hello, World!');
    }
// Run Hello World functiion
printHello();
```
Where:
- "string" is data type that function returns,  
- "printHello" is function name,  
- `printf('Hello, World!')` is a code of our function which consist of one command `printf()` recieving "Hello, World!" string as an argument.

## Loops
**Loop** is a concept which allows performing a specific task **several times**. It's **repeating** one or more command, function, program. It is used when you need to solve the same task with a numerous amount of objects. Loops are used widely to process **arrays** (see data types), so you can do something with each element of the array one by one.

For example, if you need to delete a bottom face of several cubes in your scene, you will write a function which would be able to delete bottom face of one cube (and this cube would be passed as input data (argument) into your function) and then repeat this function for every cube with a help of **loop**.

Check one more example — [double intensity of all lights in the scene](https://github.com/kiryha/AnimationDNA/wiki/06-Tutorials#introduction-to-artistic-developing) for Maya and Python. 

Let's see how deleting cube face looks in [pseudocode](#pseudocode):
```c
// Create a container for all cubes in scene (list variable)
listOfCubes = ['Cube_01', 'Cube_02', ... 'Cube_N']

// Create a procedure which will delete the bottom face of input cube
deleteFace(inputCube)
    find the bottom face of inputCube
    delete the bottom face of inputCube

// Apply deletion procedure for all cubes
for each object in listOfCubes run deleteFace()
```
**Loop syntax** in VEX:
```c
// Create array with a list of students 
string listStudents[] = {'John','Dan','Sarah'};

// Print each student name
foreach ( string student;  listStudents){
    printf(student);
    } 
```
Where `string listStudents[] = {'John','Dan','Sarah'}` is a definition of string array holding students names, `foreach(string student;  listStudents)` is a loop definition: for each member `student` of array `listStudents` execute code in `{}`. And `printf()` is a VEX command which prints an argument `student` (student name), so each iteration we print a new name from the given array.

The `printf()` command expects arguments of string data type (you should "place" inside this command only string data: `printf(stringInput)`). So if we would process integer array we would need to convert integer data type to a string data type and it could be done with a construction `printf('%d', integerVariable)`, where`'%d', integerVariable` means: substitute `'%d'` with currently processed integer element of array `integerVariable`.

```c
// Create array with a students grades 
int listGrades[] = {10, 2, 9, 0};

// Print each grade from array
foreach(int grade;  listGrades){
    printf('The student grade is: %d', grade);
    // %s > stringVariable, %f > floatVariable, %d > integerVariable
    } 
```

Note, that you can have access to the index of the item in the loop in addition to item value:
```c
foreach(int index; int grade;  listGrades){
    ...
    } 
```

Another option for loops in VEX:
```c
// Print "Hello, World!" 10 tims
for (int n=0; n<10; n++){
    printf('Hello, World!');  
    }
```
Where `for (int n=0; n<9; n++)` is a loop definition: start from 0 (`n=0`), until we reach 9 (`n<9`), with a step of 1 (`n++`) execute everything located in `{}` — print "Hello, World!".

Going deeper, `n++` is a "syntaxis sugar" (more readble or short form) for `n = n + 1` and a step of 1 means we will run code inside the curly brackets every step from 0 to 9 (what will give us 10 times repetition). We can run a code with a different step value, `n = n + 2` will skip every second iteration, but never mind, most of the time `n++` will work for you.

#### Loop Iterations
So you repeat a block of code several times with a loop. Each time you repeat a code inside the loop you doing one **iteration**. In this code `for (int n=0; n<9; n++)`, the `n` is **iteration number** - integer variable that you have acsess in the loop.. In this code `foreach(int grade;  listGrades)` we also have iterations — when each time executing the code inside the loop, we just don`t have a explit variable for them in this case.

## Conditions
Conditions are used when you need to act differently depending on circumstances. You check if a particular condition is true or false and run different code depending on the answer. Conditions represented as an `if-else` statements almost in any programming language. If there is rain outside I would take an umbrella, otherwise, I will leave the umbrella at home. 

The `else` statement is optional: "if there is a rain outside I would take an umbrella" will work as well. Conditions could be nested and combined to describe more complex algorithm: if there is rain outside and the rain is heavy I would stay at home, but if the rain is not so bad I would take an umbrella and go out.

```c
// Define constant variables
int a = 10;
int b = 10;
// Check if vriables are equal
if (a/b == 1){
    printf ('A egual B');
    }
```

Where `(a/b == 1)` is a checked condition: if A divided by B is an equal of one. Why we use `==` instead of `=`? In a case with conditions, the sign you are checking condition with (sign between compared items, `a/b` and `1`) is called **condition operator**. The equal sign `=` means that you **assign a value** to a variable, to check equality in a condition we have to use `==` operator. 

Common condition operators:
- Does A equal to B: `A == B`
- Does A not equal to B: `A != B`
- Does A grater or equel to B: `A >= B` 

In conjunction with the conditional operators you may use **logical operator** like AND, OR and NOT. If the rain is heavy and I have an umbrella and I don't have a cold — go outside.

Now you are hopefully ready to complete [VEX tutorials](VEX-Quick-start). If you are interested in more fancy coding stuff, go ahead and read the next section.

# Programming theory next steps
This is not a necessary section for starting VEX experiments but it could be useful for curious minds. Here we can go further from the basics and learn about [pseudocode](#pseudocode), [dictionaries](#dictionaries), algorithms and abstraction.

## Pseudocode
Pseudocode — is a form of a computer program for human reading. It's a computer program brief written in usual sentences. Usually its a first stage of program creation, you formalize all tasks as pseudocode and then "convert" this code to a computer program with a desirable computer language.

## Hash table
Hash table is a special [data type](#data-types) which can store information in form `KEY : VALUE`.  
The `KEY` should only be a **string** and the `VALUE` could hold any data type (even dictionary and such nested dictionary able to hold quite complex data structures).  

Dictionary is a very powerful way to store complex data sets, you can imagine them as a shelf with containers (which could store any items — any data types). You can find a container (and get its content) by the name (KEY). 
 
_For example_: `colors = {'RED': [1,0,0], 'GREEN':[0,1,0], 'BLUE':[0,0,1]}`  

This will give you a palette of necessary colors and you can use colors in your code:  
`pointColor = colors('RED') // Returns [1, 0, 0]`

VEX does not support dictionaries directly.

## Abstraction
Abstraction is the opposite of a specific. 

If we speak about the `apple`, could we be more specific? Yes, we could say "Galla" apple. Can we be more abstract? Sure, we could say "fruit" instead of "apple".

# Algorithms
An algorithm is a common language to understand nature.