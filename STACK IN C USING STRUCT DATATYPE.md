# STACK PROGRAM USING STRUCT IN C

Created: May 9, 2021
Created by: Rajat Chauhan
Status: InProgress
Tags: Article

> In the following article, we will learn how STACKS are implemented in C by making a program STEP BY STEP.

# PREREQUISITES

- C Compiler
- A Computer
- Little bit of thinking

Let us begin with a fresh C file, the one we will be using is simply called `stack.c`.

We first import the standard input/output library required for C compilation:-

```c
#include <stdio.h>
```

We declare 3 variable, 

`MAXSIZ` to set the maximum amount of items (size of stack).

`stack` which is a struct object which will comprise of:

`array[MAXSIZE]`which is the array we will use to store values

`top` which is an integer value used for determining the index of the top most element in the stack 

Here is the implementation:

```c
#define MAXSIZE 5

struct stack
{
    int array[MAXSIZE];
    int top;
};
```

The C language contains the `typedef` keyword to allow users to provide alternative names for the primitive (e.g., int) and user-defined (e.g struct) data types.

### Reason for using `typedef`?

Using typedef struct results in a cleaner, more readable code, and saves the programmer keystrokes. However, it also leads to a more cluttered global namespace which can be problematic for large programs.

The primitive type is `struct` which will apply an alias to called `STACK`. This essentially means we created a unique datatype to store the values we want instead of relying on predefined data structure types.

```c
typedef struct stack STACK;
```

Now that we have created the structure, lets use it

We are going to name it `s`:

```c
STACK s;
```

In a Stack data structure we have following core operations:

1. Push - Individual items can be added in a stack.
2. Push - Objects can be retrieved/removed using this operation.
3. Display - This operation simply prints our stack to the console.

Let us declare them:

```c
void push(void);
int  pop(void);
void display(void);
```

The reason as to why we picked "pop" operation as an integer (`int`) value is because in order to retrieve the value removed,  we need to return it from the function.

### Lets start with the PUSH operation:

```c
**void push () 
{
    int num;
    if (s.top == (MAXSIZE - 1))
    {
        printf ("Stack is Full\n");
        return;
    }
    else
    {
        printf ("Enter the element to be pushed\n");
        scanf ("%d", &num);
        s.top = s.top + 1;
        s.stk[s.top] = num;
    }
    return;
}**
```

Firstly, we assign `num` variable to get input data from user, this `num` value will be pushed into the stack.

The we check if the `top` vale in our structure has been reached meaning either our stack is full or not :

### if (s.top == (MAXSIZE -1))

If the top value matches the the maxsize then we print Stack is Full :

### **printf ("Stack is Full\n");
        return;**

If the stack is not full then we SCAN (take input) the new value and add `1` to `top` index:

### **printf ("Enter the element to be pushed\n");
        scanf ("%d", &num);**

We then also store value of `num` (new value) to the address (index) of top:

### **s.top = s.top + 1;
        s.stk[s.top] = num;**

The final return; is empty as void function don't return values.

### The POP Operation:

```c
int pop ()
{
    int num;
    if (s.top == - 1)
    {
        printf ("Stack is Empty\n");
        return (s.top);
    }
    else
    {
        num = s.stk[s.top];
        printf ("poped element is = %dn", s.stk[s.top]);
        s.top = s.top - 1;
    }
    return(num);
}
```

This time we declare `num` because it will be used to assign the top value of the stack in order to remove it.

The following `if` condition checks either the stack is empty or not.

### if (s.top == -1)
    {
        printf ("Stack is Empty\n");
        return (s.top);
    }

### Reason for `top = -1`?

As you for a normal array

## [ a1, a2 , a3, a4, ........ ]

The index is as follows:

## [ a1, a2 , a3, a4, ........ ]

      **0       1        2       3** 

So if an array has 1 element its index is 0, to reproduce such behaviour in our STACK the top value is -1 when the stack is empty. When an item is pushed into the stack the top value becomes 0. Hence, the index of the item can also be considered 0.

This is the reason why we check whether `s.top`'s value is equal to -1 or not because if it is -1 then we can print STACK IS EMPTY.

### else
    {
        num = s.stk[s.top];
        printf ("poped element is = %d\n", s.stk[s.top]);
        s.top = s.top - 1;
    }

As you can see from above, we assign `num` as the top most item in our stack, we print that the item is removed but in reality we don't actually remove the element from the memory.

Instead, we decrement the `top` value, so our stack makes it seem like we have one less item. This gets rid of the hassle of removing the element from memory and is more faster and efficient.

At last, we return the popped element for any other use:

### return (`num`);

### The Last Operation: Display

```c
void display ()
{
    int i;
    if (s.top == -1)
    {
        printf ("Stack is empty\n");
        return;
    }
    else
    {
        printf ("\n The status of the stack is \n");
        for (i = s.top; i >= 0; i--)
        {
            printf ("%d\n", s.stk[i]);
        }
    }
    printf ("\n");
}
```

We basically create a for loop and index from `i=0` till `s.top`.

Now in the main function you can utilise these functions as per your need, Here is an example :-

```c
void main ()
{
    int choice;
    int option = 1;
    s.top = -1;
 
    printf ("STACK OPERATION\n");
    while (option)
    {
        printf ("------------------------------------------\n");
        printf ("      1    -->    PUSH               \n");
        printf ("      2    -->    POP               \n");
        printf ("      3    -->    DISPLAY               \n");
        printf ("      4    -->    EXIT           \n");
        printf ("------------------------------------------\n");
 
        printf ("Enter your choice\n");
        scanf    ("%d", &choice);
        switch (choice)
        {
        case 1:
            push();
            break;
        case 2:
            pop();
            break;
        case 3:
            display();
            break;
        case 4:
            return;
        }
        fflush (stdin);
        printf("Do you want to continue(Type 0 or 1)?\n");
        scanf("%d", &option);
    }
}
```

## And that's about it!

# Thank you for reading.
