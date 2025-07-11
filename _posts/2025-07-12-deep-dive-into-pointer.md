---
layout: post
title: "Deep Dive into Pointers Unleashing the Power of C"
description: "In this blog, we will explore the power of pointers in C"
date: 2025-07-10 3:00:00
author: Vignesh
categories: [Pointer,C]
tags: [Pointer]
image: assets/Thumnails/pointers.png
---


# Deep Dive into Pointers: Unleashing the Power of C

## Introduction

Hey Guys, Welcome to this blog!

In this blog we're going to learn about the **basics of a pointer** and how to **use a pointer in C**. We’ll also look at the history behind pointers, why we use them, and some **common pitfalls** to avoid.

So let's dive deep into the world of pointers!

## What is Pointer?

Pointer is nothing but a variable that stores the memory address of another variable or object Woooo! What does that mean?
Before diving into the pointer, we need to understand some basics about how memory works and represented in under the hood.

Before moving on to that, what is Computer Science?
Computer Science is all about the study information,computation, and how information is represented, right?
Information represented in the memory as the binary form

```c
for eg:
int x = 10;
```

How would this been respresented in the memory?

```c
000001010
```

Because computers understand only 0s and 1s, everything you see on the computer screen is just information represented in the binary form in memory.
In modern computers, memory(RAM) is divided into the bytes, which are capable of the storing eight bits of information. Each bytes has the unique address.

For example, if there are x numbers of memory in the computer, the range would be from 0 to x - 1

## How they came up with this idea?

So, in this case, every byte has a unique address. This mean if we get the address, we can  manipulate the data directly.  

This idea is actually present in the CPU registers. if you take a look at the registers, there's something called EIP (Extended instruction pointer) in 32-bit system or RIP (Register instruction pointer) in 64-bit system.These works exactly like pointers they take the address of current instruction and process in the CPU. So, you could think of RIP as a pointer.

<center>

<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_1.png" width="600" height="300" alt="Pointer in C Programming"/>

</center>

This is where the pointer is capable of storing the memory address of the another variable or object.

## How to use a Pointer in C ?

## Declaring a pointer

To declare a pointer in C, we need to specify the datatype, followed by an asterisk, and then the name of the variable.

```c

datatype *variablename

eg:
    int *ptr;  // declaring an uninitialized pointer 
```

## Address of operator

Next, we need to point to an object Here, the object refers to a region of memory or the address of another variable.
To get the address of another variable we need use the & operator.

```c
#include <stdio.h>
int main(void)
{
    int a = 12;
    int *ptr = &a; // Getting the address of the another variable or object
}
```

- **Note**
    An integer pointer can only points to an integer, not a float,char etc. if it points to a different size of integer, it could lead to loss of value. To avoid this, you need to use type casting or pointer type casting eg: (int *)
    int *ptr; // points to an integer object
    float *ptr; // points to a float object
    char *ptr; // points to a char object

## Indirection/dereference operator

Indirection or dereferencing goes to the address and fetches the value.

```c
#include <stdio.h>
int main(void)
{
    int a = 12;
    int *ptr = &a;
    printf("the value ptr %d\n",*ptr); // dereferencing the value // print 12 
}
```

Output:

<center>

<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/pointer_2.png" alt="Pointer in c"/>

</center>

**Note: Don't confuse with reference and dereference, as both may look similar at times.**

By using the dereference operator, we can also reassign the value. 
```c
#include <stdio.h>
int main(void)
{
    int a = 12;
    int *ptr = &a; // assign the value to the pointer 
    *ptr = 32; // dereference and assign the value 
}
```
Now both ```a``` and ```ptr``` has the same value. So, if change for one, it will affect the other because you are directly manipulating the memory and here the a points to the ptr. 
<center>

<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_3.png" alt="Pointer in C"/>

</center>

## Pointer Assignment
C allows us to assign one pointer to another pointer which copies the value(address) stored in the original pointer.

```c
#include <stdio.h>
int main(void)
{
    int i = 12;
    int *ptr;
    int *htr;

    ptr = &i;
    htr = ptr;

    printf("The address of ptr %p\n",ptr);
    printf("The address of htr %p\n",htr);

}
```
In this code, if you take a look, both ```ptr``` and ```htr``` have same address that of ```i``` which they point to.

<center>

<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_4.png" alt="Pointer in C"/>

</center>

Now both point to same address if change any one of the value it affect 2 object i,ptr.

```c
#include <stdio.h>
int main(void)
{
    int i = 12;
    int *ptr;
    int *htr;

    ptr = &i;
    *htr = 100;

    printf("The value of i %d\n",i);
    printf("The value of ptr %d\n",*ptr);
}
```
Output:

<center>

<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_5.png" alt="Pointer in C"/>

</center>

## Why do we use a pointer?
So far, we've seen how to use a pointer. now we're going to see why we use a pointer and where to use the pointer.

### Passing a pointer as an argument 
In general, if you take a look at a function parameter, it usually takes the value as a deep copy or just makes a copy of the value and stores it in a local variable,like ```a``` in this case Because it protects the value from been changed. 
```c
#include <stdio.h>

void modify(int a)
{
    a = 32;
}

int main(void)
{
    int a = 12;
    modify(a);
    printf("the value of a: %d\n",a);
    return 0;
}
```
In the above code, will the value of a change if we print it after the function call?

Simply, no.

Output:

<center>

<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_6.png" alt="Pointer in C"/>

</center>

But if we use a pointer, it directly manipulates the memory,allowing us to change the value without needing to return it.The modified value is stored in the same variable.

```c
#include <stdio.h>

void modify(int *a) // accepting the parameter as pointer
{
    *a = 32; // dereferencing the variable 
}

int main(void)
{
    int a = 12;
    modify(&a); // passing the address 
    printf("the value of a: %d\n",a);
    return 0;
}
```

**Note:** Don't confuse with referencing and dereferencing both look same in this case.

Output: 


<center>

<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_7.png" alt="Pointer in C"/>

</center>

## Pointer for Problem-solving (Math Problem)

Let's use pointer for problem solving, specifically math problem.

We are given an array, and your task is to find the maximum and minimum values using a void function.

```c
Enter a 10 number: 82 42 102 94 23 11 50 31 49 10
Largest: 102
Smallest: 10
```

```c
#include <stdio.h>
#include <limits.h>
#define MAX 10

void findMaxandMin(int arr[], int n,int *max, int *min)
{

    *max = INT_MIN;
    *min = INT_MAX;
    for(int i = 0; i < MAX; i++)
    {
        if(arr[i] > *max)
        {
            *max = arr[i];
        }
        else
        {
            *min = arr[i];
        }
    }

}

int main(void)
{
    int largest,smallest;
    int arr[MAX];
    printf("Enter %d numbers: ",MAX);
    for(int i = 0; i < MAX; i++)
    {
        scanf("%d",&arr[i]);   
    }
    findMaxandMin(arr,MAX,&largest,&smallest);
    printf("Largest %d\n",largest);
    printf("Smallest %d\n",smallest);
    return 0;
}
```

Output: 


<center>

<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_8.png" alt="Pointer in C"/>

</center>

So yeah, that's the power of pointers in C! 
And this is just the basics we only scratched the surface. There lot more cool stuff coming up with the next blog, like ```array vs pointer``` etc...

If you liked this post, feel free to subscribe to my blog for more content like this and if found it would be helpful, consider donating to support my work. Every bit helps and keeps me motivated 

Just Buy me some coffee ☕!

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="tamilcode" data-color="#5F7FFF" data-emoji="☕"  data-font="Lato" data-text="Buy me a coffee" data-outline-color="#000000" data-font-color="#ffffff" data-coffee-color="#FFDD00" ></script>
</center>
<br/>

Thanks for reading and happy coding..............................💻