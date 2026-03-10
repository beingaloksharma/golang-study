# Understanding Structs in Go

A **struct** is a user-defined type that groups multiple fields into a single unit. Think of it as a record or object-like structure.

---

## 1️⃣ Struct Declaration

### Syntax
```go
type StructName struct {
	field1 Type
	field2 Type
}
```

### Example
```go
package main
import "fmt"

type User struct {
	Name string
	Age  int
}

func main() {
	var u User

	u.Name = "Alok"
	u.Age = 30

	fmt.Println(u)
}
```

### Output
```text
{Alok 30}
```

---

## 2️⃣ Struct Initialization

There are multiple ways to initialize structs.

### Method 1 — Field Assignment
```go
var u User
u.Name = "Alok"
u.Age = 30
```

### Method 2 — Ordered Initialization
```go
u := User{"Alok", 30}
```
⚠️ *Risky if the struct field order changes.*

### Method 3 — Named Fields (Best Practice)
```go
u := User{
	Name: "Alok",
	Age:  30,
}
```
*This is recommended in production code.*

---

## 3️⃣ Nested Structs

A struct can contain another struct.

### Example
```go
type Address struct {
	City  string
	State string
}

type User struct {
	Name    string
	Age     int
	Address Address
}
```

### Initialization
```go
u := User{
	Name: "Alok",
	Age:  30,
	Address: Address{
		City:  "Delhi",
		State: "India",
	},
}
```

### Accessing fields
```go
fmt.Println(u.Address.City)
```
**Output:** `Delhi`

---

## 4️⃣ Anonymous Nested Struct

Struct inside struct without defining a separate type.

```go
type User struct {
	Name string
	Contact struct {
		Email string
		Phone string
	}
}
```

### Initialization
```go
u := User{}
u.Name = "Alok"
u.Contact.Email = "alok@email.com"
```

---

## 5️⃣ Struct Tags

Struct tags provide metadata about struct fields.

### Syntax
```go
FieldName Type `tag:"value"`
```

### Example
```go
type User struct {
	Name string `info:"username"`
	Age  int    `info:"userage"`
}
```

Tags are commonly used for:
- JSON encoding
- Database mapping (e.g., ORMs)
- Validation libraries

---

## 6️⃣ JSON Tags

JSON tags control how structs are encoded/decoded to JSON using Go's `encoding/json` package.

### Example
```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func main() {
	u := User{
		Name: "Alok",
		Age:  30,
	}

	data, _ := json.Marshal(u)
	fmt.Println(string(data))
}
```
**Output:** `{"name":"Alok","age":30}`

### JSON Tag Options

**Rename Field**
```go
Name string `json:"username"`
```

**Ignore Field**
```go
Password string `json:"-"`
```

**Omit Empty Fields**
```go
Age int `json:"age,omitempty"`
```
If the value is the zero value, the field is omitted.

```go
type User struct {
	Name string `json:"name"`
	Age  int    `json:"age,omitempty"`
}
// Output if age = 0: {"name":"Alok"}
```

---

## 7️⃣ Struct Comparison

Structs can be compared using `==` only if all fields are comparable.

### Example
```go
type User struct {
	Name string
	Age  int
}

func main() {
	u1 := User{"Alok", 30}
	u2 := User{"Alok", 30}

	fmt.Println(u1 == u2)
}
```
**Output:** `true`

### When Struct Comparison Fails
If a struct contains a slice, map, or function, comparison is not allowed.

```go
type User struct {
	Name string
	Tags []string
}
```
Running `u1 == u2` results in a **Compile error:**
`invalid operation: struct containing slice cannot be compared`

---

## 8️⃣ Pointer to Struct

Common in real applications to avoid copying large payloads.

### Example
```go
u := &User{
	Name: "Alok",
	Age:  30,
}

fmt.Println(u.Name)
```
Go **automatically dereferences pointers** to structs.
`u.Name` is equivalent to `(*u).Name`.

---

## 9️⃣ Struct Memory Layout

### Example
```go
type User struct {
	Name string
	Age  int
}
```

### Memory layout
```text
+--------+--------+
| Name   | Age    |
+--------+--------+
```
Struct fields are stored **contiguously in memory**.

---

## 🔟 Common Struct Interview Questions

#### 1️⃣ What is the zero value of a struct?
```go
var u User
fmt.Println(u)
```
**Output:** `{"" 0}` (Zero values of all fields).

#### 2️⃣ What happens when a struct is passed to a function?
```go
func modify(u User) {
	u.Age = 40
}
```
The struct is **copied**.
Use a pointer to modify the original:
```go
func modify(u *User) {
	u.Age = 40
}
```

#### 3️⃣ What is struct embedding?
```go
type Address struct {
	City string
}

type User struct {
	Name string
	Address
}
```
Access directly: `u.City`

#### 4️⃣ Why are struct tags useful?
They allow libraries to control serialization behavior (JSON, XML, ORM, Validation frameworks).

---

> [!WARNING]
> ## 🧠 Senior-Level Struct Question
> **What will this print?**
> ```go
> type User struct {
> 	Name string
> }
> 
> func main() {
> 	u1 := User{"Alok"}
> 	u2 := u1
> 
> 	u2.Name = "Rahul"
> 
> 	fmt.Println(u1.Name)
> }
> ```
> 
> ### Answer
> `Alok`
> Because struct assignment completely **copies** the struct.

---

## ✅ Summary

Structs in Go provide:
- Custom data types
- Field grouping
- JSON mapping
- Memory-efficient structures

**Key concepts:**
- Struct declaration
- Initialization
- Nested structs
- Struct tags
- JSON serialization
- Struct comparison



---

## Struct Embedding and Composition

Since Go does not support classical inheritance, it uses:
- **Struct Embedding**
- **Composition**

These together provide behavior similar to inheritance in OOP languages.

---

## 1️⃣ What is Composition in Go?

Composition means a struct contains another struct as a field.

### Example
```go
package main
import "fmt"

type Engine struct {
	HorsePower int
}

type Car struct {
	Name   string
	Engine Engine
}

func main() {
	c := Car{
		Name: "BMW",
		Engine: Engine{
			HorsePower: 300,
		},
	}

	fmt.Println(c.Engine.HorsePower)
}
```

**Output:**
```text
300
```
Access requires explicitly going through the embedded struct: `c.Engine.HorsePower`

---

## 2️⃣ What is Struct Embedding?

Embedding means including a struct without giving it a field name.

### Example
```go
package main
import "fmt"

type Engine struct {
	HorsePower int
}

type Car struct {
	Name string
	Engine
}

func main() {
	c := Car{
		Name: "BMW",
		Engine: Engine{
			HorsePower: 300,
		},
	}

	fmt.Println(c.HorsePower)
}
```

**Output:**
```text
300
```
Notice we use `c.HorsePower` instead of `c.Engine.HorsePower`.
This is called **field promotion**.

---

## 3️⃣ How Embedding Works Internally

Even though we access: `c.HorsePower`

Internally, Go still stores it like this:
```text
Car
 ├── Name
 └── Engine
        └── HorsePower
```
The compiler automatically **promotes fields** to the parent struct level for convenience.

---

## 4️⃣ Method Promotion with Embedding

Embedding also promotes methods.

### Example
```go
package main
import "fmt"

type Engine struct {}

func (e Engine) Start() {
	fmt.Println("Engine started")
}

type Car struct {
	Engine
}

func main() {
	c := Car{}
	c.Start()
}
```

**Output:**
```text
Engine started
```
Even though `Car` does not define `Start()`, it has access to it via embedding.

---

## 5️⃣ Composition vs Embedding

| Feature | Composition | Embedding |
| :--- | :--- | :--- |
| **Field declaration** | Named | Anonymous |
| **Access** | `car.Engine.HorsePower` | `car.HorsePower` |
| **Method promotion** | No | Yes |
| **Readability** | Explicit | Cleaner |

### Example

**Composition**
```go
type Car struct {
	Engine Engine
}
```

**Embedding**
```go
type Car struct {
	Engine
}
```

---

## 6️⃣ Go Alternative to Inheritance

Languages like Java, C++, and Python use: `class Car extends Vehicle`
But Go uses embedding.

### Example:
```go
type Vehicle struct {
	Speed int
}

type Car struct {
	Vehicle
}
```
Now `Car` has `Vehicle` behavior.

---

## 7️⃣ Method Overriding (Shadowing)

If a child struct defines a method with the same name, it **overrides** (shadows) the embedded method.

### Example
```go
package main
import "fmt"

type Animal struct {}

func (a Animal) Speak() {
	fmt.Println("Animal sound")
}

type Dog struct {
	Animal
}

func (d Dog) Speak() {
	fmt.Println("Bark")
}

func main() {
	d := Dog{}
	d.Speak()
}
```

**Output:**
```text
Bark
```
The `Dog` method overrides the embedded method.

---

## 8️⃣ Calling Parent Method

You can still access the embedded (parent) method explicitly:

```go
d.Animal.Speak()
```
**Example output:** `Animal sound`

---

## 9️⃣ Multiple Embedding

Go allows embedding multiple structs at once.

### Example
```go
type Logger struct {}
type Metrics struct {}

type Service struct {
	Logger
	Metrics
}
```
`Service` now has behavior (fields and methods) from both embedded structs.

---

## 🔟 Real Production Example

Embedding is heavily used in Go frameworks.

**Example pattern:**
```text
HTTP Server
   ↓
Middleware
   ↓
Handler
```
Embedding allows method reuse and behavior extension without complex inheritance hierarchies.

---

## 🧠 Important Interview Questions

#### 1️⃣ Does Go support inheritance?
**Answer:** No. Go uses **composition + embedding** to achieve similar behavior.

#### 2️⃣ What is field promotion?
Accessing embedded struct fields directly.
Example: `car.HorsePower` instead of `car.Engine.HorsePower`.

#### 3️⃣ Can we embed pointer types?
Yes.
```go
type Car struct {
	*Engine
}
```

#### 4️⃣ Can we embed interfaces?
Yes.
```go
type Service struct {
	io.Reader
}
```

---

> [!WARNING]
> ## 🔥 Advanced Interview Question
> **What is the output?**
> ```go
> package main
> import "fmt"
> 
> type A struct {}
> 
> func (A) Show() {
> fmt.Println("A")
> }
> 
> type B struct {
> A
> }
> 
> func (B) Show() {
> fmt.Println("B")
> }
> 
> func main() {
> b := B{}
> b.Show()
> }
> ```
> 
> ### Answer
> `B`
> Because the child's method overrides (shadows) the embedded method.

---

## ✅ Key Takeaways
- Go has **no inheritance**.
- Use **composition and embedding**.
- Embedding promotes fields and methods automatically.
- Allows behavior reuse simply and effectively.
