# Interfaces

Interfaces are one of the most powerful features of Go. They enable abstraction, loose coupling, and polymorphism without traditional inheritance.

## Table of Contents
- [1️⃣ Interface Basics](#1️⃣-interface-basics)
- [2️⃣ Implicit Implementation](#2️⃣-implicit-implementation)
- [3️⃣ Empty Interface](#3️⃣-empty-interface)
- [4️⃣ Type Assertion](#4️⃣-type-assertion)
- [5️⃣ Type Switch](#5️⃣-type-switch)
- [6️⃣ Interface Embedding](#6️⃣-interface-embedding)
- [7️⃣ Interface Design Patterns](#7️⃣-interface-design-patterns)
- [8️⃣ Interface Internals](#8️⃣-interface-internals)
- [9️⃣ Nil Interface Trap](#9️⃣-nil-interface-trap)
- [🔟 Advanced Interview Questions](#🔟-advanced-interview-questions)
- [Examples](#examples)
- [✅ Summary](#✅-summary)

---

## 1️⃣ Interface Basics

An interface defines a set of method signatures.
Any type that implements those methods automatically satisfies the interface.

**Syntax**
```go
type InterfaceName interface {
	Method1()
	Method2()
}
```

**Example**
```go
package main
import "fmt"

type Animal interface {
	Speak()
}

type Dog struct{}

func (d Dog) Speak() {
	fmt.Println("Bark")
}

func main() {
	var a Animal
	a = Dog{}
	a.Speak()
}
```

**Output:**
```text
Bark
```

**Explanation:**
- `Animal` → interface
- `Dog` → concrete type
- `Dog` implements `Speak()`

[Back to Top](#table-of-contents)

---

## 2️⃣ Implicit Implementation

In Go there is no explicit `implements` keyword. A type automatically implements an interface if it has the required methods.

**Example:**
```go
type Shape interface {
	Area() float64
}

type Circle struct {
	radius float64
}

func (c Circle) Area() float64 {
	return 3.14 * c.radius * c.radius
}
```
`Circle` implicitly implements `Shape`. This enables loose coupling.

[Back to Top](#table-of-contents)

---

## 3️⃣ Empty Interface

The empty interface can hold any value:
```go
interface{}
```
Because it has zero methods, every type satisfies it.

**Example:**
```go
func PrintValue(v interface{}) {
	fmt.Println(v)
}

func main() {
	PrintValue(10)
	PrintValue("hello")
	PrintValue(true)
}
```

**Output:**
```text
10
hello
true
```

**Internals of Empty Interface:**
Internally it stores a `type` and a `value`. Conceptual structure:
```go
interface {
    type pointer
    data pointer
}
```

[Back to Top](#table-of-contents)

---

## 4️⃣ Type Assertion

Used to extract the concrete type from an interface.

**Syntax:** `value := i.(Type)`

**Example:**
```go
var i interface{} = "hello"
s := i.(string)
fmt.Println(s)
```

**Output:** `hello`

**Safe Type Assertion:** `s, ok := i.(string)`

**Example:**
```go
var i interface{} = 10
s, ok := i.(string)
fmt.Println(s, ok)
```

**Output:** `"" false`

[Back to Top](#table-of-contents)

---

## 5️⃣ Type Switch

A type switch checks multiple possible types.

**Example:**
```go
func checkType(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Println("Integer", v)
	case string:
		fmt.Println("String", v)
	case bool:
		fmt.Println("Boolean", v)
	default:
		fmt.Println("Unknown")
	}
}

// Usage:
// checkType(10)
// checkType("Go")
// checkType(true)
```

**Output:**
```text
Integer 10
String Go
Boolean true
```

[Back to Top](#table-of-contents)

---

## 6️⃣ Interface Embedding

Interfaces can embed other interfaces.

**Example:**
```go
type Reader interface {
	Read()
}

type Writer interface {
	Write()
}

type ReadWriter interface {
	Reader
	Writer
}
```
Now `ReadWriter` requires both `Read()` and `Write()`.

**Example Implementation:**
```go
type File struct{}

func (f File) Read()  {}
func (f File) Write() {}
```
`File` satisfies `Reader`, `Writer`, and `ReadWriter`.

[Back to Top](#table-of-contents)

---

## 7️⃣ Interface Design Patterns

Interfaces are widely used in Go architecture.

### Pattern 1 — Dependency Injection
Define behavior as interface.
```go
type Payment interface {
	Pay(amount int)
}

// Implementations:
type CreditCard struct{}
func (c CreditCard) Pay(amount int) {
	fmt.Println("Paid using card")
}

type PayPal struct{}
func (p PayPal) Pay(amount int) {
	fmt.Println("Paid using PayPal")
}

// Service:
func Checkout(p Payment) {
	p.Pay(100)
}

// Usage:
// Checkout(CreditCard{})
// Checkout(PayPal{})
```

### Pattern 2 — Strategy Pattern
Select algorithm dynamically.
```go
type SortStrategy interface {
	Sort([]int)
}
// Implementations: QuickSort, MergeSort, HeapSort
```

### Pattern 3 — Adapter Pattern
Convert incompatible interfaces. e.g. Legacy system interface -> Adapter connects them -> New system interface.

### Pattern 4 — Interface Segregation
Small interfaces are preferred.

**Bad design:**
```go
type Animal interface {
    Eat()
    Run()
    Fly()
}
```

**Better:**
```go
type Eater interface { Eat() }
type Runner interface { Run() }
type Flyer interface { Fly() }
```

[Back to Top](#table-of-contents)

---

## 8️⃣ Interface Internals

Interface value internally stores `(type, value)`.

**Example:**
```go
var i interface{} = 10
```

**Memory concept:**
```text
interface
 ├── type = int
 └── value = 10
```

[Back to Top](#table-of-contents)

---

## 9️⃣ Nil Interface Trap

Common interview question.

**Example 1:**
```go
var i interface{}
fmt.Println(i == nil)
```
**Output:** `true`

**Example 2:**
```go
var p *int = nil
var i interface{} = p
fmt.Println(i == nil)
```
**Output:** `false`

Because interface contains:
- `type = *int`
- `value = nil`

So interface itself is not nil.

[Back to Top](#table-of-contents)

---

## 🔟 Advanced Interview Questions

**1️⃣ Does Go use duck typing?**
Yes. If a type has required methods, it satisfies interface.

**2️⃣ What is difference between interface and struct?**

| Feature | Interface | Struct |
|---|---|---|
| Purpose | behavior | data |
| Contains | method signatures | fields |
| Implementation | implicit | explicit |

**3️⃣ Why are interfaces powerful in Go?**
They enable loose coupling, testability, mocking, and polymorphism.

**🔥 Senior-Level Interview Question**

What will this print?
```go
type Shape interface {
	Area() int
}

type Square struct{}

func (s Square) Area() int {
	return 10
}

func main() {
	var s Shape = Square{}
	fmt.Println(s.Area())
}
```
**Output:** `10`

Because `Square` implements `Area()`.

[Back to Top](#table-of-contents)

---

## Examples

### Geometry Shape Pattern
Demonstrates polymorphism using interfaces.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

// Geometry interface defines the contract
type Geometry interface {
	Area()
	Perimeter()
}

type Circle struct {
	Radius int
}

type Rectangle struct {
	Length, Width int
}

// Circle implementation
func (c Circle) Area() {
	fmt.Printf("Area of Circle: %d\n", 3*c.Radius*c.Radius)
}

func (c Circle) Perimeter() {
	fmt.Printf("Perimeter of Circle: %d\n", 2*3*c.Radius)
}

// Rectangle implementation
func (r Rectangle) Area() {
	fmt.Printf("Area of Rectangle: %d\n", r.Length*r.Width)
}

func (r Rectangle) Perimeter() {
	fmt.Printf("Perimeter of Rectangle: %d\n", 2*(r.Length+r.Width))
}

func main() {
	// We can store different types in a slice of the interface
	shapes := []Geometry{
		Circle{Radius: 5},
		Rectangle{Length: 10, Width: 5},
	}

	for _, shape := range shapes {
		shape.Area()
		shape.Perimeter()
	}
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Area of Circle: 75
  Perimeter of Circle: 30
  Area of Rectangle: 50
  Perimeter of Rectangle: 30
  ```
- **Complexity:**
  - **Time:** O(N) where N is number of shapes.
  - **Space:** O(1) auxiliary.

---

### Payment Method (Factory Pattern)
Demonstrates the Factory Pattern using interfaces to decouple object creation logic.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

type PaymentMethod interface {
	Pay(amount float32)
}

type CreditCard struct{}

func (c CreditCard) Pay(amount float32) {
	fmt.Printf("Paid %v via Credit Card\n", amount)
}

type PayPal struct{}

func (p PayPal) Pay(amount float32) {
	fmt.Printf("Paid %v via Paypal \n", amount)
}

func GetPaymentMethod(method string) PaymentMethod {
	if method == "paypal" {
		return PayPal{}
	}
	return CreditCard{}
}

func main() {
	payment := GetPaymentMethod("others")
	payment.Pay(100.0)
}
```
</details>

#### Analysis
- **Expected Output:** `Paid 100 via Credit Card` (Default case)
- **Complexity:**
  - **Time:** O(1)
  - **Space:** O(1)

[Back to Top](#table-of-contents)

---

## ✅ Summary

Interfaces provide abstraction and polymorphism in Go.

Key concepts:
- Interface basics
- Implicit implementation
- Empty interface
- Type assertion
- Type switch
- Interface embedding
- Design patterns

[Back to Top](#table-of-contents)