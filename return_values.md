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

---

## Tricky Example 1: Named Return + Defer

### Question
What will be the output of the following code?
```go
package main

import "fmt"

func test() (result int) {
	defer func() {
		result++
	}()

	result = 5
	return
}

func main() {
	fmt.Println(test())
}
```

### Answer
Output: `6`

### Step-by-Step Explanation
1. **Function starts:** `result` is a named return variable, so Go automatically creates it. Initial value: `result = 0`.
2. **defer is registered:**
   ```go
   defer func() {
       result++
   }()
   ```
   This function will execute after `return` but before the function actually exits.
3. **Assign value:** `result = 5`. Now `result = 5`.
4. **return executes:** Because this is a named return, Go internally does `return result`. But before returning, deferred functions run.
5. **defer executes:** `result++`. Now `result = 6`.
6. **Final returned value:** `6`

> [!IMPORTANT]
> **Important Rule (Interview Gold ⭐)**
> In Go, deferred functions can modify named return values before the function exits.
> 
> **Execution order:**
> 1. Function executes
> 2. `return` statement triggered
> 3. `defer` functions execute
> 4. Final return value sent

---

## Tricky Example 2: Unnamed Return + Defer

### Code
```go
func test() int {
	result := 5

	defer func() {
		result++
	}()

	return result
}
```

### Output
`5`

### Why?
Because `result` is not a named return variable.
Go actually does:
```go
temp := result
run defer
return temp
```
So the returned value stays `5`.

### Difference Summary

| Case | Output |
| --- | --- |
| Named return + defer modify | Value changes |
| Unnamed return + defer modify | Value unchanged |

### Real World Use Case
This pattern is often used for:
- Error handling
- Logging
- Metrics
- Transaction rollback

**Example:**
```go
func process() (err error) {
	defer func() {
		if err != nil {
			fmt.Println("error occurred:", err)
		}
	}()

	return doSomething()
}
```

---

## 5 More Go Defer Trick Questions (Senior Interviews)
Here are 5 tricky defer questions commonly asked in interviews for Go developers. These test deep understanding of execution order, arguments, closures, and return behavior.

### 1. Defer Execution Order

#### Code
```go
package main

import "fmt"

func main() {
	defer fmt.Println("1")
	defer fmt.Println("2")
	defer fmt.Println("3")

	fmt.Println("Start")
}
```

#### Answer
```text
Start
3
2
1
```

#### Explanation
`defer` works like a stack (LIFO - Last In, First Out).

Execution order:
1. `defer 1`
2. `defer 2`
3. `defer 3`

Execution during return: `3 → 2 → 1`

---

### 2. Defer Arguments Evaluation

#### Code
```go
package main

import "fmt"

func main() {
	i := 1

	defer fmt.Println(i)

	i = 2
}
```

#### Output
`1`

#### Why?
Arguments of `defer` are evaluated immediately, when the `defer` statement is executed.
So Go internally stores:
```go
defer fmt.Println(1)
```

---

### 3. Closure + Defer Trap

#### Code
```go
package main

import "fmt"

func main() {
	i := 1

	defer func() {
		fmt.Println(i)
	}()

	i = 2
}
```

#### Output
`2`

#### Why?
The deferred function captures the variable `i`, not the value.
So when it runs, `i` is already `2`.

---

### 4. Defer Inside Loop (Classic Bug)

#### Code
```go
package main

import "fmt"

func main() {
	for i := 0; i < 3; i++ {
		defer fmt.Println(i)
	}
}
```

#### Output
```text
2
1
0
```

#### Why?
All `defer` calls execute after the function returns, and they follow LIFO order.
Stack: `defer 0`, `defer 1`, `defer 2`
Execution: `2`, `1`, `0`

---

### 5. Named Return + Defer Modification

#### Code
```go
package main

import "fmt"

func test() (x int) {
	defer func() {
		x++
	}()

	x = 10
	return
}

func main() {
	fmt.Println(test())
}
```

#### Output
`11`

#### Why?
Steps:
1. `x = 10`
2. `return` triggered
3. `defer` runs (`x++`)
4. `return x`

Final value: `11`

> [!TIP]
> **Senior Interview Insight**
> In Go, `defer` is heavily used for:
> - **Closing files:** `defer file.Close()`
> - **Unlocking mutex:** `mu.Lock()`, `defer mu.Unlock()`
> - **Database connection cleanup:** `defer rows.Close()`

---

## One More Super Tricky Question (Google-level)

### Predict the output:
```go
func main() {
	for i := 0; i < 3; i++ {
		defer func() {
			fmt.Println(i)
		}()
	}
}
```

#### Output
```text
3
3
3
```

#### Why This Happens
**1️⃣ Loop execution**
The loop runs: `i = 0`, `i = 1`, `i = 2`.
During each iteration, a deferred anonymous function is registered.
But the important thing is: *The function captures the variable `i`, not its value.*

**2️⃣ After loop finishes**
When the loop exits, `i = 3`.

**3️⃣ Now deferred functions execute**
All deferred functions run after the function returns.
Each function prints the same variable `i`, which is now `3`.

#### Visual Comparison

| Code | Captures | Output |
| --- | --- | --- |
| `defer fmt.Println(i)` | Value | `2 1 0` |
| `defer func(){ fmt.Println(i) }()` | Variable | `3 3 3` |

#### Fix for This Problem (Interview Follow-up)
To print expected values (2, 1, 0), correctly capture the loop variable:
```go
for i := 0; i < 3; i++ {
	i := i // create a new variable `i` for this loop iteration
	defer func() {
		fmt.Println(i)
	}()
}
```

**Output:**
```text
2
1
0
```

> [!WARNING]
> **Senior Go Interview Insight ⭐**
> This issue is called **closure capturing loop variable**, and it also appears frequently with:
> - `goroutines`
> - `defer`
> - `callbacks`
> 
> **Example bug:**
> ```go
> for i := 0; i < 3; i++ {
> 	go func() {
> 		fmt.Println(i)
> 	}()
> }
> ```
> Output often becomes `3 3 3`. *(Note: Beginning in Go 1.22, the scope of loop variables was changed to be per-iteration, meaning `3 3 3` would no longer happen in modern Go versions by default, although it remains a classic interview question.)*