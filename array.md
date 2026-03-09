# Go Arrays – Complete Guide

An **Array** in Go is a fixed-size collection of elements of the same type stored in contiguous memory locations. Arrays are one of the fundamental data structures in Go and serve as the underlying storage for slices.

This document explains arrays in detail including **definition, internal structure, declaration methods, operations, memory behavior, and best practices**.

---

## Table of Contents

1. [What is an Array](#1-what-is-an-array)
2. [Key Characteristics](#2-key-characteristics)
3. [Declaring Arrays](#3-declaring-arrays)
4. [Array Initialization](#4-array-initialization)
5. [Accessing and Modifying Elements](#5-accessing-and-modifying-elements)
6. [Array Length](#6-array-length)
7. [Iterating Over Arrays](#7-iterating-over-arrays)
8. [Multidimensional Arrays](#8-multidimensional-arrays)
9. [Passing Arrays to Functions](#9-passing-arrays-to-functions)
10. [Arrays vs Slices](#10-arrays-vs-slices)
11. [Memory Layout](#11-memory-layout)
12. [Common Pitfalls](#12-common-pitfalls)
13. [Practical Examples](#13-practical-examples)
14. [Summary](#14-summary)
15. [Difference Between Array and Slice](#15-difference-between-array-and-slice-in-go)

---

## 1. What is an Array

An **array** is a collection of elements of the same type stored in contiguous memory locations.

The size of an array is **fixed at compile time** and cannot be changed.

**Example:**
```go
var numbers [3]int = [3]int{10, 20, 30}
```
Here:
- `numbers` is an array
- `3` is the size
- `int` is the element type

**Memory representation:**
```text
[10 20 30]
```

## 2. Key Characteristics

Arrays in Go have the following properties:
- **Fixed size**
- Elements stored in **contiguous memory**
- All elements must have the **same type**
- **Array size is part of the type**
- Arrays are **value types**

**Example:**
`[3]int` ≠ `[4]int`

These are considered completely different types.

## 3. Declaring Arrays

### Method 1: Standard Declaration
```go
var arr [5]int
```
Default zero values:
```text
[0 0 0 0 0]
```

### Method 2: Declaration with Initialization
```go
var arr [3]int = [3]int{1, 2, 3}
```

### Method 3: Short Declaration
```go
arr := [3]int{1, 2, 3}
```

### Method 4: Let Compiler Determine Size
```go
arr := [...]int{1, 2, 3, 4}
```
Compiler automatically sets size to 4.

## 4. Array Initialization

**Example:**
```go
arr := [5]int{1, 2, 3}
```
Remaining elements get zero values:
```text
[1 2 3 0 0]
```

You can also initialize specific indices:
```go
arr := [5]int{0: 10, 3: 40}
```
Result:
```text
[10 0 0 40 0]
```

## 5. Accessing and Modifying Elements

Access elements using an index (0-based).
```go
arr := [3]int{10, 20, 30}

fmt.Println(arr[0])
```
**Output:**
```
10
```

Modify element:
```go
arr[1] = 50
```
Now:
```text
[10 50 30]
```

## 6. Array Length

Use the `len()` function.
```go
arr := [4]int{1, 2, 3, 4}
fmt.Println(len(arr))
```
**Output:**
```
4
```

## 7. Iterating Over Arrays

### Using `for` loop
```go
for i := 0; i < len(arr); i++ {
    fmt.Println(arr[i])
}
```

### Using `range`
```go
for index, value := range arr {
    fmt.Println(index, value)
}
```

## 8. Multidimensional Arrays

Arrays can have multiple dimensions.

**Example:**
```go
matrix := [2][3]int{
    {1, 2, 3},
    {4, 5, 6},
}
```

**Memory:**
```text
[1 2 3]
[4 5 6]
```

Access element:
```go
fmt.Println(matrix[1][2])
```
**Output:**
```
6
```

## 9. Passing Arrays to Functions

Arrays are **value types** in Go. When passed to a function, the **entire array is copied**.

**Example (Pass by Value):**
```go
func update(a [3]int) {
    a[0] = 100
}

func main() {
    arr := [3]int{1, 2, 3}
    update(arr)
    fmt.Println(arr)
}
```
**Output:**
```
[1 2 3]
```
The original array is not modified.

**Using Pointer to Modify Array:**
```go
func update(a *[3]int) {
    a[0] = 100
}

func main() {
    arr := [3]int{1, 2, 3}
    update(&arr)
    fmt.Println(arr)
}
```
**Output:**
```
[100 2 3]
```

## 10. Arrays vs Slices

| Feature | Array | Slice |
| :--- | :--- | :--- |
| **Size** | Fixed | Dynamic |
| **Memory** | Direct storage | Reference to underlying array |
| **Passing** | Value copy | Reference-like (header copied, array shared) |
| **Usage** | Rare | Very common |

**Example array:**
```go
arr := [3]int{1, 2, 3}
```

**Example slice:**
```go
s := []int{1, 2, 3}
```

## 11. Memory Layout

Arrays store elements in contiguous memory locations.

**Example:**
```text
Index   Value   Memory
0       10      0x100
1       20      0x104
2       30      0x108
```

This layout makes arrays very fast for indexing operations.

## 12. Common Pitfalls

### Large Arrays Copy on Function Call
Passing large arrays copies the entire memory.

**Example (Inefficient):**
```go
func process(arr [100000]int)
```
This can be inefficient.

**Better approach (using pointer):**
```go
func process(arr *[100000]int)
```
Or use **slices**, which are just headers referencing an array.

## 13. Practical Examples

**Example Program:**
```go
package main

import "fmt"

func main() {
    arr := [5]int{1, 2, 3, 4, 5}

    for i := 0; i < len(arr); i++ {
        fmt.Println(arr[i])
    }
}
```

**Output:**
```
1
2
3
4
5
```

## 14. Summary

Important points about arrays:
- Arrays have **fixed size**.
- Array size is part of the **type**.
- Arrays are **value types**.
- Passing arrays to functions **copies** the entire array memory.
- Arrays store elements in **contiguous memory**.
- Arrays are **rarely used directly** in Go.
- **Slices** are built on top of arrays and are used more often.


## 15. Difference Between Array and Slice in Go

| Feature | Array | Slice |
|---|---|---|
| Definition | Fixed-size collection of elements | Dynamic-sized view over an array |
| Size | Size is fixed at compile time | Size can grow or shrink |
| Syntax | `[n]Type` | `[]Type` |
| Example | `var arr [3]int` | `var s []int` |
| Memory | Stores elements directly | References an underlying array |
| Length | Fixed | Dynamic |
| Capacity | Same as length | Can be greater than length |
| Resizing | Cannot be resized | Can grow using `append()` |
| Passing to Function | Passed by value (full copy) | Slice header copied but references same array |
| Performance | Faster for fixed-size data | Slight overhead due to dynamic behavior |
| Flexibility | Less flexible | More flexible |
| Usage | Rarely used directly | Widely used in Go programs |
| Built-in Functions | Only `len()` | `len()`, `cap()`, `append()`, `copy()` |
| Underlying Data | Contains actual elements | Contains pointer, length, and capacity |
| Memory Allocation | Fixed memory allocated | Memory can expand when needed |
| Common Use Case | Fixed datasets (e.g., fixed matrix size) | Dynamic collections like lists, buffers |