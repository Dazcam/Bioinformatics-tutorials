Harvard computer science course [CS50 course](https://www.youtube.com/watch?v=8mAITcNt710)

L0 - Scratch

Computer Science is about problem solving and creativity, about bring something new to life. 
That is not to say that it is not without frustrations. What ultimately matters in this course
is not where you end up relative to your classmates but where you end up relative to where you 
began.

How does a compute count? If a human was counting, we could use our fingers to represent people 
in a room. A computer uses a binary system, zeros and ones to represent numbers. One would be 001,
and two would be 010. A bit, i.e. a 1 or a zero, is the most basic unit of information
in computing. Humans operate in the decimal system, 0-9, the prefix 'dec', meaning ten. A 
BYTE is 8 bits. The binary system is used as using a switch / transistor based system, 
1 = on and 0 = off, is a very efficient way to store information for electronic systems. 

Binary and decmimal systems are related in a columnar fashion. Considering the fiest thre columns
the decimal system columns are annotated as 10^2; 10^1; 10^0 whereas the binary system is 
annotated as 2^2; 2^1; 2^0. The higher the number the more bit that are required to encode the 
number in binary. 

But how is non-numerical code stored? A gold standard was agreed to represent, for example, letters.
Capital A is represented as 65 or 01000001. But how does the computer know when 01000001 is supposed 
to represent A or 65? How a paticular binary pattern is represented to us in a GUI is dependent on 
how the program has been written to represent that binary pattern. Excel may represent it as a 
number, whereas Word may represent it as a letter. It is context dependent. This system behind this
is ASCII the American Standarised Code for Information Interchange. However, as ASCII was designed
for American English it had to be updated to represent all the characters in different laguages 
around the world and things like emojis.

Unicode is a supraset of ASCII which maps many more characters using up to 16, or 32 bits rather than 8 which means there is space to represent up to 4 billion characters. 

Colours are represented as a mix of R,G,B which are also represented as numbers. For video, 
the computer needs to be able to represent time. So a series of images, that are encoded in a 
binary format, are played one after the other. For audio, MIDI stores notes as a sequence of
numbers. But to capture music fully we need to represent, the notes themselves, the duration,
timbre and the volume, so in order to capture analogue human realities qualitatively, these 
apsects of music have to be represented digitally to the computer. 

Remember that all the computer uses are 0s and 1s, like switches or lightbulbs,
and it is up to us to write software that can interpret that binary input in 
some way to accomplish the task we are interested in doing. The sotware is 
representing the binary infomation in a particular way. 

An algorithm is a step by step process, or set of instructions, for solving a 
problem. Programming requires human intuition to solve problems algoritmically. 
Uses an analogy of designing an algorithm to serach through a phone book for a 
contact.

To get an idea of the code you will need to write to solve a problem it is 
often useful to write pseudocode. This is a form of words we use to communicate 
our ideas and to formalise the steps / processes we need to carry out to 
solve our problem such as:

1. Pick up phone book
2. Open to the middle of the phone book
3. Look a page
4. If person is on page
5.     Call person
6. Else if person is earlier in the book
7.     Open to middle of left half of the book
8.     Go back to line 3
9. Else if person in later in book
10.     Open to middle of right half of the book
11.     Go back to line 3 
12. Else
13.     Quit

Pseudocode is useful for conceptualising what edge cases may be which helps to
anticipate potential areas where bugs culd be introduced.

Most programming languages have `functions` which are typically actions or verbs
that represent short bits of code that solve small problem. We use a group of
fucntions to solve a bigger problem.

`Conditionals`, like `If`, `Else if` or `Else` solve conditional problems, you
can thing of these like forks in the road. The direction we take depends on the
answer to a particular question. These questions are called `Boolean expression`
named after a mathematician called Bool that have True or False answers.

The last thing to mention are 'loops' which allow you to run a, repetitive or 
iterative process a set number of times, or based on a specific condition, using 
only a few lines of code.

Even though programming languages look at first cryptic, the vocabulary and 
processes they execute are very similar.

Scratch is a graphical language that uses blocks, like puzzle pieces, that represent
common computer language processes that helps programmers get to grips with
scripting fundamentals using a drag and drop process.

