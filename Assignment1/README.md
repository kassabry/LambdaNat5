# README

## Assignment 1

This assignment is worth 15% of the total grade.

Deadline for hand-in is Friday, September 27.

Let me know if there are any questions ... I list some resources that may be helpful:

- I added more comments to the [LambdaNat0 interpreter](https://github.com/alexhkurz/programming-languages-2019/blob/master/Lab1-Lambda-Calculus/LambdaNat0/src/Interpreter.hs). 

- A Haskell tutorial: [Learn you a Haskell](http://learnyouahaskell.com/). But I don't think you need to know Haskell to do the assignment. Just three or four new cases in the function `evalCBN` of the interpreter. Once you get the logic right, you should be able to guess the Haskell syntax by comparing with the code that is already there. If you get stuck let me know ...

- For specialised enquiries, documetation on Haskell libraries is at [Hoogle](https://hoogle.haskell.org/). 

### Instructions

You may form groups of up to 4 students. If you think you have a reason for an exception let me know.

Each group submits their answer by sending me a link to a github repository via [email using this link](mailto:akurz@chapman.edu?subject=CPSC-354-Assignment-1). This repository needs to contain a folder Assignment 1 which must be identical which the one [here](https://github.com/alexhkurz/programming-languages-2019/tree/master/Assignment1), with the exeption of the following:

- You should create a folder `/src/` and populate it with the files generated by the parser for `LambdaNat5`.

- You should create the file `/src/Interpreter.hs`, adapted from [LambdaNat4](https://github.com/alexhkurz/programming-languages-2019/blob/master/Lab1-solutions/LambdaNat4/src/Interpreter.hs).

- You may add programs to the folder `test`.

- You will provide in the folder `solutions` the following programs

      even.lc
      length.lc
      member.lc
      append.lc
      reverse.lc
      le.lc
      sort.lc
    They should be executable by running `stack exec LambdaNat-exe solutions/NameOfTheProgram.lc`

I ask you to keep to these rules because I will run a script that automatically tests your interpreter on your solutions (and possibly runs further tests). 

I cannot give you credit for programs that do not execute.

### Introduction

The purpose of the assignment is to build a simple programming language that has function definitions, function application, numbers, conditionals, recursion and lists.

The assignment starts with [LambdaNat4](https://github.com/alexhkurz/programming-languages-2019/tree/master/Lab1-solutions/LambdaNat4), which is our programming language that has function definitions, function application, numbers, conditionals, and recursion.

The [grammar for LambdaNat5](https://github.com/alexhkurz/programming-languages-2019/blob/master/Assignment1/grammar/LambdaNat5.cf) adds to the grammar `LambdaNat4.cf` the following

    EHd.       Exp6 ::= "hd" Exp ;
    ETl.       Exp6 ::= "tl" Exp ;
    ENil.      Exp9 ::= "#" ; -- EndOfList, aka empty list
    ECons.     Exp9 ::= Exp10 ":" Exp9 ;

According to the rules `ENil` and `ECons` we can build lists such as

    a:b:c:#

`hd` and `tl` are pronounced "head" and "tail", respectively. The first task is to adapt the interpreter of `LambdaNat4` in such a way that head and tail compute as 

     hd a:b:c:#   --->   a
     tl a:b:c:#   --->   b:c:#

**Remark on the side:** Lists can also be nested in order to form trees as in 

    Plus:(N1:#):(Times:(N2:#):(N3:#):#):#

If you wonder why we need the EndOfList symbol `#` above, then the answer is that in the tree above it is redundant if we have agreed that the `N` symbols are constants (take no arguments) and that `Plus` and `Times` are binary (take exactly 2 arguments). Then we could write the above instead as 

    Plus:N1:(Times:N2:N3)

(which, by the way, is an abstract syntax tree for `1+2*3`.) The reason we need the EndOfList symbol is that lists are meant to work in situations where we do not know at *programming time* (aka compile time) how long the lists will be at *run time*. 

**Remark:** The previous remark hides a deeper duality between two different readings of `#`, one as the empty list (often written as "nil") and the other has the EndOfList symbol. This duality is known as the duality of **algebras** and **co-algebras**. In the algebraic view, we *build* or *construct* finite data from smallest data (here `ENil` with concrete syntax `#`) by inductively applying a finite number of rules (here `ECons` with concrete syntax `:`). In the co-algebraic view, data is potentially infinite and we *observe* or *deconstruct* this data (here using `hd` and `tl`) until we get to the end symbol (here `#`).

### Assignment1.A (5 out of 15)

Write an interpreter for LambdaNat5 by modifying the code of the [LambdaNat4 Interpreter](https://github.com/alexhkurz/programming-languages-2019/blob/master/Lab1-solutions/LambdaNat4/src/Interpreter.hs). Test it with the programs in [test](https://github.com/alexhkurz/programming-languages-2019/blob/master/Assignment1/test/test-interpreter4.lc) as well as your own examples.

### Assignment1.B

Implement and run the following functions in LambdNat5. Save the program answering question Assignment1.B.xyz as `solutions/xyz.lc`. All programs must be executable by the interpreter. You cannot get credit for a program that does not run.

#### Assignment1.B.even (1 out of 15)

`even l` evaluates to `S 0` if `l` is a list of even length and evaluates to `0` if `l` is not of even length, for example

    even a1:a2:a3:a4:#      --->   S 0    
    even a1:a2:a3:a4:a5:#   --->   0    

#### Assignment1.B.length (1 out of 15)

`length l` evaluates to the length of `l` for any list `l`. For example,

    length a1:a2:a3:a4:a5:#   --->   S S S S S 0


#### Assignment1.B.member (1 out of 15)

`member x l` returns 1 if `x` is a member of the list `l` and 0 if it is not. For example,

    member a a:b:#        --->   S 0
    member S 0  0:S 0:#   --->   0

#### Assignment1.B.append (1 out of 15)

`append l1 l2 ` returns the list obtained from appending `l1` to `l2`. For example, 

    append # #             --->   #
    append 0:S 0:# #       --->   0:S 0:#
    append # a:b:#         --->   a:b:#
    append 0:S 0:# a:b:#   --->   0:S 0:a:b:#

#### Assignment1.B.reverse (2 out of 15)

`reverse l ` returns the list obtained from reversing `l `. For example, 

    reverse #         --->   #
    reverse a:b:c:#   --->   c:b:a:#


#### Assignment1.B.le (1 out of 15)

`le n m ` returns `S 0` if as numbers `n` is smaller or equal to `m` and returns `0` if `n` is bigger than `m`. For example,

    le S 0 S S 0 --> 0
    le S 0 S 0 --> S 0

#### Assignment1.B.sort (3 out of 15)

If `l` is a list of numbers, then `sort l` returns a list that has the same elements as `l` but sorted from smaller to larger. `sort` can be specified mathematically as follows

    sort #  =  #
    sort n:l  =  insert n l

    insert n #  =  n:#
    insert n m:l  =  
        n smaller equal to m -> n:m:l
        otherwise -> m:(insert n l)

Example:

    sort S S 0:S 0:# --> S 0:S S 0:#








    
    