Lecture 2 - C

A lecture in C which is a common language that underpins many other 
languages such as pythin. The purpose of this lecture series is not 
that you've learned to code in a specific language but that you have 
fundmentalluy learned how to program.

Traditional languages like C have a lot of syntacical intricacies 
that need to be learned. We will discuss similar topic to last week:

+ Functions
    + arguments, return values
+ Conditionals
+ Boolean expressions
+ Loops
+ Variables
+ ...

Remember that computer laguages have a much less rich vocabulary than 
and syntactical idiosyncracies than a human language, but you need to
be very precise when writing computer code.

Note that code needs to be `correct` to solve the problem set correctly.
Something else to consider is how well you code is `designed`. Solving a 
problem correctly can be done in many ways but code that is efficient 
and succint will be far better than overly verbose code. 'Style' in 
computer science refers to how code is written, clean and clear code, 
with sensible punctation and spacing is always better than mess code.


When printing text to the screen programming lanaguages need to be able
to translate 0s and 1s to the screen. High level lanagues that allow 
you to print text to the screen simply MS Word, google docs are
not great environments to write code in as the have lots of unnecessary 
features that can slow you down. `Integrated development environments (IDEs)`,
or more simply text editors, like Visual Studio Code, or VS code are used.

This first C code snippet prints 'hello world' to the screen:

```C
#include <stdio.h>

int main(void)
{
    printf("hello, world\n");
}
```

Now that we have our program we need to run it, in other words convert or
source code, as it is conventionally called, to 0s and 1s, which we call 
machine code. Note that binary information can also represent instructions
to a computer like print, delete or save depending on the context of how
these binary numbers are interpreted. Sometimes 1s and 0s are interpreted 
as colours, letters, numbers or commands to you computer to do low level
operation such as print letters to the screen. 

Programs that translate source code to machine code are called compilers. 
The purpose of compilers is to convert one language into another.

source code > compiler > machine code

C is a language that does use a compiler.

To compile our code we use the terminal in `VS code` (bottom panel) 
which is a command line interface (CLI), which sits on top of an 
operating system, where we can type commands do do things like 'compile 
my code into machine code'. The dollar sign is the prompt aside which we 
type our commands. The CLI sits on top of an operating system called 
`Linux`, which is in a family of OSs used for programming, serving websites 
and developing apps. The Explorer part of the `VS code` window shows
your file system and is a GUI, a graphical user interface.

To compile our code we type in the terminal:

```C
make hello
./hello
```

Note that make itself is not a compiler. It is a program that knows how
to find and use a compiler on the system to create the program. To use 
the compiler you have to specify a longer set of commands to create a
program, make automates this.

In C to print to the screen we use `printf`, the f stands for
formatted. Strings must be placed in double quotes. We also
need to use a semi colon to end the line.

Some functions can output a side effect like a visual output,
or it can be stored using return values that you can use
and re-use which can be stroed in variable. 

In C `=` is the assignment value for varaibles. It does not
mean equality line equals in maths. In C you need to specify 
what `type`, or more fomally what `data type`  the variable is, 
i.e a string, integer etc. In C this is how the program knows 
how to assign the 1s and 1s correctly. 

The following line of code tells C to interpret the variable
answer as a string:

```C
string answer = get_string("What's your name? "); 
```

If you mix up the variable data type assignment, i.e. you try to
pass a `string` as an `int`, the compiler will throw an 
error. Backslashes are the escape symbol for C. So to take 
a new line you would type `\n` to escape the `n`, which means
in this context to take a new line.

```C
#include <stdio.h>

int main(void)
{

    string answer = get_string("What's your name? "); 
    printf("hello, answer\n");

}
```

When running `make` this error is thrown:

```C
cc     hello.c   -o hello
hello.c:6:5: error: use of undeclared identifier 'string'
    string answer = get_string("What's your name? "); 
    ^
1 error generated.
make: *** [hello] Error 1
```

To avoid this we need to add an additional line to the top
of the script to load an additional library (only works in cs50 container):

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{

    string answer = get_string("What's your name? "); 
    printf("hello, answer\n");

}
```

The `stdio.h` load the standard IO library which allows you to
inlcude input and ouput in your program, this allows you to use
functions like printf and type text in from a keyboard. `cs50.h`
makes it easy to accept input from a user and provides some 
additional functions that don't come with C like `get_string`.

We also need to let C know that answer show be read as a variable 
rather than a string we do this by using a placeholder, `%s`:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{

    string answer = get_string("What's your name? "); 
    printf("hello, %s\n", answer);

}
```

This is where the f comes in, it means that printf does the formating
for you by filling in the placeholder. %s is for strings, %i is for
integers etc.

Arguments, or paramters, are inputs to a function which have side
effects like printing to the screen, which you can't reuse, or they
create `return values` which can be stored in a variable.

arguments > functions > return value

Header files is the mechanism by which you load libraries in C. A library 
is a menu of functions. These are loaded via the lines of code at the top 
of the program to tell the compiler what libraries it needs to load to be
able to interpret the functions in the script. For clarity, the library is
`stdio` the header file is `stdio.h`. 

Note that in C, no such thing as a tring exists as it does in other program
languages.

2h 40min

