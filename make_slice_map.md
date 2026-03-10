# Memory Allocation with `make()`: Slice vs Map

This document explains the internal behavior and memory allocation differences when using the `make()` function for **slices** and **maps** in Go.

---

## 1️⃣ Slice Example

```go
slice := make([]int, 3, 5)
```

### Syntax
```go
make([]Type, length, capacity)
```

So here:
- **Type** → `int`
- **Length** → `3`
- **Capacity** → `5`

### What Happens Internally
Go allocates an array of size 5 in memory:
```text
[0 0 0 0 0]
```
But the slice initially uses only the first 3 elements:
- **Length** = 3
- **Capacity** = 5

**Memory visualization:**
```text
Underlying Array
[0 0 0 0 0]
 ↑ ↑ ↑
 slice length
```

### Code Example
```go
package main

import "fmt"

func main() {
	slice := make([]int, 3, 5)

	fmt.Println("Slice:", slice)
	fmt.Println("Length:", len(slice))
	fmt.Println("Capacity:", cap(slice))
}
```

**Output:**
```text
Slice: [0 0 0]
Length: 3
Capacity: 5
```

### What Happens When You Append
```go
slice = append(slice, 10)
slice = append(slice, 20)
```
Now the slice becomes:
```text
[0 0 0 10 20]
```
- **Length** = 5
- **Capacity** = 5
*(Still the same underlying array.)*

### If You Append One More
```go
slice = append(slice, 30)
```
Now capacity is exceeded. Go will:
1. Allocate a **new bigger array**
2. Copy existing elements
3. Append the new element

**Example new array:**
```text
[0 0 0 10 20 30]
```
Capacity usually **doubles**.

### Slice Internal Structure
A slice is actually a descriptor. Internally it looks like:
```go
type slice struct {
    pointer  // → underlying array
    length
    capacity
}
```

**Example:**
```text
pointer → [0 0 0 0 0]
length  → 3
cap     → 5
```

---

## 2️⃣ Map Example

```go
mapHu := make(map[string]int, 100)
```

### Syntax
```go
make(map[keyType]valueType, capacityHint)
```

So here:
- **Key type** → `string`
- **Value type** → `int`
- **Capacity** → `100` (hint)

### Important Interview Point
The `100` here is **NOT the map size**.

```go
fmt.Println(len(mapHu))
```
**Output:**
```text
0
```
Because no elements have been inserted yet.

### What `100` Actually Means
It is only a **capacity hint** to the Go runtime.
It tells Go: *"I expect around 100 elements."*
So Go can allocate buckets efficiently and reduce map resizing overhead.

### Map Internals (Simplified)
Maps are implemented using hash tables.

**Concept:**
`hash(key) → bucket → value`

**Structure example:**
```text
Map
 |
 +--- Bucket
 |      key:value
 |
 +--- Bucket
 |      key:value
```
Each bucket stores multiple key-value pairs.

### Example Code
```go
package main

import "fmt"

func main() {
	mapHu := make(map[string]int, 100)

	mapHu["A"] = 10
	mapHu["B"] = 20

	fmt.Println(mapHu)
	fmt.Println("Length:", len(mapHu))
}
```

**Output:**
```text
map[A:10 B:20]
Length: 2
```

---

## Key Differences in `make()` Usage

| Feature | Slice | Map |
| :--- | :--- | :--- |
| **make parameter** | length, capacity | capacity hint |
| **Memory allocation** | contiguous array | hash buckets |
| **Initial elements** | zero values created | empty |
| **`len()`** | equals provided length | 0 |

---

## Interview Trick Questions

### Question 1
What is the output?
```go
slice := make([]int, 3, 5)
fmt.Println(slice)
```
**Answer:** `[0 0 0]`
Because length = 3, so 3 zero values are initialized.

### Question 2
What is the output?
```go
mapHu := make(map[string]int, 100)
fmt.Println(len(mapHu))
```
**Answer:** `0`
Because capacity hint ≠ map size.

---

## Quick Memory Comparison

### Slice
```text
slice header
   |
   v
[0 0 0 0 0]
```

### Map
```text
map header
   |
   v
hash buckets
```

---

## ✅ Summary

#### `make([]int, 3, 5)`
- length = `3`
- capacity = `5`
- `3` elements initialized (zero values)

#### `make(map[string]int, 100)`
- capacity hint = `100`
- length = `0`
- no elements yet
