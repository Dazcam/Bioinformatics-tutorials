Lecture 2 - C

A lecture in C which is a common language that underpins many other 
languages such as python. The purpose of this lecture series is not 
that you've learned to code in a specific language but that you have 
fundmentally learned how to program.

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

Note that code needs to be `correct` to solve the problem you have set correctly.
Something else to consider is how well your code is `designed`. Solving a 
problem correctly can be done in many ways but code that is efficient 
and succinct will be far better than overly verbose code. `Style` in 
computer science refers to how code is written, clean and clear code, 
with sensible punctation and spacing is always better than mess code.


When printing text to the screen programming lanaguages need to be able
to translate 0s and 1s to the screen. High level lanagues that allow 
you to print text to the screen simply such as MS Word, google docs are
not good environments to write code in as they have lots of unnecessary 
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

Now that we have our program we need to run it, in other words convert our
source code, as it is conventionally called, into 0s and 1s, which we call 
machine code. Note that binary information can also represent instructions
to a computer like print, delete or save depending on the context of how
these binary numbers are interpreted. Sometimes 1s and 0s are interpreted 
as colours, letters, numbers or commands to you computer to do low level
operation such as print letters to the screen. 

Programs that translate source code to machine code are called compilers. 
The purpose of compilers is to convert one language into another.

> source code > compiler > machine code

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
and re-use which can be stored in a variable. 

In C `=` is the assignment value for varaibles. It does not
mean equality line equals in maths. In C you need to specify 
what `type`, or more fomally what `data type`  the variable is, 
i.e a string, integer etc. In C this is how the program knows 
how to assign the 1s and 0s correctly. 

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
include input and ouput in your program, this allows you to use
functions like `printf` and type text in from a keyboard. `cs50.h`
makes it easy to accept input from a user and provides some 
additional functions that don't come with C like `get_string`.

We also need to let C know that answer should be read as a variable 
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

> arguments > functions > return value

Header files is the mechanism by which you load libraries in C. A library 
is a menu of functions. These are loaded via the lines of code at the top 
of the program to tell the compiler what libraries it needs to load to be
able to interpret the functions in the script. For clarity, the library is
`stdio` the header file is `stdio.h`. 

Note that in C, no such thing as a string exists as it does in other program
languages.

Each program has a set of `data types` that it can be assigned. These are 
user specifed and informs the prorgam how a pattern of 0s and 1s should be 
assigned.

+ bool - True and False
+ char - single chacter
+ double - A double is a real number that is more precise (more numbers 
  after the decimal point).
+ float - a real number with a decimal point
+ int - A single integer
+ long - A longer integer
+ string - Words

The functions available in the CS50 library to get user input are:

+ get_char
+ get_double
+ get_float
+ get_int
+ get_long
+ get_string

How would you go about printing types of data? We use special characters 
for called `format codes` for this:

+ %c - character
+ %f - float, double
+ %i - integer
+ %li - long integer
+ %s - string

There are specific `operators` in certain langages that allow you to carry 
out mathmatical operations (I know all of these). The only wierd on is 
remainder which is `%` in C.

There are also some variables, sometimes called syntactic suagr, that allow 
you to express certain commands more succinctly. 

```C
# Create a counter by adding 1
int counter = 0; 
counter = counter + 1;

# Written more succinctly
counter += 1

# Or
counter ++
```

Let's make a basic calculator in C

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{

    int x = get_int("x: ");
    int y = get_int("y: ");
    printf("%i\n", x + y)
     
}
```

When running the above program to add 2 billion + 2 billion we get an
incorrect answer, a long negative integer. This is because each of our
datatypes have use a specific and finite number of bits to represent 
them, this can vary between programs and old vs. new computers, so is 
not standardised across environments, but in this environment we are 
32 bits for an integer. 

So if 8 bits can give you integerts up to 256, 8 bits gives you 256
permutations of 0s and 1s, 32 bits gives you ~4 biliion permutations. 
This calculation fails because we also need to encode negative numbers 
so 32 bit encoding can support ~2billion positive and 2 billion negative 
numbers. We need to change calculator in consider long integres to get 
around this problem. 

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    // Prompt user for x
    int x = get_int("x: ");

    // Prompt user for y
    int y = get_int("y: ");
  
    // Perform addition  
    printf("%i\n", x + y)    

}
```

Using `long` instead of `int` assigns 64 bits to an integer, so our 
calculator can deal many more postive and negative number now, but 
bear in mind that regardless of how much memory we allocate here the
number of number the calcuator can parse will always be finite. This
is the same for floats. A computer with finite memory cannot capture
the infinite number of mathmatical permuations that exist in the real
world. Floating point imprecision refers to the inability of computers
to represent all possible real numbers in langauges like C. There are 
solutions to this in scientific computing that give you more digits, 
but at a fundanmental level this is something to bear in mind.

Another thing to bear in mind that when dividing integers, for example,
the ouput with be returned as an integer. If you were to assign the 
output of this caculation to a float, to handle a caculation such as 
`2 / 3` a truncation would occur. Rather than get the answer you would
expect like `0.6667`, you would get `0.00000`. Effectively, the answer 
has been truncated so that the only part of `0.6667` that has been 
retained is the first digit, the `0` before the decimal point, aka the 
integer. You can use Type conversion to get around this, i.e. change the
data type of at least one of the varaibles in the calculation such as:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    // Prompt user for x
    int x = get_int("x: ");

    // Prompt user for y
    int y = get_int("y: ");

    // Divide x by y
    float z = (float) x / (float) y;

    // Print result  
    printf("%.50f\n", z)    

}
```

Integer overflow occurs when you run out of bits to represent a number
higher than the number of bits you have available, resulting in a 0.
This was the problem that was behind the millenium bug issue as many old
computers stored years as a two digit number rather than a 4 digit number.
Jan 19th 2038 is the next time when a similar issue may occur as many 
computers count time in seconds. The start date for this is 1st Jauary 
1970. Though most compuetrs used 32 bits for integers meaning that 
32 bit computers can olny count up to 2147483647 seconds, ~ 2 billion
seconds. That number of seconds occurs on Jan 19th 2038. 

Note that for negative numbers the bit that is leftmost represents 
postitive or negativity, i.e. `0 = +`, `1 = -`. (There is a fancier
representation for `-0`. This is why most hardware is no 64-bit.

Also be mindful that there could be rounding errors that can  occur 
when using low level languages such as C due to how numbers are
processed. 

**Conditional statements**

What about conditional statements in C. Conditional statements such as
`If` are a programming contructs that asks a True or False question. 
This is written in C as follows:

```C
# If
if (x < y)

    printf("x is less than y\n");


# If / Else
if (x < y)
{
    printf("x is less than y\n");
} 
else
{
    printf("x is not less than y\n");
} 


# Else if
if (x < y)
{
    printf("x is less than y\n");
} 
else if (x > y)
{
    printf("x is greater than y\n");
} 
else
{
    printf("x is equal y\n");
} 
```

Note that in C it is not necessary to have curly braces under a conditional
statement if you only have a single line of code underneath it. Howvere, 
for best practice you should use curly braces at all time.

The construct `const` allows you to assign a variable in a strict way such 
that you cannot change, or reassign that varible later in the program. This
is an example of defensive programming. Another convetion is that when a
constant variable is assigned the variable is capitalised to make it stand 
stand out. 


```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    const int mine = 2;
    int points = get_int("How many points did you lose? ");

if (points < mine)
    {
        printf("You lost fewer points than me.\n");
    }
else if (ponits > mine)
{
    printf("You lost more points than me.\n");
} 
else
{
    printf("You lost the same number of points as me.\n");
}
}
```

There is no semi colon used after loops or conditionals.

> A nice technique the instructor uses is to write out pseudocode in
comments as a list of ideas to code that he then fills out with code 
later. In effect, he sets out his recipe, or list of steps in English
first within the script and then fills it out in code later. 

Code to test odd and even:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    const int mine = 2
    int points = get_int("How many points did you lose? ");

    if (points < mine)
    {
        printf("You lost fewer points than me.\n");
    }
    else if (ponits > mine)
    {
        printf("You lost more points than me.\n");
    } 
    else
    {
        printf("You lost the same number of points as me.\n");
    }
}
```

Not that if you are writing code that refers to a string in C the 
string must be in double quotes, if you are refrring to a single 
character, i.e. a `char` variable, the character must be in single 
quotes. 

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{

    // Prompt user to agree
    char c = get_string("Do you agree? ");

    // Check whether user agreed
    if (c == 'y' || c == 'Y')
    {
        printf("Agreed.\n");
    }
    else if (c == 'n' || c == 'N')
    {
        printf("Not agreed.\n");
    } 
}
```

**Loops**

Notice the difference between using while and for loops. This has an 
impact on scope; `i` is not available in the global environment when 
assinged  as it is in the paraentases of the for loop.

```C
// while
int i = 0;
while (counter < 3)
{
    printf("meow.\n");
    i++;
}

// for
for (int i = 0; i < 3; i++)
{
    printf("meow.\n");

}
```

**Functions**

To write a function in C you need to give the function a name, specify
whether the function has any inputs, if not you can explicitly state this
by typing `void` in parentheses next to the function name. It is
also necessary in C to specify what the return type of the function is,
again if nothing is returned void is used to make this explicit.

```C
#include <stdio.h>

void meow(void)
{
    print("meow\n");
}

int main(void)
{
    for (int i = 0; i < 3; i++)
    {
        meow();
    }
}
```

Note that in C functions  need to be declared in a script before they are
used in the script however it is possible to decalre your function in 
a script by creating a prototype fucntion at the top of the scripts, so for 
meow like so:

```C
```C
#include <stdio.h>

void meow(void);
```

This tells C that the function will be defined elsewhere in the script. More
recent programming langages will check the entiore script for function 
definitions.

To assign variables in a function in C and create a return value we
can use the following:

```C
#include <cs50.h>
#include <stdio.h>

// Tell C what functions to expect later in script 
float discount(float price, int percentage);

int main(void)
{
    float regular = get_float("Regular Price: ");
    int percent_off = get_int("Percent Off: ");
    float sale = discount(regular, percent_off);
    printf("Sale price: %.2f\n", sale);
}

float discount(float price, int percentage)
{
    return price * (100 - percentage) / 100;
}
```

Notice we can send a variable directly to return here. 

**Creating basic computer game**

We can create ASCII art to create a primitive game environment
map like the original Super Mario brothers, which contains
particular icons or obstacles.

Notice the use of the `do while` loop. This checks the while 
condition after the first instance of the loop is run. This 
constuct is often used to ask a user for a positive value. In
this case a value > 1. So, in this case, we break out of the 
loop when the user has cooperated.

A single column of question marks:

```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int n;
    do 
    {
        n = get_int("Width: ");
    }
    while (n < 1)

    for (int i = 0; i < n; i++)
    {
        printf("?");
    }  
    printf("\n");
}
```

A cube barrier:


```C
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int n;
    do 
    {
        n = get_int("Size: ");
    }
    while (n < 1)

    // For each row
    for (int i = 0; i < n; i++)
    {
        // For each column
        for (int j = 0; j < n; i++)
        print("#");
    }  

}
```

Remember for functions in C you must specify the return type, the
name for the function and the argumnets for the function in that
order. If none of these are applicable you write void.



