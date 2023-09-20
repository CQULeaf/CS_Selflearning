# Some questions and their analysis

## The special retuen statement of Q: a_plus_abs_b

```python
def a_plus_abs_b(a, b):
    """Return a+abs(b), but without calling abs.

    >>> a_plus_abs_b(2, 3)
    5
    >>> a_plus_abs_b(2, -3)
    5
    >>> # a check that you didn't change the return statement!
    >>> import inspect, re
    >>> re.findall(r'^\s*(return .*)', inspect.getsource(a_plus_abs_b), re.M)
    ['return f(a, b)']
    """
    if b >= 0:
        f = add 
    else:
        f = sub
    return f(a, b)
```

1. `f = add` actually means `f` is set to `add`.
2. The return statement `return f(a, b)` is actually fine if `f` is properly defined. It calls whatever function `f` is pointing to with arguments `a` and `b`.
3. The use of `f` as a function is a more advanced concept called ***'First-Class Functions'***. In Python, functions can be assigned to **variables** and then used just like any other **value**.
    - ***First-Class Functions***: actually means functions can be treated like any other value or variable. This feature allows us to do a lot of flexible and powerful things in our code. It's like saying functions are "first-class citizens" in the world of Python, having the same rights and privileges as any other type of data such as **assigning them to a variable**, **passing them around**, **getting them as output**, **storing them in lists or dictionaries** etc.
        - **Assigned to a Variable**: You can assign a function to a variable and then use that variable to call the function.

            ```python
            def say_hello():
                print("Hello")

            greet = say_hello
            greet()  # Output: Hello
            ```

        - **Passed as an Argument**: You can pass functions as arguments (also called parameters) to other functions.

            ```python
            def greet(func):
                func()
   
            def say_hello():
                print("Hello")
   
            greet(say_hello)  # Output: Hello
            ```

        - **Returned from another Function**: A function can return another function.

            ```python
            def wrapper():
                def inner_function():
                    print("Inside inner function")
                return inner_function

            wrapped_function = wrapper()
            wrapped_function()  # Output: Inside inner function
            ```

        - **Stored in Data Structures**: Functions can be stored in lists, dictionaries, etc.

            ```python
            def add(a, b):
                return a + b

            def sub(a, b):
                return a - b
  
            func_list = [add, sub]
            print(func_list[0](2, 3))  # This sentence means call the first function in `func_list` with arguments 2 and 3 and then print the result (here is 5) to the console.
            ```

## The complex logic of Q: If Function vs Statement

```python
def if_function(condition, true_result, false_result):
    """Return true_result if condition is a true value, and
    false_result otherwise.

    >>> if_function(True, 2, 3)
    2
    >>> if_function(False, 2, 3)
    3
    >>> if_function(3==2, 3+2, 3-2)
    1
    >>> if_function(3>2, 3+2, 3-2)
    5
    """
    if condition:
        return true_result
    else:
        return false_result


def with_if_statement():
    """
    >>> result = with_if_statement()
    47
    >>> print(result)
    None
    """
    if cond():
        return true_func()
    else:
        return false_func()

def with_if_function():
    """
    >>> result = with_if_function()
    42
    47
    >>> print(result)
    None
    """
    return if_function(cond(), true_func(), false_func())

def cond():
    "*** YOUR CODE HERE ***"
    return False


def true_func():
    "*** YOUR CODE HERE ***"
    print(42)


def false_func():
    "*** YOUR CODE HERE ***"
    print(47)
```

1. In Python, all arguments to a function are evaluated before the function is called. That means when you call `if_function(cond(), true_func(), false_func())` in `with_if_function()`, `cond()`, `true_func()`, and `false_func()` are all executed before `if_function()` is called.

    So, even if `cond()` returns `True`, `false_func()` has already been executed as part of the argument evaluation. The reverse is also true: even if `cond()` returns `False`, `true_func()` has already been executed.

    In contrast, `with_if_statement()` uses an `if` statement, which has short-circuiting behavior: it only evaluates `true_func()` if `cond()` returns `True`, and `false_func()` if `cond()` returns `False`.
2. Actually, `print()` is a non-pure function and its side-effect is to print the content in its parentheses and has no return (actually return `None`). So the series of `print(result)` display `None`.
