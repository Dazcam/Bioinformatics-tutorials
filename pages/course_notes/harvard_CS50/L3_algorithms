## L3 Algorithms

## Efficiency

Computer Science define the time an algorithm will take as big O, 
or in words 'on the order of', or 'the upper bound'. 

- O(n^2)
- O(n log n)
- O(n)
- O(log n)
- O(1)

To describe the lower bound we use big Omega.
When the upper bound and the lower bound are the same we say big Theta.

Linear search algorithm 
Binary search (ordered search)
Any algorithm that is divinding N by 2 

Resources time to run the code, write the code 

```C
include <cs50.h>
include <stdio.h>

int main(void)
{
    int numbers[] = {4, 6, 8, 2, 7, 5, 0};
    
    for (int i = 0; 1 < 7; i++)
    {
        if (numbers[i] == 0)
        {
            printf("Found\n");
            return(0);    
        }
    }
    printf("Not found\n");
    return(0);
}
```

A string is not a primitive built into the language

### Data structure

C lets us create out own data structures using typedef and struct syntax:

```C
typedef struct
{
    string name;
    string number:
}
person;
```

- typedef: keyword that tells the compiler we are defining a data type
- struct: tells the compiler this data type is more complex that an int or float,
that there is deimesionality to this data type.
- person: is the name of the data type

Clang will know that the data type `person` will have two string elements per entry.

This is not the same as object oriented programming (C++, Python, Java etc.) 
those create objects called classes that can store variables and functions.

## Sorting

Two options discussed:
 
**Selection sort**

for i from 0 to n - 1
    Find smallest number between numbers[i] and numbers[n - 1]
    Swap smallest number with numbers[i]

The effciency of this algorithm

- n + (n-1) + (n-2) + ... + 1
- n(n+1)/2
- (n^2+n)/2
- n^/2 + n/2

If n is a very big number we can aprroximate this the efficency of this procedure 
as O(n^2). As a rule of thumb look at the highest order value to estimate the
efficency. Note that Omega is also n^2 for selection sort.

**Bubble sort**

Repeat n - 1 times
    For i from 0 to n-2
        If numbers[i] and numbers[i+1] out of order
            Swap them
        If no swaps
            Quit

- (n-1) x (n -1)
- n^2 1n - 1n + 1
- n^2 - 2n + 1

Again bubble sort is approx. O(n^2) however Omega is n.

Algorithms that are n^2 tend to be frowned upon as the take too long to run.

How can we do better than this? Fundamentally better than O(n^2). The sorts
we've done so far are comparison sorts, however we want to use recursion
here.

Recursion is a programming technique where a function calls itself. Thinking
back to the mario task a few weeks back, the pyrimids you crew were recursive 
structures.

A segmentaion fault means that you have touched memroy in the computer that
you shouldn't have. 

**merge sort**

Merge sort uses recursion.

If only one number
    Quit
Else
    Sort left half of numbers
    Sort right half of numbers
    Merge sorted halves

Merge sort uses more memory to create a second array, but that means the total running time O(n log n).
The omega and theta are also n log n. 
