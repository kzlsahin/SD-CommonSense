# Bug Free Coding

[![en](https://img.shields.io/badge/lang-tr-red.svg)](BUG-FREE-CODING.tr.md)

## Contents
- [General Principles](#general-principles)
- [Writing bug-free Functions/Methods](#writing-bug-free-functionsmethods)
- [Exception Sources](#exception-sources)
- [Sneaky bugs](#sneaky-bugs)
- [Object Oriented Design](#object-oriented-design)
- [Concurrency](#concurrency)
  - [Essential Considerations for Thread Safety and Locking in Concurrent Programming](#essential-considerations-for-thread-safety-and-locking-in-concurrent-programming)
  - [Principles](#principles)
- [Logging](#logging)
- [Examples](#examples)

## General principles

- Sharpen your axe before chopping tree.

- Be patiant and write with ease
  - write the code consciously, do not rush
  - consider procedure results
  - use an effective strategy for exception management. For example, Use result objects and get logs
    
- Learn refactoring techniques and Design Patterns ([Refactoring Guru](https://refactoring.guru/refactoring)).
  
- Avoid magic numbers and strings ([example](#1-avoid-megic-literals))
  - Magic numbers and strings don't mean anything to the developer
  - `enum` should probably have been used if there is magic numbers among the codes,
  - `enum` should have been used if strings are used as human-readable case literals
  - Use constant value if the same string is used in multiple places, as each string occupies memory.
  - Don't use strings as state variables, this is likely to lead to typos and [Shotgun Surgery](https://refactoring.guru/smells/shotgun-surgery) in future refactorings.
  - Use constants and enums to give the developer the benefit of intellisense.

 - Internalize "fail fast" principle and use "Guard Clause" blocks
    
## Writing bug-free Functions/Methods 
First of all, any function or method (will be used interchangeably) shall be written in such a way that, any developer shall understand the characteristics of the function at first glance. These characteristics are:
- **Exceptions**: Shall the function be handled for thrown exception (and what kind of),
- **Input validation**: Input types and specific rules for validation
- **Return type**: Shall the function be expected to return null
- **Dependency**: Does the function depends on any variable or constant outside (a class property or global object)
- **Side effects**: Does the function do any thing outside of its scope

Here are the principles I witnessed as quite important for writing bug free methods:

- **Outline any function before writing it:**

  - Outline generic workflow
  - Determine range of possible inputs
  - Determine range of possible outputs
  - Determine possible side effects

- **Determine characteristics of the function:**

- **Design tests for the function as far as needed:**
  
  - Use specified rules as a checklist for test coverage
  - Test null inputs
  - Test exceptions
  - Test every possible state of the global dependencies
  
- **Avoid any global dependency as far as possible:**
  
  - Explicitly specify and indicate any global-like dependency ([example](#2-document-side-effects)).
  - If function uses any global property, receive that at the begining of the function
  - If function sets any global-like property as a product of inner process, do that at the end of the function
  - Check global dependecies in terms of concurrency
    
- **Keep function as simple as possibe.**
  
- **handle inputs carefully:**
  
  - Use early returns to validate the initial state
  - Check function arguments for null values
  - check for empty collections If there is any collection type argument
    
- **Use type allias for complicated nested types, such as `Action<Action<UseState>, Object[]>`:**
  
  - this will prevent ( [Shotgun Surgery](https://refactoring.guru/smells/shotgun-surgery) ) challanges for debugging and refactoring the codabase in future

- **use local variables as alliases for repeated dot-notation chains.**

  - this will improve the performance and readability of the code.
 
- **Return empty collection instead of null value**
  
  - by this way, checking only the length of the collection will be enough for the caller.
  - this will prevent overlooked null exceptions.

- **Briefly document the function and keep it up-to-date:**
  
  - Briefly describe what the function is doing
  - Specify any input related validation rule
  - Briefly indicate any global dependency
  - List all the possible exceptions that can be thrown
  - Indicate possibility of `Null` return.
    
- **Double check any function as if it is written on a paper:**
  
  - take a look for any possible exception.
  - take a look for any possible null values.
  - Take a look for any possible empty collection.

## Exception Sources

- **Division by zero**

  While reviewing a code block, keep an eye on division operators. Ensure that the divisor is checked for not having zero value, or this exception is handled.

- **Null arguments**

  Arguments or complex variables may have null values. Ensure that any reference type values are checked for being null or this exception is handled properly.

- **Out of range index**

  Always review index flow in loops, ensure that;
    - indices starts from correct value
    - indices are incremented properly
    - complex logic around index increment is avoided.
 
## Sneaky bugs

Bugs that don't throws an exception but causes undesired behavior. These bugs are hard to identify.

- **Default Values**

  The value type variables have default values. It is possible to use unset variables without even recognizing it, especially when working on deeply nested algorithms.

- **unlocked data structure during concurrency**
  -  modifying a collection from multiple threads concurrently without proper synchronization mechanisms in place can result in exceptions or unexpected behavior due to race conditions.
    
## Object Oriented Design

(in progress)

## Concurrency

[History](https://joeduffyblog.com/2016/11/30/15-years-of-concurrency/) of Concurrency from the designer of concurrency in the CLR team of Microsoft.

### Essential Considerations for Thread Safety and Locking in Concurrent Programming
- **Data Integrity:**
  > Data integrity refers to the accuracy, consistency, and reliability of data stored in a system. In concurrent programming, ensuring data integrity involves preventing corruption or inconsistencies in shared data structures when accessed by multiple threads concurrently.

- **Race Conditions:**
  > Race conditions occur in concurrent programs when the outcome of operations depends on the non-deterministic interleaving or timing of threads. They typically occur when multiple threads access shared resources without proper synchronization, leading to unexpected behavior or incorrect results.

- **Atomicity:**
  > Atomicity refers to the property of an operation that ensures it is performed as a single, indivisible unit. In concurrent programming, atomicity prevents interleaving of operations from multiple threads, ensuring that operations are either fully completed or not executed at all.

- **Memory Visibility:**
  > Memory visibility refers to the guarantees provided by the programming language or runtime environment regarding when changes made by one thread to shared data become visible to other threads. Synchronization mechanisms like locks enforce memory visibility to ensure that changes are propagated to other threads in a timely manner.

- **Deadlocks and Livelocks:**
  > Deadlocks occur in concurrent programs when two or more threads are blocked indefinitely, waiting for resources held by each other. Livelocks occur when threads continuously change their states without making progress, often due to incorrect or ineffective locking strategies.

- **Performance:**
  > Performance in concurrent programming refers to the efficiency and scalability of multi-threaded applications. While locking mechanisms introduce overhead due to synchronization and context switching, they are necessary for ensuring correctness. Optimized locking strategies and thread-safe data structures help minimize contention and maximize performance in concurrent applications.

- **Concurrency Control:**
  > Concurrency control involves managing access to shared resources in multi-threaded applications to prevent race conditions, ensure data integrity, and maintain correctness. It includes designing effective locking strategies, using thread-safe data structures, and coordinating access to shared resources among multiple threads.

### Principles

- **Prefer not to use concurrency if not neccessary, it makes debugging harder.**
  - Rather asynchronous programming than multithread programming.
- **Use thread-safe collections if the collection is supposed to be modfied via "add" and "remove" methods.**
  - Reading operations are thread-safe operations. 
  - Keep in mind reference tpyes and value types. Eventhough thread-safe collections are used, objects may have mutable fields.

- **Use functional programming paradigm for the methods consumed by asynchronous processes**
  - Methods shall never call any external data besides the ones provided as arguments. This makes managing synchronization between async calls easier.
  - Keep methods as simple as possible.
  - Avoid side effects inside methods.
 
- **Modification of objects requires `sync lock` where reading does not**
  - orginize modification calls together and keep them smaller as much as possible
  
## Logging

(in progress)

## Examples

### 1. Avoid Megic Literals


1.1. Avoid Magic Numbers
```C#
// Bad
int result = someFunction(5);

// Good
const int DefaultRetryAttempts = 5;
int result = someFunction(DefaultRetryAttempts);
```
1.2. Avoid Magic Strings:
```C#
// Bad
void processState(string state) {
    if (state == "start") {
        // ...
    } else if (state == "finish") {
        // ...
    }
}

// Good
enum ProcessState { Start, Finish };

void ProcessState(ProcessState state) {
    if (state == ProcessState.Start) {
        // ...
    } else if (state == ProcessState.Finish) {
        // ...
    }
}
```
1.3. Avoid Strings as State Variables
```C#
// Bad
string currentState = "processing";

// Good
enum ProcessState { Processing, Completed };
ProcessState currentState = ProcessState.Processing;
```
return [General Principles](#general-principles)

### 2. Document Side Effects

   ![Documentation of a scientific method in .Net](https://github.com/kzlsahin/SD-CommonSense/blob/main/Assets/MethodDocumentation_1.png)


return [Writing Bug-free Functions/Methods](#writing-bug-free-functionsmethods)
   
[wiki page about bug free programming](https://en.wikibooks.org/wiki/Bug_Free_Programming)
