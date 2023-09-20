Lecture 2 - Arrays

We need to compile source code into machine code, until now we
have been using `make`. `make` is not a compiler, it is a
utility that comes with most systems that uses a compiler
on the system to translate source code and make it 
executable. When running `make hello` make looks for
a file called hello in the directory and complies the code.

Make is actually running a command called clang, which
means C language. Clang is the compiler. We can run the 
compiler manually running `clang hello.c`. This produces a 
file called `a.out` which stands for `assembler output` 
which is a convention, i.e. the default name of the executable 
program. Make renames this for you to match the name given 
in the source code script.

If you pass the `-o` argument to clang and the name you
want for the executable it will produce the same output
as make `clang -o hello hello.c`. 

If we want to use bespoke libraries in source code that we
wish to compile we need to provide a `linker` flag to 
the clang command to tell it to add the machine code
the authors of the libraries wrote to your program so 
that the functions within those libraries are avaialable 
to your program, `clang -o hello hello.c -lcs50`. You 
don't need to link to `stdio.h`. Note that make
also does this for you automatically.

When compiling your code with make 4 distinct things are happening:

- Preprocessing: Reads preprocess directives at the top of the script and copies in required functions from associated libraries
- Compliling: Takes source code after preprocessing and changes it into assembly language (which is a low a level language you can get  that feeds basic instructions into the CPU (i.e. your INTEL processor). Each CPU has it's own constuction set of commands this is why programs written for Windows do not work on a mac.
- Assembling: The assembly language is recoded to machine code
- Linking: Links all 0s and 1s from source code and associated libraries into one file.

How to debug

- Use print statements
- Use a debugger: `debug50` is a debugger in VScode. Use it to step through your code at 
your own pace.
- Talk through what you are doing step by step helps work out errors

When using a debugger:

- Step over a line means that you run the line of code without stepping into any fucntions that were used in that line of code
- Step into: means step into the function that was used in that line of code.


## Types

In most syetmes this is how large a data type is in terms of memory allocation:

- bool: 1 byte
- char: 1 byte
- double: 8 bytes
- float: 4 bytes
- int: 4 bytes
- long: 8 bytes
- string: ? bytes

The 0s and 1s are stored on black chips on circuit boards, equating to the RAM
of the computer. 

Make a program to take the average of a number:

```C
#include <studioio.h>

int main (void)
{

    int score1 = 72;
    int score2 = 73;
    int score3 = 33;

    printf("Average: %f\n", (score1 + score2 + score3 / 3);

}
```

This throws an error as the result is not an integer. However, if you change the 
divisor to include a floating point value, i.e. chnaging it to 3.0, C will treat 
the result as a floating point value. As long as one number in the calculation
is a floating point value this will always be the case.

An array is another type of data that allows you to store mutiple values of the
same type contigously. It lets you create memory for any number of values using
the same varible name. This doesn't use less memory to store the array, it just 
means that there is a mechanism to write cleaner code by using indicies for each 
value in the array rather than having individual variable names for each value.
For example in C you decalre an array as follows (note that in C arrays are 0 
indexed):


```R
int scores[3];

score[0] = 72;

score[1] = 73;

score[2] = 33;

```

So to improve the script above:

```C
#include <cs50.h>
#include <studioio.h>

int main (void)
{

    int n = get_int("How many scores? ");    

    int scores[n];

    for (int 1 = 0; i < 3; i++)
    {
        score[i] = get_int("Score: ");
    }

    printf("Average: %f\n", (score[0] + score[1] + score[2] / n);

}
```

Note that when working with `chars` C can report back the ASCII numbers associated
with those `chars`:

```C
#include <studioio.h>

int main (void)
{

    char c1 = 'H';
    char c1 = 'I';
    char c1 = '!';

    printf("%i %i %i\n", (int) c1, (int) c2, (int) c3);

}
``` 

This prints 72 73 33 to the screen. Note that we use t`type casting` to change the
chars to ints in the printf statement.

The point of going through this is to demonstarte that the data type `string` is an
array of characters. Note that to indicate the termination of a string the computer 
uses a `sentinal value` aka a NULL character or `\0` to indicate this which means 
the string uses an additional byte in memory to accomodate this. This is how strings 
are represented in memory.

Let's write a short script to work out the length of a string:


```C
#include <cs50.h>
#include <studioio.h>

int main (void)
{

    string name = get_string("Name: ");

    int i = 0;
    while (name[i] != '\0')
    {
        i++;
    }

    printf("%i\n", i);
}
``` 

This can be anstracted out:

```C
#include <cs50.h>
#include <studioio.h>

int string_length(string s);

int main (void)
{

    string name = get_string("Name: ");
    int length = string_length(name);
    printf("%i\n", length);

}

int string_length(string s)
{
    int i = 0;
    while (s[i] != '\0')
    {
        i++;
    }

    return i;
}
```

The manual for cs50.h is to be found on manual.cs50.io.

Lets write some code to print the characters of a string to the screen:

```C
#include <cs50.h>
#include <studioio.h>
#include <string.h>


int main (void)
{

    string s = get_string("Name:  ");
    printf("Output: ");
    int length = strlen(s)    

    for (int  i = 0; n = strlen(s); i < n; i++)
    {
        printf("%c", s[i]);
    }
    printf("\n);
}
```

Note that you can set multiple varibales in a for loop in C. 

Lets write another to capitalise the lowercase letters in a string taking 
advantage of the ASCII numbers. Capital letters are 32 places behind
there corresponding lowercase equivlents in ACSII code.

```C
#include <cs50.h>
#include <studioio.h>
#include <string.h>


int main (void)
{

    string s = get_string("Before: ");
    printf("After:  ");
    int length = strlen(s)    

    for (int  i = 0; n = strlen(s); i < n; i++)
    {
        if (s[i] >= 'a' && s[i] <= 'z')
        {
            printf("%c", s[i] - 32);
        }
        else
        {
            printf("%c", s[i]);
        }

    }
    printf("\n);
}
```

You could replace the much of this code using predefined functions.

To allow your program to take command line argumnets you nedd to add these 
as in the `main` statement. Here is a simple example:

```C
#include <cs50.h>
#include <studioio.h>


int main (in argc, string argv[])
{
    if (argc == 2)
    {
        printf("hello, %s\n", argv[1]);
    }
    else
    {
        printf("hello, world\n");
    }


}
```

- `argc`: stands for argument count and stores an integer for how many words the user typed
- `argv[]`: stands for argumnet vector, i.e. an array containing all of the wrods the human typed

Main has another feature that we haven't discussed yet called the `exit status`. Any time 
that a program fails you should return a non zero value, usually 1. In programming, 0
means that everything worked. Error codes are usually a number designated by the program writer.

Crytography

key       >        >
plaintext > cipher > ciphertext 

