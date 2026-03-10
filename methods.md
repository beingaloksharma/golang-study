# Methods in Go

A method is a function that is associated with a type (usually a struct).

### Syntax
```go
func (receiver Type) MethodName(parameters) returnType {
    // method body
}
```
The receiver connects the function to the type.

---

## 1️⃣ Example

```go
package main
import "fmt"

type User struct {
	Name string
	Age  int
}

func (u User) Greet() {
	fmt.Println("Hello,", u.Name)
}

func main() {
	u := User{Name: "Alok", Age: 30}
	u.Greet()
}
```

**Output:**
```text
Hello, Alok
```
Here:
- `User` → type
- `Greet()` → method
- `u` → receiver

---

## 2️⃣ Method Receivers

A receiver specifies which type the method belongs to.

```go
func (u User) Greet() {
	fmt.Println("Hello", u.Name)
}
```
Receiver types can be:
- Value receiver
- Pointer receiver

---

## 3️⃣ Value Receiver

The method receives a **copy** of the struct.

### Example
```go
package main
import "fmt"

type Counter struct {
	Value int
}

func (c Counter) Increment() {
	c.Value++
}

func main() {
	c := Counter{Value: 10}
	c.Increment()
	fmt.Println(c.Value)
}
```

**Output:** `10`

**Why?**
The method works on a copy, so the original struct remains completely unchanged.

---

## 4️⃣ Pointer Receiver

Pointer receivers allow modifying the original struct values.

### Example
```go
package main
import "fmt"

type Counter struct {
	Value int
}

func (c *Counter) Increment() {
	c.Value++
}

func main() {
	c := Counter{Value: 10}
	c.Increment()
	fmt.Println(c.Value)
}
```

**Output:** `11`

Here the method successfully modifies the original value.

---

## 5️⃣ Value vs Pointer Receiver

| Feature | Value Receiver | Pointer Receiver |
| :--- | :--- | :--- |
| **Data passed** | Copy | Pointer |
| **Modifies original** | ❌ No | ✅ Yes |
| **Memory usage** | More copying | Efficient |
| **Method receiver type** | `T` | `*T` |

### Interview Rule
Use pointer receiver when:
1. The struct is large
2. The method modifies data
3. You want to avoid copying memory

**Example:**
```go
func (u *User) UpdateAge() {}
```

---

## 6️⃣ Method Sets

Method sets determine which methods belong to a type. Understanding this is very important for interfaces.

### Method Set Rules

**For type `T`**
Method set includes:
- methods with receiver `(T)`

**For type `*T`**
Method set includes:
- methods with receiver `(T)`
- methods with receiver `(*T)`

### Example
```go
type User struct {}

func (u User) A() {}
func (u *User) B() {}
```
**Method sets:**
- `User` → `A()`
- `*User` → `A()`, `B()`

### Implicit Conversion Example
```go
package main
import "fmt"

type User struct{}

func (u User) SayHello() {
	fmt.Println("Hello")
}

func (u *User) SayBye() {
	fmt.Println("Bye")
}

func main() {
	u := User{}

	u.SayHello()
	u.SayBye()
}
```
Go automatically converts `u.SayBye()` to `(&u).SayBye()`.

---

## 7️⃣ Method Chaining

Method chaining allows calling multiple methods sequentially.

### Example pattern:
```go
obj.method1().method2().method3()
```

### Example
```go
package main
import "fmt"

type Builder struct {
	value int
}

func (b *Builder) Add(x int) *Builder {
	b.value += x
	return b
}

func (b *Builder) Multiply(x int) *Builder {
	b.value *= x
	return b
}

func (b *Builder) Result() int {
	return b.value
}

func main() {
	b := &Builder{}
	result := b.Add(5).Multiply(2).Add(10).Result()
	fmt.Println(result)
}
```

**Output:** `20`

**Explanation:**
- `Add(5)` → value = 5
- `Multiply(2)` → value = 10
- `Add(10)` → value = 20

### Why Method Chaining Works
Each method returns a `*Builder`, so the next method can continue calling off the returned instance.

---

## 8️⃣ Real-World Example

Method chaining is frequently used in:
- Database query builders
- HTTP frameworks
- Fluent APIs

**Example idea:**
```go
query.Select().Where().Limit()
```

---

## 9️⃣ Common Interview Questions

#### 1️⃣ Can methods be defined on non-struct types?
Yes.
```go
type MyInt int

func (m MyInt) Double() int {
	return int(m * 2)
}
```

#### 2️⃣ Can we define methods on built-in types?
No.
**Invalid:** `func (s string) MyFunc() {}`
**Allowed:** `type MyString string`

#### 3️⃣ What happens when calling a pointer method on a value?
Go automatically converts it.
Example: `u.SayBye()` becomes `(&u).SayBye()`

#### 4️⃣ What happens when calling a value method on a pointer?
It is allowed.
Example: `ptr.SayHello()`

---

> [!WARNING]
> ## 🔥 Advanced Interview Question
> **What will this print?**
> ```go
> package main
> import "fmt"
> 
> type Data struct {
> 	value int
> }
> 
> func (d Data) Set(v int) {
> 	d.value = v
> }
> 
> func main() {
> 	d := Data{value: 10}
> 	d.Set(50)
> 	fmt.Println(d.value)
> }
> ```
> 
> ### Answer
> `10`
> Because a value receiver works on a **copy** of the struct, not the original.

---

## ✅ Summary

Methods in Go provide behavior for types.

**Key concepts:**
- Methods and Structs
- Method receivers
- Pointer vs Value receivers
- Method sets
- Method chaining
