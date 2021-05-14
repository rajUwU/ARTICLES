# COMPLETE GUIDE TO LINKED LIST

> The following article will completely encapsulate the core concepts around Linked List. Some implementation's been performed as well on a C compiler.

Before we jump right into Linked List we must first need to understand some fundamental concepts:

# A NODE

Each node of a linked list can store a data called an element. These nodes are popularly also known as links.

![COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_02.46.19.png](COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_02.46.19.png)

The data part consists of stored information and the link part contains the address to the next Node. It essentially points to the location of the next link in the memory. This section of the node is also called as "Next" as it points to the next link.

# A Linked List:

A linked list is a linear data structure, in which the elements are not stored at contiguous memory locations. Each link contains a connection to another link. Linked list is the second most-used data structure after array.

![COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_03.04.26.png](COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_03.04.26.png)

Here is a linked list with 4 nodes all containing data A, B, C and D respectively. As you can see the right adjacent part of the node directs to the next node. Essentially that part stores the address of the next node.

You have probably observed that the last node is directed to NULL. It is basically a garbage value in the memory which facilitates that the list as ended.

This will be easily understood when we perform this in a program. The functions / operations will be performing are as follows:

- [Creating a Linked List]()
- [Traversing a Linked List / Displaying a linked list]()
- Inserting a link at the first location
- Inserting a link at the last location
- Deleting the link at the first location
- Deleting the link at the last location
- Sorting a Linked List
- Finding a link with a given key
- Delete a link with given key
- Reversing a linked list

Whoa!! that's a lot functionality, this might seem overwhelming but as begin with one operation the other become fairly easy!

Not to mention, There are 3 types of Linked List :-   

1. **Simple** **Linked** **List** - Item navigation is forward only.
2. **Doubly** **Linked** **List** - Items can be navigated forward and backward.
3. **Circular** **Linked** **List** - Last item contains link of the first element as next and the first element has a link to the last element as previous.

First, we will cover Simple Linked List or also known as Singly Linked List.

# Creating A Linked List

You have seen a Node diagrammatically, lets see how its done in C:

```c
struct Node {
    int data;
    struct Node* next;
};
```

We introduce a structure with data types : an `int data` and a struct `Node* next`.  As you can our node structure has a structure called `next` which is the same structure it resides in. This type of structure is a recursive one. Its content calls it self. 

This is how we achieve a link between multiple nodes:

Let us say we have a node with some data and a next link that has address `100` (an arbitrary address) that points to another node.

![COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_03.49.20.png](COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_03.49.20.png)

In our node, the we reference the node instead of calling its value, ensuring we are storing the address of the next node.

When a node is pointing to null our list ends.

Now that our node is prepared lets see how we start to add some items:

In our main() function: 

```c
void main()
{
    struct Node* head = NULL;
    struct Node* second = NULL;
    struct Node* third = NULL;
  
    head = (struct Node*)malloc(sizeof(struct Node));
    second = (struct Node*)malloc(sizeof(struct Node));
    third = (struct Node*)malloc(sizeof(struct Node));
}
```

In the above code we manually create 3 nodes head, second and third respectively. If you look carefully they all point to NULL meaning they are not linked yet.

A empty list is represented by setting the head to null.

As of now we created 3 nodes but we haven't allocated them into memory, the next 3 lines of code does exactly that. The malloc() function dynamically allocates all 3 nodes some memory.

A pointer to a linked list contains the address of the linked list... "head", "second" and third acts as as pointers to the nodes.

![COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_04.21.35.png](COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_04.21.35.png)

It is time to assign some data to these nodes, in the main() func: 

```c
void main() {
...
		head->data = 1; 
    head->next = second; 
}
```

We use an Arrow Operator to access elements in Structures and Unions. It is used with a pointer variable pointing to a structure or a union.

**(pointer_name)->(variable_name)**

We have successfully assigned `1` as data to fist node and linked it with the second node:-

![COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_04.34.13.png](COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_04.34.13.png)

Similarly,

```c
void main() {
...
	second->data = 2;
	second->next = third;

	third->data = 3;
	third->next = NULL;
}
```

And voila!!, Our linked list is ready!

![COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_04.41.07.png](COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-12_04.41.07.png)

Here is the complete code:

```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node* next;
};

void main() {
    struct Node* head = NULL;
    struct Node* second = NULL;
    struct Node* third = NULL;
  
    head = (struct Node*)malloc(sizeof(struct Node));
    second = (struct Node*)malloc(sizeof(struct Node));
    third = (struct Node*)malloc(sizeof(struct Node));

		head->data = 1;
    head->next = second;

		second->data = 2;
		second->next = third;

		third->data = 3;
    third->next = NULL;
}
```

# Traversing a Linked List

We can use the linked list so far and print it to our console, For this we will be creating a separate function 'display'.

Remember the 3 nodes (head, second, third) we declared inside the main function? We will be taking it out of the scope of the function to make them global and accessible to other functions in out program.

Just below the struct node like shown :-

```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *next;
};

struct Node *head = NULL;
struct Node *second = NULL;
struct Node *third = NULL;
```

Now below the above snippet we can introduce a new function to display out list!

```c
void display() {
    struct Node *ptr = head;
    printf("\n[");

    while (ptr != NULL)
    {
        printf(" %d ", ptr->data);
        ptr = ptr->next;
    }

    printf(" ]\n");
}
```

This might seem like a lot but we will breaking the code done line by line for you!

## struct Node *ptr = head;

We define a pointer which is pointing at the head of our linked list :-

![COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-14_12.06.52.png](COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-14_12.06.52.png)

Through this ptr we will be accessing the data inside the nodes.

## while (ptr != NULL)

As we know the last node in a linked list has its link pointing to NULL, So when we traverse through the list we need to ensure that when we reach the last node the traversing ends.

The while loop will keep looping until it finds a link pointing at NULL value.

## printf(" %d ", ptr->data);
        ptr = ptr->next;

We print the data of the current node the pointer is pointing at, then in the next line you can see we change the ptr's value to the next value of the current node its pointing at. Meaning our pointer is now pointing at the second node in our linked list.

![COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-14_12.14.04.png](COMPLETE%20GUIDE%20TO%20LINKED%20LIST%20b699a85487ea41b8902fbcd66dcf13df/Screenshot_from_2021-05-14_12.14.04.png)

Similarly it will do for the third node and finishes when it finds NULL.

The output would be:

```c
[ 1  2  3  ]
```

The printf() functions are added for formatting the list appropriately

And here is the full code:-

```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *next;
};

struct Node *head = NULL;
struct Node *second = NULL;
struct Node *third = NULL;

void display() {
    struct Node *ptr = head;
    printf("\n[");

    while (ptr != NULL)
    {
        printf(" %d ", ptr->data);
        ptr = ptr->next;
    }

    printf(" ]\n");
}

void main() {

    head = (struct Node *)malloc(sizeof(struct Node));
    second = (struct Node *)malloc(sizeof(struct Node));
    third = (struct Node *)malloc(sizeof(struct Node));

    head->data = 1;
    head->next = second;

    second->data = 2;
    second->next = third;

    third->data = 3;
    third->next = NULL;

    display();
}
```
