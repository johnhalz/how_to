# Diagnosing Slow Python Code

> Note: This guide is inspired from the video [Diagnose Slow Python Sode (feat. async/await)](https://www.youtube.com/watch?v=m_a0fN48Alw&t)

Imagine we have a bit of code in python and it's performance is under the required specififcation. We want an effective way to find what in the code is causing the slowdown without guessing or relying on any experience of the coder. A wrong guess can mean spending hours of time improving a bit of code only to find that performance is barely improved.

In our example, say we have a function that we run from the `main()` function:

``` python
import ...

def does_stuff():
    ...

def does_stuff_slow():
    ...

def main():
    does_stuff()

    does_stuff_slow()

if __name__ == '__main__':
    main()
```

We can use `cProfile` and `snakeviz` to show us what is slowing down the code in our project. Both can be install via `pip`.

## Using `cProfile`
Using `cProfile` for debugging slow code is easy. All we need to do is add the following lines to our main function:

``` python
import ...

def does_stuff():
    ...

def does_stuff_slow():
    ...

def main():
    import cProfile
    import pstats

    does_stuff()
    
    with cProfile.Profile() as pr:          # Profile the slow function
        does_stuff_slow()

    stats = pstats.Stats(pr)                # Create stats object from profile
    stats.sort_stats(pstats.SortKey.TIME)   # Sort stats by the total time they took
    stats.print_stats()                     # Print the sorted stats in the terminal

if __name__ == '__main__':
    main()
```

When running this, we will see in the terminal all of the operations and how long they took. This will already give us a good idea of what is taking the most time to run, so we can focus on that.

## Using `snakeviz`
However what we want to vizualize all of our operations in the code in the order that they are executed and how long they take? To do that we can use a tool called `snakeviz`.

To use `snakeviz`, we jsut need to modify the code a little bit more:

``` python
import ...

def does_stuff():
    ...

def does_stuff_slow():
    ...

def main():
    import cProfile
    import pstats

    does_stuff()
    
    with cProfile.Profile() as pr:          # Profile the slow function
        does_stuff_slow()

    stats = pstats.Stats(pr)                # Create stats object from profile
    stats.sort_stats(pstats.SortKey.TIME)   # Sort stats by the total time they took
    # stats.print_stats()                   # Print the sorted stats in the terminal

    stats.dump_stats(filename='needs_profiling.prof')   # Dump stats to a file

if __name__ == '__main__':
    main()
```

After running this code, the new file `needs_profiling.prof` will be generated. To view this in `snakeviz`, we type in the following command in the terminal:

``` bash
snakeviz ./needs_profiling.prof
```

`snakeviz` will then open up a new browser window where you can see clearly all of the operations and how long they take in your code.
