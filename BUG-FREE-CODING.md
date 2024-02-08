# Bug Free Coding

[![en](https://img.shields.io/badge/lang-tr-red.svg)](BUG-FREE-CODING.tr.md)

## Principles

### General principles

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

 - Use fail early principle
    
### Writing bug-free Functions/Methods 
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

### Object Oriented Design


### Logging

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