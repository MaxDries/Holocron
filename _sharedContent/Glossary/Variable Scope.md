# Definition

Variables and constants apply only within the module/function in which they are declared.
# Example/s

**Local Scope:**

- A variable declared within a function, loop, or block is only accessible within that specific scope.
- Once the scope ends, the variable is no longer accessible.

**Example:**

```python
def my_function():
    local_variable = 10

# Outside the function, local_variable is not accessible
print(local_variable)  # Error: NameError: name 'local_variable' is not defined
```

**2. Global Scope:**

- A variable declared outside of any function or block is accessible throughout the entire program.
- Global variables can be accessed from any part of the program.

**Example:**

```python
global_variable = 20

def my_function():
    # Can access global_variable within the function
    print(global_variable)

# Can access global_variable outside the function
print(global_variable)
```