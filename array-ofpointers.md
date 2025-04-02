# Array of Pointers in C

## Overview
An **Array of Pointers** in C is an array that stores memory addresses instead of actual values. Each element in the array is a pointer that can point to different memory locations, enabling efficient data management.

## Table of Contents
1. [What is an Array of Pointers?](#what-is-an-array-of-pointers)
2. [Syntax and Declaration](#syntax-and-declaration)
3. [Example Code](#example-code)
4. [Memory Layout](#memory-layout)
5. [Accessing Elements](#accessing-elements)
6. [Use Cases](#use-cases)
7. [Mermaid Diagrams](#mermaid-diagrams)

## What is an Array of Pointers?
An **Array of Pointers** is a collection of pointers, where each pointer stores the address of a variable.

### Syntax
```c
int *arr[5];
```
- `arr` is an array of 5 pointers to integers.

### Example
```c
int a = 10, b = 20, c = 30;
int *arr[3] = {&a, &b, &c};
```
- `arr[0]` stores the address of `a`.
- `arr[1]` stores the address of `b`.
- `arr[2]` stores the address of `c`.

## Syntax and Declaration
```c
int *arr[5];  // Declaring an array of pointers
```

## Example Code
```c
#include <stdio.h>

int main() {
    int a = 10, b = 20, c = 30;
    int *arr[3] = {&a, &b, &c};

    printf("Value of a using arr[0]: %d\n", *arr[0]);
    printf("Value of b using arr[1]: %d\n", *arr[1]);
    printf("Value of c using arr[2]: %d\n", *arr[2]);

    return 0;
}
```
### Output:
```
Value of a using arr[0]: 10
Value of b using arr[1]: 20
Value of c using arr[2]: 30
```

## Memory Layout
Each array element stores an address.

```mermaid
graph TD
    A[10] --> P1[Pointer arr[0]]
    B[20] --> P2[Pointer arr[1]]
    C[30] --> P3[Pointer arr[2]]
```

## Accessing Elements
### Using Indexing
```c
printf("%d", *arr[1]); // Access value at arr[1]
```

### Using Loops
```c
for (int i = 0; i < 3; i++) {
    printf("Value: %d\n", *arr[i]);
}
```

## Use Cases
### 1. Managing Strings
```c
char *names[] = {"Alice", "Bob", "Charlie"};
printf("First Name: %s\n", names[0]);
```

### 2. Dynamic Memory Allocation
Used for dynamic arrays where each pointer points to a dynamically allocated memory block.

### 3. Function Pointers
Stores multiple function addresses in an array for flexible execution.

## Mermaid Diagrams

### Array of Pointers Representation
```mermaid
graph TD
    A[10] --> P1[arr[0]]
    B[20] --> P2[arr[1]]
    C[30] --> P3[arr[2]]
```

## Conclusion
- **Array of Pointers** enables efficient handling of multiple memory locations.
- Used in **string handling**, **dynamic memory**, and **function pointers**.
- **Dereferencing** retrieves stored values efficiently.
