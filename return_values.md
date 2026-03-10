# Functions & Return Values

In Go, functions can return values in two ways:
1. Return Values (Unnamed)
2. Named Return Values

Let's understand the difference clearly.

## 1. Return Values (Unnamed Return Values)
In this approach, the function specifies the return types but not variable names.

### Example
```go
package main

import "fmt"

func Add(a int, b int) int {
	return a + b
}

func main() {
	result := Add(2, 3)
	fmt.Println(result)
}
```

### Key Points
- Only types are declared in the function signature.
- The function must explicitly return a value.
- No variable is automatically created for the return value.

### Example with multiple returns:
```go
func Divide(a int, b int) (int, int) {
	quotient := a / b
	remainder := a % b
	return quotient, remainder
}
```

## 2. Named Return Values
In this approach, the return values are declared with names in the function signature.
These variables are automatically created inside the function.

### Example
```go
package main

import "fmt"

func Add(a int, b int) (result int) {
	result = a + b
	return
}

func main() {
	fmt.Println(Add(2, 3))
}
```

### Key Points
- Return variables are declared in the function signature.
- They behave like local variables inside the function.
- You can use a naked return (`return`) without specifying values.

### Example with multiple named returns:
```go
func Divide(a int, b int) (quotient int, remainder int) {
	quotient = a / b
	remainder = a % b
	return
}
```

## Key Differences

| Feature | Return Values (Unnamed) | Named Return Values |
| --- | --- | --- |
| Return variables declared | No | Yes |
| Variables created automatically | No | Yes |
| Return statement | Must return values | Can use return without values |
| Readability | Usually clearer | Can be confusing if overused |
| Use case | Most common | Useful for documentation or short functions |

## Important Interview Point
In Go, named return values are initialized with **zero values**.

### Example:
```go
func Example() (a int, b string, c bool) {
	return
}
```

Returned values will be:
`0`, `""`, `false` because Go initializes them automatically.