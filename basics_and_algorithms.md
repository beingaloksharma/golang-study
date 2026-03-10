## Basics & Algorithms

### Reverse String & Number
Checks if a string or number is a palindrome by reversing it.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

func main() {
	str := "mom"
	revStr := strReverse(str)
	if str == revStr {
		fmt.Println(str, " -- ", revStr, " ", "are palindrome")
	} else {
		fmt.Println(str, " -- ", revStr, " :: ", "are not palindrome")
	}
	num := 121
	revNum := numReverse(num)
	if num == revNum {
		fmt.Println(num, " -- ", revNum, " ", "are palindrome")
	} else {
		fmt.Println(num, " -- ", revNum, " :: ", "are not palindrome")
	}
}

func strReverse(str string) string {
	// Declare a byte slice to construct the reversed string efficiently.
	// Using a slice allows us to append characters dynamically.
	var chars []byte
	for i := 0; i < len(str); i++ {
		chars = append(chars, str[len(str)-i-1])
	}
	return string(chars)
}

func numReverse(num int) int {
	// 'rev' will hold the reversed number.
	// We use an integer here because we are performing mathematical operations.
	var rev int
	for num > 0 {
		rev = rev*10 + num%10
		num = num / 10
	}
	return rev
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  mom  --  mom   are palindrome
  121  --  121   are palindrome
  ```
- **Complexity:**
  - **Time:** O(N) for string, O(log N) for number.
  - **Space:** O(N) for string, O(1) for number.
- **Performance:**
  - **String:** ~12.1 ns/op | 8 B/op
  - **Number:** ~1.0 ns/op | 0 B/op

[Back to Top](#table-of-contents)

---

### Prime Numbers Check & Range
Checks if a number is prime and finds all primes within a specific range.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

func main() {
	num := 7
	if isPrime(num) {
		fmt.Println(num, " is prime number")
	} else {
		fmt.Println(num, " is not prime number")
	}
	// 'primeNumsCount' tracks the total number of prime numbers found.
	var primeNumsCount int
	// 'primeNums' is a slice initialized to store the actual prime numbers found in the range.
	// Slices are dynamic arrays in Go, perfect for when the number of elements isn't known upfront.
	var primeNums []int
	start := 2
	end := 10
	for i := start; i <= end; i++ {
		if isPrime(i) {
			primeNumsCount += 1
			primeNums = append(primeNums, i)
		}
	}
	fmt.Println("Count of Prime Nums :: ", primeNumsCount)
	fmt.Println("Prime Numbers are :: ", primeNums)
}

func isPrime(n int) bool {
	if n < 2 {
		return false
	}
	for i := 2; i*i <= n; i++ {
		if n%i == 0 {
			return false
		}
	}
	return true
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  7  is prime number
  Count of Prime Nums ::  4
  Prime Numbers are ::  [2 3 5 7]
  ```
- **Complexity:**
  - **Time:** O(M * sqrt(N)) for range check.
  - **Space:** O(1) auxiliary.
- **Performance:**
  - **Single Check:** ~0.94 ns/op
  - **Range (2-10):** ~4.74 ns/op

[Back to Top](#table-of-contents)

---

### Factorial (Recursive)
Calculates the factorial of a number using recursion.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

func main() {
	n := 5
	fmt.Println("Factorial is :: ", fact(n))
}

func fact(n int) int {
	if n == 0 || n == 1 {
		return 1
	}
	return n * fact(n-1)
}
```
</details>

#### Analysis
- **Expected Output:** `Factorial is ::  120`
- **Complexity:**
  - **Time:** O(N)
  - **Space:** O(N) (Recursion stack)
- **Performance:** ~2.31 ns/op (N=5)

[Back to Top](#table-of-contents)

---

### Fibonacci Series (Recursive)
Generates Fibonacci numbers recursively (demonstrating exponential complexity without memoization).

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

func main() {
	for i := 0; i < 10; i++ {
		fmt.Print(fibonacciSeries(i), " ")
	}
}

func fibonacciSeries(n int) int {
	if n < 2 {
		return n
	}
	return fibonacciSeries(n-1) + fibonacciSeries(n-2)
}
```
</details>

#### Analysis
- **Expected Output:** `0 1 1 2 3 5 8 13 21 34`
- **Complexity:**
  - **Time:** O(2^N) (Exponential)
  - **Space:** O(N)
- **Performance:** ~118.6 ns/op (N=10)

[Back to Top](#table-of-contents)

---

### Length of Last Word
Finds the length of the last word in a string, handling trailing spaces.

#### Solution 1: Manual Iteration
<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

func main() {
	str := "Hello World                                                                                                                                          "
	fmt.Println("Length of String is :: ", len(str))
	fmt.Println("Length of last Word :: ", lastWordLength(str))
}

func lastWordLength(str string) int {
	// 'length' stores the count of characters in the last word.
	// It is initialized to 0 by default.
	var length int
	for i := len(str) - 1; i >= 0; i-- {
		if str[i] == ' ' && length > 0 {
			return length
		} else {
			if (str[i] >= 'A' && str[i] >= 'Z') || (str[i] >= 'a' && str[i] >= 'z') {
				length = length + 1
			}
		}
	}
	return length
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Length of String is ::  141
  Length of last Word ::  5
  ```
- **Complexity:**
  - **Time:** O(N)
  - **Space:** O(1)
- **Performance:** ~72.05 ns/op

#### Solution 2: Strings Package (Basic)
<details>
<summary><strong>View Solution</strong></summary>

```go
// You can edit this code!
// Click here and start typing.
package main

import (
	"fmt"
	"strings"
)

func main() {
	str := "Hello World"
	fmt.Println("Length of String is :: ", len(str))

	strArray := strings.Fields(str)

	if len(strArray) == 0 {
		fmt.Println(0)
		return
	}

	fmt.Println("Length of last word ::", len(strArray[len(strArray)-1]))
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Length of String is ::  11
  Length of last word ::  5
  ```
- **Complexity:**
  - **Time:** O(N)
  - **Space:** O(N)

#### Solution 3: Strings Package (Advanced)
<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	str := []string{
		"Hello World",
		"Hello World ",
		"   Hello World",
		"Gopher",
		"          ",
		"",
		"Fly    me    to  the  moon",
	}
	for _, t := range str {
		fmt.Printf("Input: %q -> Result: %d\n", t, LastwordCount(t))
	}
}

func LastwordCount(str string) int {
	words := strings.Fields(str)
	if len(words) == 0 {
		return 0
	}
	return len(words[len(words)-1])
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Input: "Hello World" -> Result: 5
  Input: "Fly    me    to  the  moon" -> Result: 4
  ```
- **Complexity:**
  - **Time:** O(N)
  - **Space:** O(N) (Allocates slice of words)

[Back to Top](#table-of-contents)

---

### Valid Parentheses
Checks if a string has valid matching parentheses using a stack.

#### Solution 1: Map (Optimized)
<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

func main() {
	testCases := []string{
		"()",     // true
		"()[]{}", // true
		"(]",     // false
		"([)]",   // false
		"{[]}",   // true
		"(",      // false
		"",       // true
	}

	for _, tc := range testCases {
		fmt.Printf("Input: %-10s Valid: %v\n", tc, IsValid(tc))
	}
}

func IsValid(str string) bool {
	// 'pairs' map holds the closing parenthesis as key and opening as value for quick lookup.
	pairs := map[rune]rune{
		')': '(',
		'}': '{',
		']': '[',
	}
	// 'stack' is used to keep track of opening parentheses.
	// We use a slice as a stack (Last-In-First-Out).
	stack := make([]rune, 0, len(str))
	for _, char := range str {
		if _, isClosing := pairs[char]; !isClosing {
			stack = append(stack, char)
		} else {
			if len(stack) == 0 || stack[len(stack)-1] != pairs[char] {
				return false
			}
			stack = stack[:len(stack)-1]
		}

	}
	return len(stack) == 0
}
```
</details>

#### Solution 2: Switch Case
<details>
<summary><strong>View Solution</strong></summary>

```go
func IsValidSwitch(str string) bool {
	// Stack to hold open parentheses.
	stack := make([]rune, 0, len(str))
	for _, char := range str {
		if char == '(' || char == '{' || char == '[' {
			stack = append(stack, char)
		} else {
			switch char {
			case '(', '{', '[':
				stack = append(stack, char)
			case ')':
				if len(stack) == 0 || stack[len(stack)-1] != '(' {
					return false
				}
			case '}':
				if len(stack) == 0 || stack[len(stack)-1] != '{' {
					return false
				}
			case ']':
				if len(stack) == 0 || stack[len(stack)-1] != '[' {
					return false
				}
			}
			stack = stack[:len(stack)-1]
		}
	}
	return len(stack) == 0
}
```
</details>

#### Solution 3: Direct Map Check
<details>
<summary><strong>View Solution</strong></summary>

```go
func IsValidMap(str string) bool {
	pairs := map[rune]rune{
		')': '(',
		'}': '{',
		']': '[',
	}
	// 'stack' keeps track of opening brackets encountered.
	stack := make([]rune, 0, len(str))
	for _, char := range str {
		if char == '(' || char == '{' || char == '[' {
			stack = append(stack, char)
		} else {
			if len(stack) == 0 {
				return false
			}
			opener := stack[len(stack)-1]
			if opener != pairs[char] {
				return false
			}
			stack = stack[:len(stack)-1]
		}
	}
	return len(stack) == 0
}
```
</details>


#### Analysis
- **Expected Output:** Matches expected boolean values for balanced strings.
- **Complexity:**
  - **Time:** O(N)
  - **Space:** O(N)
- **Performance:** ~7.42 ns/op | 0 B/op

[Back to Top](#table-of-contents)

---

### Squares of a Sorted Array
Returns an array of the squares of each number, sorted in non-decreasing order.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
)

func SortedSquare(arr []int) []int {
	n := len(arr)
	// 'result' slice of size 'n' to store the squared values.
	result := make([]int, n)
	// 'left' and 'right' pointers to iterate from both ends of the sorted array.
	left, right := 0, n-1
	for i := n - 1; i >= 0; i-- {
		if abs(arr[left]) > abs(arr[right]) {
			result[i] = arr[left] * arr[left]
			left++
		} else {
			result[i] = arr[right] * arr[right]
			right--
		}
	}
	return result
}

func abs(n int) int {
	if n < 0 {
		return -n
	}
	return n
}

func main() {
	fmt.Println("Sorted Array :: ", SortedSquare([]int{-4, -1, 0, 3, 10}))
}
```
</details>

#### Analysis
- **Expected Output:** `Sorted Array ::  [0 1 9 16 100]`
- **Complexity:**
  - **Time:** O(N) (Two-pointer approach)
  - **Space:** O(N) (Result array)
- **Performance:** ~12.56 ns/op | 48 B/op

[Back to Top](#table-of-contents)

---
