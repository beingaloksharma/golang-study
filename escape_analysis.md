# Escape Analysis in Go (Reading Guide)

## 1. What is Escape Analysis?

Escape Analysis is a compiler optimization technique used by the Go compiler to determine:

👉 Whether a variable should be allocated on the stack or the heap.

- **Stack allocation** → Faster, automatically freed when function exits.
- **Heap allocation** → Slower, managed by Go Garbage Collector.

If a variable escapes the function scope, Go moves it to the heap.

---

## 2. Stack vs Heap in Go

### Stack
- Managed automatically
- Very fast
- Local to function

**Example:**
```go
func add() int {
    x := 10
    y := 20
    return x + y
}
```
Here `x` and `y` stay on stack because they don’t escape.

### Heap
If Go detects a variable used outside the function, it moves it to the heap.

**Example:**
```go
func getPointer() *int {
    x := 10
    return &x
}
```
Here `x` escapes to heap because returning the pointer means `x` must live after function returns.

---

## 3. Why Escape Analysis Matters

1. **Performance**: Stack allocation takes nanoseconds; heap allocation requires garbage collection.
2. **Reduces GC pressure**: Less heap means less work for the garbage collector.
3. **Important for high-performance Go systems**: APIs, high-throughput services, and goroutine-heavy systems.

---

## 4. How to Check Escape Analysis

You can ask the Go compiler to print escape decisions:
```bash
go build -gcflags="-m"
```

**Example output:**
```text
./main.go:6:2: moved to heap: x
./main.go:10:9: &x escapes to heap
```

For more detail:
```bash
go build -gcflags="-m -m"
```

---

## 5. Common Escape Scenarios

1. **Returning pointer**
   ```go
   func foo() *int {
       x := 5
       return &x
   }
   ```
   ➡ escapes

2. **Interface conversion**
   ```go
   func foo() interface{} {
       x := 10
       return x
   }
   ```
   ➡ Often escapes.

3. **Closures**
   ```go
   func foo() func() int {
       x := 10
       return func() int {
           return x
       }
   }
   ```
   ➡ escapes.

4. **Goroutines**
   ```go
   func foo() {
       x := 10
       go func() {
           fmt.Println(x)
       }()
   }
   ```
   ➡ escapes because goroutine runs later.

---

## 6. Example With Escape Output

**Code:**
```go
package main

func foo() *int {
	x := 10
	return &x
}

func main() {
	_ = foo()
}
```

**Run:**
```bash
go build -gcflags="-m"
```

**Output:**
```text
./main.go:4:2: moved to heap: x
./main.go:5:9: &x escapes to heap
```

---

## 7. Important Interview Point

**Question:** Does returning a pointer always cause heap allocation?

**Answer:** No. The Go compiler may still keep it on the stack if it proves the pointer doesn't escape.

---

## 8. Best Practices

- **Avoid unnecessary heap allocations**
  - Prefer value types for small structs.
- **Avoid interface conversions in hot paths**
- **Prefer small structs by value**

---

## 9. Best Deep-Dive Resources
- Official Go docs
- "Go Escape Analysis Explained"
- "Stack vs Heap in Go"
- *Go Programming Language* (Donovan & Kernighan)

---

## 10. Escape Analysis + Goroutines

Since you're studying goroutines and concurrency, remember: **Variables captured by goroutines almost always escape.**

---

## 11. Quick Visual Summary
```text
Local variable
     │
     ▼
Does it escape function?
     │
 ┌───┴─────┐
 │         │
No        Yes
 │         │
Stack     Heap
Fast      GC managed
```
