---
layout: post
title: "Deep Dive into Pointers: Unleashing the Power of C"
description: "In this blog, we will explore the power of pointers in C."
date: 2025-07-10 3:00:00
author: Vignesh
categories: [Pointer, C]
tags: [Pointer]
image: assets/Thumnails/pointers.png
---

# Deep Dive into Pointers: Unleashing the Power of C

## Introduction

Hey Guys, Welcome to this blog!

In this blog, we're going to learn about the **basics of a pointer** and how to **use a pointer in C**. We’ll also look at the history behind pointers, why we use them, and some **common pitfalls** to avoid.

So let's dive deep into the world of pointers!

---

## What is a Pointer?

A pointer is a variable that stores the memory address of another variable or object.  
Woooo! What does that mean?

Before diving into pointers, we need to understand some basics about how memory works and is represented under the hood.

Before moving on to that, what is Computer Science?  
Computer Science is all about the study of information, computation, and how information is represented, right?  
Information is represented in memory as binary.

```c
// For example:
int x = 10;
```

How would this be represented in memory?

```c
000001010
```

Because computers understand only 0s and 1s, everything you see on the computer screen is just information represented in binary form in memory.

In modern computers, memory (RAM) is divided into bytes, which are capable of storing eight bits of information. Each byte has a unique address.

For example, if there are x bytes of memory in the computer, the range would be from 0 to x - 1.

---

## How did they come up with this idea?

So, in this case, every byte has a unique address. This means if we get the address, we can manipulate the data directly.

This idea is actually present in the CPU registers. If you take a look at the registers, there's something called EIP (Extended Instruction Pointer) in 32-bit systems or RIP (Register Instruction Pointer) in 64-bit systems. These work exactly like pointers: they take the address of the current instruction and process it in the CPU. So, you could think of RIP as a pointer.

<center>
<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_1.png" width="600" height="300" alt="Pointer in C Programming"/>
</center>

This is where the pointer is capable of storing the memory address of another variable or object.

---

## How to use a Pointer in C?

### Declaring a Pointer

To declare a pointer in C, specify the datatype, followed by an asterisk, and then the name of the variable.

```c
datatype *variablename;

// Example:
int *ptr;  // declaring an uninitialized pointer 
```

---

### Address-of Operator

Next, we need to point to an object. Here, the object refers to a region of memory or the address of another variable.  
To get the address of another variable, use the `&` operator.

```c
#include <stdio.h>
int main(void)
{
    int a = 12;
    int *ptr = &a; // Getting the address of another variable or object
}
```

**Note:**  
An integer pointer can only point to an integer, not a float or char. If it points to a different size of integer, it could lead to loss of value. To avoid this, use type casting or pointer type casting, e.g., `(int *)`.

```c
int *ptr;   // points to an integer object
float *ptr; // points to a float object
char *ptr;  // points to a char object
```

---

### Indirection/Dereference Operator

Indirection or dereferencing goes to the address and fetches the value.

```c
#include <stdio.h>
int main(void)
{
    int a = 12;
    int *ptr = &a;
    printf("the value ptr %d\n", *ptr); // dereferencing the value, prints 12 
}
```

Output:

<center>
<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/pointer_2.png" alt="Pointer in C"/>
</center>

**Note:** Don't confuse reference and dereference, as both may look similar at times.

By using the dereference operator, we can also reassign the value.

```c
#include <stdio.h>
int main(void)
{
    int a = 12;
    int *ptr = &a; // assign the value to the pointer 
    *ptr = 32;     // dereference and assign the value 
}
```

Now both `a` and `ptr` have the same value. If you change one, it will affect the other because you are directly manipulating the memory and `a` points to `ptr`.

<center>
<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_3.png" alt="Pointer in C"/>
</center>

---

## Pointer Assignment

C allows us to assign one pointer to another pointer, which copies the value (address) stored in the original pointer.

```c
#include <stdio.h>
int main(void)
{
    int i = 12;
    int *ptr;
    int *htr;

    ptr = &i;
    htr = ptr;

    printf("The address of ptr %p\n", ptr);
    printf("The address of htr %p\n", htr);
}
```

In this code, both `ptr` and `htr` have the same address as `i`, which they point to.

<center>
<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_4.png" alt="Pointer in C"/>
</center>

Now both point to the same address. If you change the value using either pointer, it affects both `i` and `ptr`.

```c
#include <stdio.h>
int main(void)
{
    int i = 12;
    int *ptr;
    int *htr;

    ptr = &i;
    htr = ptr;
    *htr = 100;

    printf("The value of i %d\n", i);
    printf("The value of ptr %d\n", *ptr);
}
```

Output:

<center>
<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_5.png" alt="Pointer in C"/>
</center>

---

## Why do we use a pointer?

So far, we've seen how to use a pointer. Now let's see why we use pointers and where to use them.

### Passing a Pointer as an Argument

In general, if you look at a function parameter, it usually takes the value as a deep copy or just makes a copy of the value and stores it in a local variable, like `a` in this case. This protects the value from being changed.

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
    printf("the value of a: %d\n", a);
    return 0;
}
```

Will the value of `a` change after the function call?  
Simply, no.

Output:

<center>
<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_6.png" alt="Pointer in C"/>
</center>

But if we use a pointer, it directly manipulates the memory, allowing us to change the value without needing to return it. The modified value is stored in the same variable.

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
    printf("the value of a: %d\n", a);
    return 0;
}
```

**Note:** Don't confuse referencing and dereferencing; both look similar in this case.

Output:

<center>
<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_7.png" alt="Pointer in C"/>
</center>

---

## Pointer for Problem-Solving (Math Problem)

Let's use pointers for problem-solving, specifically a math problem.

We are given an array, and your task is to find the maximum and minimum values using a void function.

```c
Enter 10 numbers: 82 42 102 94 23 11 50 31 49 10
Largest: 102
Smallest: 10
```

```c
#include <stdio.h>
#include <limits.h>
#define MAX 10

void findMaxandMin(int arr[], int n, int *max, int *min)
{
    *max = INT_MIN;
    *min = INT_MAX;
    for (int i = 0; i < n; i++)
    {
        if (arr[i] > *max)
        {
            *max = arr[i];
        }
        if (arr[i] < *min)
        {
            *min = arr[i];
        }
    }
}

int main(void)
{
    int largest, smallest;
    int arr[MAX];
    printf("Enter %d numbers: ", MAX);
    for (int i = 0; i < MAX; i++)
    {
        scanf("%d", &arr[i]);   
    }
    findMaxandMin(arr, MAX, &largest, &smallest);
    printf("Largest %d\n", largest);
    printf("Smallest %d\n", smallest);
    return 0;
}
```

Output:

<center>
<img src="https://raw.githubusercontent.com/0xevilox/0xevilox.github.io/refs/heads/main/_posts/_postImage/Pointer_8.png" alt="Pointer in C"/>
</center>

---

So yeah, that's the power of pointers in C!  
And this is just the basics—we only scratched the surface. There’s a lot more cool stuff coming up in the next blog, like `array vs pointer`, etc...

If you liked this post, feel free to subscribe to my blog for more content like this. If you found it helpful, consider donating to support my work. Every bit helps and keeps me motivated.

Just Buy me some coffee ☕!

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="tamilcode" data-color="#5F7FFF" data-emoji="☕"  data-font="Lato" data-text="Buy me a coffee" data-outline-color="#000000" data-font-color="#ffffff" data-coffee-color="#FFDD00" ></script>
</center>
<br/>

Thanks for reading and happy coding..............................💻