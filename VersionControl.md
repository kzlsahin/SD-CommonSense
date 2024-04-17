# Using Version Control

- Always use a version control system.


## Using GitHub

### Issues to Care

- **Inserting a new code block**
  
  inserting a new code block may cause unpredicted conflicts when the new block ends with same lines of code.

  ```C#
    public doSomething()
    {
      // some codes here...
  // ==== Git Changes starts here
      // ** ending codes ..
    }

    public doSomethingNew()
    {
      // some codes here...
  // ==== Git Changes ends  here
      // ** same ending codes ..
    }
  ```
  
