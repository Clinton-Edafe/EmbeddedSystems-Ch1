# Pointing to Data with a Datatype in C

## Overview
In C, pointers can be associated with specific **data types**. A pointer stores the **memory address** of a variable, and its type determines the size and type of the data it points to.

## Table of Contents
1. [What is a Pointer to a Datatype?](#what-is-a-pointer-to-a-datatype)
2. [Syntax and Declaration](#syntax-and-declaration)
3. [Example Code](#example-code)
4. [Memory Layout](#memory-layout)
5. [Pointer Arithmetic](#pointer-arithmetic)
6. [Use Cases](#use-cases)
7. [Mermaid Diagrams](#mermaid-diagrams)

## What is a Pointer to a Datatype?
A pointer to a datatype is a pointer that **specifically** stores the memory address of a variable of a certain type. The pointer type determines how the data is interpreted and accessed.

### Example
```c
int *ptr;   // Pointer to an integer
float *fptr; // Pointer to a float
char *cptr;  // Pointer to a character
```

## Syntax and Declaration
```c
datatype *pointer_name;
```
- `datatype` specifies the type (e.g., `int`, `float`, `char`).
- `pointer_name` is the pointer variable storing the address.

### Example:
```c
int var = 10;
int *ptr = &var; // Pointer to an integer
```

## Example Code
```c
#include <stdio.h>

int main() {
    int num = 25;
    float decimal = 3.14;
    char letter = 'A';

    int *ptr_num = &num;
    float *ptr_float = &decimal;
    char *ptr_char = &letter;

    printf("Value of num using pointer: %d\n", *ptr_num);
    printf("Value of decimal using pointer: %.2f\n", *ptr_float);
    printf("Value of letter using pointer: %c\n", *ptr_char);

    return 0;
}
```
### Output:
```
Value of num using pointer: 25
Value of decimal using pointer: 3.14
Value of letter using pointer: A
```

## Memory Layout
Pointers store addresses and refer to the actual values.

```mermaid
graph TD
    A[25 (int)] --> P1[Pointer to int]
    B[3.14 (float)] --> P2[Pointer to float]
    C[A (char)] --> P3[Pointer to char]
```

## Pointer Arithmetic
Pointer arithmetic depends on the datatype:
- `ptr + 1` moves by the **size** of the datatype.

```c
int arr[3] = {10, 20, 30};
int *ptr = arr;
printf("First value: %d\n", *ptr);
ptr++;
printf("Second value: %d\n", *ptr);
```

### Output:
```
First value: 10
Second value: 20
```

## Use Cases
### 1. Dynamic Memory Allocation
Used in `malloc()` and `calloc()` for dynamic memory management.

### 2. Data Structures
Pointers to data types are used in **linked lists, trees, and stacks**.

### 3. Function Arguments
Pointers allow functions to modify original values.

#### Example:
```c
void modify(int *ptr) {
    *ptr = 50;
}

int main() {
    int x = 10;
    modify(&x);
    printf("Modified value: %d\n", x);
    return 0;
}
```
### Output:
```
Modified value: 50
```

## Mermaid Diagrams

### Pointer to Datatype Representation
```mermaid
graph TD
    A[25 (int)] --> P1[Pointer to int]
    B[3.14 (float)] --> P2[Pointer to float]
    C[A (char)] --> P3[Pointer to char]
```

## Conclusion
- **Pointers** store addresses and refer to specific **data types**.
- They enable **efficient memory management** and **function argument passing**.
- **Pointer arithmetic** follows data type size rules.
