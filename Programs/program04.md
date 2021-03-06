# Program 4: Post-fix Calculator

CSCI 211: Programming and Algorithms II<br>
Points: 200

## Objectives

* Deploy a Linked List in an application
* Implement a Stack
* Learn postfix notation
* Practice using cin.peek()
* Practice catching errors

## Overview

> The stack used in this assignment was created in Lab 7. Follow the [Lab 7 instructions](https://github.com/shelleywong/CSCI211-Course-Materials/blob/main/Labs/lab07.md) to create and test your stack.

The standard form of equations, 2 + 4, is called *infix* because the *operator* (+ in this example) is "in" the middle of the *operands* (2 and 4 in this example). This form has the drawback of not being able to control the order in which the operators are applied without using parentheses. For example, if you want 2 + 4 multiplied by 5, you have to write (2+4)*5. While this is easy to do in a programming language, it is difficult to do on a standard calculator.<br>

Postfix notation (also called Reverse Polish Notation or RPN) is much better suited for calculators.  In postfix notation, the operator goes after the operand.  For example, the equation 2 4 + means 2 plus 4, and the expression 2 4 + 5 * means to first apply + to 2 and 4 and then apply * to the result of 2 + 4 and 5.  The result is (2 + 4) * 5 and you didn't have to type in the "(" and ")".<br>

Postfix calculators are easily implemented using a stack data structure. When the user enters an operand, it is inserted on to the stack (in stacks, inserting on top of the stack is called *push*). When the user enters a binary operator, the 2 operands on top of the stack are removed (*popped*), the operator is applied to them, and the result is pushed on to the stack.<br>

If at the end of the input, there is one and only one operand on the stack, then the input was a valid expression and the value on the stack is the result of evaluating that expression.<br>

This assignment is to implement an RPN calculator that can evaluate any legal expression and catch **all** illegal input (except numbers with so many digits that they won't fit into a double). As soon as illegal input is encountered, print the error message (see below) and call `exit(1)`.

> Note: Start early and make incremental progress. If you think you can finish a programming assignment in one day, finish it the first day it is assigned. Students who put off these assignments tend to have a bad time.

## Program Requirements

### Implementation

Implement your stack from scratch using dynamic allocation. In other words, your stack must look like a linked-list. You may not use a stack from a library. Create a class Node to hold the elements of the stack. The Node class must be nested inside the stack class.

Implement your program using the following class:
```
class Dstack
    Stack of doubles that provides at least the following functions:
        push        // push a new element on top of the stack
        pop         // remove and return the top element from the stack
                    // (using a reference parameter)
        empty       // return true if stack is empty, false otherwise
        ~Dstack     // a correct destructor
```
You may use whatever return types and arguments you see fit. You may add additional functions if you need them.<br>

Put `main()` in the file calc.cpp<br>

**The Dstack class may not contain any parts of the calculator program. It is a stack not a calculator.** Put the parts of the program that have to do with the calculator (including the error message) in calc.cpp.<br>

> Note: I recommend writing `Dstack` from scratch. If you copy code from any provided examples, you won't learn as much as if you write it from scratch. By the end of 211, you should be able to write linked-list based classes quickly and easily.

### Input

Your program must read a single equation from standard input. The operators are `+ - * / ^` (^ is power, `2 4 ^` is 2 raised to the power of 4). Numbers can be any double number, for example 4, 4.2, .2, 0.2, 0000.233. You can assume that the number is small enough to fit into a double variable (I won't give you a number with 200 digits).<br>

Your program should ignore all white space (spaces, tabs, newlines).<br>

If two numbers are in a row, there must be a space between them. For example, `42` is the number forty-two, and `4 2` are the numbers four and two.<br>

There do not have to be spaces between numbers and operators. For example, `10 10+` is a legal equation.<br>

There do not have to be spaces between two operators. For example, `10 10 10++` is a legal equation.

### Files

Each class should have a .h file and a .cpp file for the stack (note that the filenames are all lower case):
```
dstack.h
dstack.cpp
```

In addition to the above files, you will need to turn in a file containing your `main()` function:
```
calc.cpp
```

### Program Output

If the input is a correct post-fix expression, your program should print the result.<br>

For example, if the input is `10 10+`, your program should output `20` followed by a newline, and nothing else (don't print any prompts or "answer =").

## Arithmetic Overflow

Assume that all intermediate results and the final result are small enough to fit in a double. For example, I will not test on an expression like:  `2 ^ 1000000000`

## Errors and Error Messages

All errors must be detected in calc.cpp. Recall that stack `pop()` must return an error when the stack is empty (see above).<br>

All errors must be printed in calc.cpp (you cannot print an error message in any of the stack functions).<br>

If the input is not a valid post-fix expression, your program should print the following to standard error (`cerr`):
```
Error: Invalid expression.
```
followed by a newline (not a blank line, but a newline) and then terminate the program. Programs can be terminated by using the `exit()` functions, but you may only call `exit()` from calc.cpp.<br>

Your program must check for all possible errors (except numbers that are too large). This includes all illegal mathematical operations such as divide by zero. The power function is especially problematic. Consider it closely.<br>

## Tips and Best Practices

The algorithm is:
```
while (not the end of the input)

    If the next input is a number, read it and push it on the stack
    If the next input is an operator, read it, pop 2 operands off of the stack, apply the operator, push the result onto the stack
    When you reach the end of the input:

        if there is one number on the stack, print it
        else error
```

I suggest you implement and test the stack before you tackle the calculator aspects of the program. You will have to write a few lines of code in `main()` to do this testing, but it will save you time in the long run.<br>

You can't just use `getline()` or `cin >> string_variable` for this program because you usually don't know if an operator or an operand (a number) will be the next thing in the input stream. You will need to use `cin.peek()` to look at the next character in the input. `cin.peek()` returns the next character without actually reading it. In other words, the next character you read will be the one returned by `cin.peek()`. If it is whitespace (space, tab, newline) you can skip it using `cin.ignore()`. If it is a legal operator, you can read the single character using `cin >> char_variable`. If the next character is a digit, you can use `cin >> double_variable` to read the entire number.  `cin.peek()` returns EOF (which is defined as -1) if end-of-file is true.

> Note: The [cctype library](https://www.cplusplus.com/reference/cctype/) provides some functions that may be useful for checking characters from input.

## General Requirements

Programs should be well formatted and consistent so they are easy to read. The same [General Requirements](https://github.com/shelleywong/CSCI211-Course-Materials/blob/main/Programs/program01.md#general-requirements) listed in P1 apply here, including the guidelines for Comments, Formatting, and Style.<br>

Remember that the first lines of all your files (.h and .cpp) must contain the following comments:
```
// filename
// last name, first name
// ecst_username
```

## Testing Your Program

* Some sample tests and a Makefile are included in 211-starter-pack/211/p4.
* [Lab 2](https://github.com/shelleywong/CSCI211-Course-Materials/blob/main/Labs/lab02.md) includes instructions for testing your program using the provided sample tests.
* I will test your program with additional tests not posted in the test directory. It is a very good idea to design and implement your own set of tests.

## Submitting Your Program

You MUST turn in your assignment several times (at least three) during development. Only the last submission has to compile and run and only the last submission will be graded. The submissions should show your progress (e.g. the first submission has a little bit of the program working, the second a little more, etc). When I suspect cheating I will look at these early submissions for a clear development trend; I want to prevent students from copying finished programs from another student and then turning them in.<br>

Turn in the following files using [Turnin](https://turnin.ecst.csuchico.edu):
```
calc.cpp
dstack.h
dstack.cpp
```

All programming assignments are **individual** assignments -- you are expected to complete and submit your own program.<br>

Assignments can be turned in up to 48 hours late with a 15% penalty.<br>

If you are unable to complete everything on time, you should still turn in whatever work you have. If you turn in nothing, you get a zero. If you turn in something, you receive partial credit.

## Extra credit

No extra credit points will be given to late assignments. You must turn in a non-extra credit version in addition to the extra credit version.<br>

The extra credit version must also pass all of the non-extra credit tests.<br>

You will get 10 points extra credit if your program passes all the extra credit tests, and a fraction of 10 points if it passes some of the extra credit tests. Since these are multi-character operators, you will have to modify how you read the input.<br>

Implement the following operators:
| Operator | Definition | Example |
| --- | --- | --- |
| sqrt | calculate the square root of the previous number | `15 sqrt` means the square root of 15. |
| cos | calculate the cosine of the previous number | `30 cos` means the cosine of 30 |
| sin | calculate the sine of the previous number | `45 sin` means the sine of 45 |
| ave | remove all the operands off the stack, calculate their average and push the result on to the stack | `ave` means the average of all operands on the stack |

> Note: For cos and sin, assume the user is entering degrees. Since the cmath functions cos() and sin() expect the angle in radians you will have to convert the number to radians. Use the constant M_PI from cmath to perform the conversion.
