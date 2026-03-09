# Go Slices – Complete Guide

A **Slice** in Go is a flexible and powerful data structure used to store a sequence of elements. It is one of the most commonly used data types in Go and provides a dynamic alternative to arrays.

This document explains slices in detail including **definition, internal structure, creation methods, operations, append behavior, memory model, and best practices**.

---

## Table of Contents

1. [What is a Slice](#1-what-is-a-slice)
2. [Slice vs Array](#2-slice-vs-array)
3. [Internal Structure of Slice](#3-internal-structure-of-slice)
4. [Creating Slices](#4-creating-slices)
5. [Slice Length and Capacity](#5-slice-length-and-capacity)
6. [Slice Operations](#6-slice-operations)
7. [How append() Works](#7-how-append-works)
8. [Slice Growth Algorithm](#8-slice-growth-algorithm)
9. [Slice Memory Behavior](#9-slice-memory-behavior)
10. [Passing Slices to Functions](#10-passing-slices-to-functions)
11. [Common Pitfalls](#11-common-pitfalls)
12. [Performance Best Practices](#12-performance-best-practices)
13. [Practical Examples](#13-practical-examples)
14. [Summary](#14-summary)
15. [Difference Between Array and Slice](#15-difference-between-array-and-slice-in-go)

---

## 1. What is a Slice

A **slice** is a dynamically sized view into the elements of an **array**.
Slices do not store data themselves. Instead, they reference an underlying array.

```go
numbers := []int{1, 2, 3}
```
Here:
- `numbers` is a slice.
- It references an underlying array `[1, 2, 3]`.

Slices are preferred over arrays because they are more flexible and easier to work with.

## 2. Slice vs Array

| Feature | Array | Slice |
| :--- | :--- | :--- |
| **Size** | Fixed | Dynamic |
| **Declaration** | `[3]int` | `[]int` |
| **Resize** | Not possible | Possible with `append` |
| **Usage** | Rare | Very common |

**Example array:**
```go
var arr [3]int = [3]int{1, 2, 3}
```

**Example slice:**
```go
s := []int{1, 2, 3}
```

## 3. Internal Structure of Slice

Internally, a slice contains three fields:
- **Pointer** → points to the first element of the underlying array that the slice can access.
- **Length** → number of elements present in the slice.
- **Capacity** → maximum size the slice can expand to before reallocation is needed.

**Conceptually:**
```go
type slice struct {
    pointer *array
    length  int
    capacity int
}
```

**Example Memory Layout:**
```text
Slice Header
+---------+------+------+
| pointer | len  | cap  |
+---------+------+------+
     |
     v
[10 20 30]
```

## 4. Creating Slices

### 4.1 Using Slice Literal
```go
s := []int{1, 2, 3}
```

### 4.2 Using make()
```go
s := make([]int, 3, 5)
```
**Meaning:**
- `length = 3`
- `capacity = 5`

**Underlying array:**
```text
[0 0 0 _ _]
```

### 4.3 Creating from an Array
```go
arr := [5]int{1, 2, 3, 4, 5}
s := arr[1:4]
```
**Result:**
```go
[2, 3, 4]
```

## 5. Slice Length and Capacity

Two built-in functions:
- `len(slice)`: Returns the length of the slice.
- `cap(slice)`: Returns the capacity of the slice.

**Example:**
```go
s := []int{10, 20, 30}

fmt.Println(len(s)) // Output: 3
fmt.Println(cap(s)) // Output: 3
```

## 6. Slice Operations

- **Access element:**
  ```go
  fmt.Println(s[0])
  ```
- **Modify element:**
  ```go
  s[1] = 50
  ```
- **Slice a slice:**
  ```go
  s := []int{1, 2, 3, 4, 5}
  sub := s[1:4] // Result: [2, 3, 4]
  ```

## 7. How append() Works

The `append()` function adds elements to a slice.

**Example:**
```go
s := []int{1, 2, 3}
s = append(s, 4) // Result: [1, 2, 3, 4]
```

Behavior depends on capacity:

### Case 1: Capacity Available
If `len = 3` and `cap = 5`:
```go
// Append 4:
// [1 2 3 4 _]
```
No new array is created.

### Case 2: Capacity Full
If `len = 3` and `cap = 3`:

Append triggers:
1. New array allocation.
2. Copy of existing elements.
3. Insertion of the new element.

## 8. Slice Growth Algorithm

Go runtime increases slice capacity automatically.

**Typical growth pattern:**
- 1 → 2
- 2 → 4
- 4 → 8
- 8 → 16
- 16 → 32

For large slices, growth slows down to roughly `1.25x` to reduce memory overhead.

## 9. Slice Memory Behavior

**Example:**
```go
data := []int{1, 2, 3, 4, 5}
sub := data[1:3]
```

**Memory layout:**
```text
data → [1 2 3 4 5]
sub  →   [2 3]
```
Both share the **same underlying array**. Changes to one affect the other:

```go
sub[0] = 100
// Now:
// data → [1 100 3 4 5]
```

## 10. Passing Slices to Functions

Slices are passed by value, but since they contain a pointer, they reference the **same array**.

**Example:**
```go
func update(s []int) {
    s[0] = 100
}

func main() {
    a := []int{1, 2, 3}
    update(a)
    fmt.Println(a)
}
```
**Output:**
```
[100 2 3]
```
*Because both slices point to the same array.*

## 11. Common Pitfalls

### 11.1 Hidden Memory Usage
**Example:**
```go
data := make([]byte, 1_000_000)
small := data[:10]
```
Even though `small` contains only 10 bytes, the entire 1MB array remains in memory because `small` retains a reference to it.

**Solution:**
```go
smallCopy := append([]byte(nil), small...)
```

### 11.2 Append inside Function
```go
func update(s []int) {
    s = append(s, 10)
}
```
The caller's slice may not reflect the new changes because `append` might allocate a **new array** internal to the function, modifying the local slice header but leaving the original unmodified.

## 12. Performance Best Practices

**Preallocate slices** whenever possible.

**Bad:**
```go
var s []int
for i := 0; i < 100000; i++ {
    s = append(s, i)
}
```

**Better:**
```go
s := make([]int, 0, 100000)
for i := 0; i < 100000; i++ {
    s = append(s, i)
}
```
This avoids multiple expensive memory allocations during the loop.

## 13. Practical Examples

**Example Program:**
```go
package main

import "fmt"

func main() {
    s := make([]int, 0, 5)

    for i := 1; i <= 5; i++ {
        s = append(s, i)
    }

    fmt.Println("Slice:", s)
    fmt.Println("Length:", len(s))
    fmt.Println("Capacity:", cap(s))
}
```

**Output:**
```
Slice: [1 2 3 4 5]
Length: 5
Capacity: 5
```

## 14. Summary

Important points about slices:
- A slice is a **dynamic view** of an array.
- Internally it contains a **pointer, length, and capacity**.
- `append()` may or may not allocate a new array.
- Multiple slices can **share** the same array.
- Improper slicing can cause **hidden memory retention**.
- **Preallocating capacity** drastically improves performance.

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