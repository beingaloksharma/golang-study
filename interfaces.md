## Interfaces

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

[Back to Top](#table-of-contents)

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
