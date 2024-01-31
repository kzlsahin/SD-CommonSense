# Bug Free Codding

## Principles
### Writing Functions/Methods 
First of all, any function or method (will be used interchangeably) shall be written in such a way that, any developer shall understand the characteristics of the function at first glance. These characteristics are:
- **Exceptions**: Shall the function be handled for thrown exception (and what kind of),
- **Input validation**: Input types and specific rules for validation
- **Return type**: Shall the function be expected to return null
- **Dependency**: Does the function depends on any variable or constant outside (a class property or global object)
- **Side effects**: Does the function do any thing outside of its scope

Here are the principles I witnessed as quite important for writing bug free meyhods:

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
  
- **Prevent any global dependency as far as possible:**
  
  - Explicitly specify and indicate any global-like dependency.
  - If function uses any global property, receive that at the begining of the function
  - If function set any global-like property as a product of inner process, do that at the end of the function
  - Check global dependecies in terms of concurrency
    
- **Keep function as simple as possibe.**
  
- **handle inputs carefully:**
  
  - Use early returns to validate the initial state
  - Check function arguments for null values
  - check for empty collections IF there is any colletion type argument
    
- **Use type allias for complicated nested types, such as `Action<Action<UseState>, Object[]>`:**
  
  - this will prevent ( [Shotgun Surgery](https://refactoring.guru/smells/shotgun-surgery) ) challanges for debugging and refactoring the codabase in future
    
- **Return empty collection instead of null value**
  
- **Keep documentation up-to-date:**
  
  - Breifly describe what the function is doing
  - Specify any input related validation rule
  - Briefly indicate any global dependecny
  - List all the possible exceptions that can be thrown
  - specify possiblity of returning `Null`
    
- **Double check any function as if it is written on a paper:**
  
  - take a look for any possible exception.
  - take a look for any possible null values.
  - use in-functon alliases for repeated dot-notation chains.

### Logging

(in progress)
[wiki page about bug free programming](https://en.wikibooks.org/wiki/Bug_Free_Programming)
