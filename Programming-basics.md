# Programming Basics introduction
The goal of this section is to give artist **fast understanding** of core programming concepts to be able to follow [VEX tutorials](VEX-Quick-start) if the programming is Terra Incognita for him (or her). To explain complex things (and programming could be sophisticated for those who never have a deal with it or who consider himself an artist but not technician) we will use simplification and analogies. This is not even a first steps, **we learn to crawl here**.

**Simplification** is a focus on the main topic ignoring all surrounding exceptions and details. **Analogies** — explaining unknown with the notions which are intuitive (familiar without any extra knowledge to a majority of humans in the world). Both simplification and analogies yields to a faster understanding but less precision. Keep in mind that you may not pass an exam on computer science with such kind of knowledge but from the other side it would be possible to start applied developing without finishing computer science class.

At the end of the lesson, you should be comfortable with [syntax](#syntax), [data types](#data-types), [variables](#variables), [commands](#commands), [functions](#functions), [loops](#loops).  

# Programming theory
## Programming and programs
Let's define what is programming. Sure we can say that programming is a process of creating computer programs, it would be correct, but not very useful. Let's say, **programming** — is communication with a computer, it`s how you can "speak" with this device. The more accurate definition would be: **giving precise instructions to a computer** to perform specific tasks. Moreover, anything you want your computer to execute should be expressed in a way that your computer will understand. And the only thing **computers understand** is a **program** written in particular programming language. 

You can imagine programs as blocks. The program could be simple (like a lonely small block of code), or it could be more sophisticated and combined with numerous small blocks linked together. How we link blocks we will describe later in [data types](#data-types) section. 

One important aspect of programming which worth to learn at the early stages is **comments**. Comment is a note in program code for humans, for those people who could read the code later, including a program author. One of the basement law of programming speaks: 

**Code is read much more often than it is written**.

So you can expect that you write a comment for yourself and its a **must follow practice**. Always comment your code precisely!

_A particular example: when you copy a file from one folder to another your OS execute specific program._

## Syntax
As you can speak different languages, computers understand different programming languages also. The written rules for each language (the alphabet, words and sentence structure, etc) is what we can call a **syntax**. Each programming language has its own syntax. Once you learn one of them it's more and more easy to learn another. 

_A particular examples: you should end every sentence in VEX with a semicolon. Comments line should start with `//`_

## Data types
The concept of data type is much more easy to understand than to explain. Let's define what is data first. **Data** is any piece of information you are dealing with inside your program (code). It's important to understand that data is not a part of the code (program) itself.  It is something that "comes inside" your program from the outer environment (everything around your program block). 

You can imagine data pieces as blocks as well, but this blocks, unlike programs, do not do anything, just holds information.

Now we can clarify links between different programs we speak above in [Programming](#programming-and-programs) section. In addition to program input (when something come inside the program), the program can produce something (data) and give it away (output data). And usually, programs modify input data and output (return) the results of the modifications. This data flow between blocks of programs can be imagined as lines between blocks and this is how we link blocks (programs) together. 

For example, if we have a program which copies files from `folder A` to a `folder B` (when user selects a file in explorer and press Ctrl + C in folder A and Ctrl + V in folder B) the **input data** here is a **name with a path of the copied file** (C:/temp/myFile.py). Giving a program file path as input we let it know what file it needs to copy, so the program can do the job. Another data examples: list of students in a class, the current time in Michigan, number of wheels in a car, etc.

Obviously, if data could be any piece of information that comes into a program we need to **sort out** (structure) somehow all varieties of this information, otherwise, we would not be able to deal with it efficiently. The number of options in which form the information could be presented is limited. A number of wheels in a car is a digit. List of students is a stack of characters (students names). Time is also a digit. And a path to a file is also a stack of characters.

**Data type** — is exact form in which any piece of information in a computer program can be represented.

We will examine most used in VEX data types:  

**Integer** — a whole numbers: `256`  
**Float** — a fractional number: `3.14159`  
**Vector** — a set of 3 floats:  `{1.0, 0.0, -12.56}`  
**String** — a text, set of characters: `'Hello, World!'`  
**List** — a set of values with the same data type: `['John Daw', 'Paul Anderson', 'Sarah Connor']`

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

Commands also can have **inputs** and **outputs** of data as we discover with programs. The input for the command is called an **argument**:

```c
thisIsCommand(thisIsArgument);
```

 
## Functions
What if there is no appropriate command to solve your task in a given programming language? Write and use your own!
Let`s define a **function** as a **custom command to perform a specific task** inside your programm.

Now we can assume that **command** is a **built-in function** in a certain language. 

We define functions in VEX with data type that function will output (return) and function name:

```C
// Define Hello World message function
string printHello(){
    printf ('Hello, World!');
    }
// Run Hello World functiion
printHello();
```

## Loops

# Going further: algorithms, abstraction, dictionary 

