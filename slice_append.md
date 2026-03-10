# How `append()` works with Slices in Go

This is one of the most misunderstood behaviors of slices in Go and is asked frequently in Go interviews.

The key idea:
`append()` may modify the original slice OR may create a new underlying array, depending on the capacity.

---

## 1️⃣ Slice Internals Refresher

A slice is not the array itself. It is a descriptor containing:
```go
type slice struct {
    pointer  // points to underlying array
    len
    cap
}
```

**Example memory layout:**
```text
slice header
   |
   v
[1 2 3 0 0]
 len=3 cap=5
```

---

## 2️⃣ Case 1 — `append()` modifies the original slice

When capacity is available, Go reuses the same underlying array.

### Example
```go
package main
import "fmt"

func main() {
	a := make([]int, 3, 5)

	a[0] = 1
	a[1] = 2
	a[2] = 3

	b := append(a, 4)

	fmt.Println("a:", a)
	fmt.Println("b:", b)
}
```

### Memory
```text
Underlying Array
[1 2 3 4 0]
 ↑ ↑ ↑
  a
```
Both slices point to the same array.

**Output:**
```text
a: [1 2 3]
b: [1 2 3 4]
```
But internally: `a` and `b` share the same array.

---

## 3️⃣ Case 2 — `append()` creates a new array

If capacity is full, Go must allocate a new array.

### Example
```go
package main
import "fmt"

func main() {
	a := []int{1, 2, 3}

	b := append(a, 4)

	fmt.Println("a:", a)
	fmt.Println("b:", b)
}
```

Here:
- `len = 3`
- `cap = 3`

Capacity is full. So Go:
1. Allocates a new array
2. Copies elements
3. Appends the new element

### Memory:
```text
a → [1 2 3]
b → [1 2 3 4]
```
Now they are different arrays.

---

## 4️⃣ Famous Interview Trap

### Code
```go
func modify(s []int) {
	s = append(s, 4)
	fmt.Println("inside:", s)
}

func main() {
	a := []int{1, 2, 3}
	modify(a)
	fmt.Println("outside:", a)
}
```

### Output:
```text
inside: [1 2 3 4]
outside: [1 2 3]
```

### Why?
Because `append()` created a new array inside the function. (Pass by value for slice header).

---

## 5️⃣ Another Example (Modifies Original)

```go
func modify(s []int) {
	s[0] = 100
}

func main() {
	a := []int{1, 2, 3}
	modify(a)
	fmt.Println(a)
}
```

### Output:
```text
[100 2 3]
```

### Why?
Because the slice shares the same array, and changing the element directly updates the underlying array.

---

## 6️⃣ Real Interview Brain-Teaser

### What is the output?
```go
func main() {
	a := make([]int, 3, 4)

	a[0] = 1
	a[1] = 2
	a[2] = 3

	b := append(a, 4)
	c := append(a, 5)

	fmt.Println(b)
	fmt.Println(c)
}
```

### Output
```text
[1 2 3 5]
[1 2 3 5]
```

### Why?
Because both `append` operations used the same underlying array whose capacity was 4.

**Memory:**
```text
[1 2 3 5]
```
The second append overwrites the same slot. This surprises many developers.

---

## 7️⃣ Safe Pattern

If you want independent slices, copy first.

```go
b := append([]int{}, a...)
```

Now append safely:
```go
b := append([]int{}, a...)
b = append(b, 4)
```

---

## 8️⃣ Slice Growth Rule

When capacity is exceeded, Go grows the slice.

**Typical growth pattern:**
`1 → 2 → 4 → 8 → 16 → 32`

For larger slices: `~1.25x growth`
*(This behavior is implemented in the Go runtime.)*

---

## 9️⃣ Visual Summary

| Scenario | Behavior | Result |
| --- | --- | --- |
| **When capacity is available** | `append()` | Same array reused |
| **When capacity is full** | `append()` | New array allocated |

---

> [!IMPORTANT]
> **🔑 Golden Rule (Interview Answer)**
> A slice is a view over an array. `append()` reuses the array if capacity allows, otherwise it allocates a new array.

---

## Senior Go Interview Question

### What will this print?
```go
func main() {
	a := []int{1, 2, 3}
	b := a
	b[0] = 100

	fmt.Println(a)
}
```

### Answer:
```text
[100 2 3]
```
Because slices share the same underlying array.
