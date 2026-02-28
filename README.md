# Golang Study Guide 🚀

![Go Version](https://img.shields.io/badge/Go-1.21%2B-blue?style=flat&logo=go)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)
![Status](https://img.shields.io/badge/Status-Active-success?style=flat)

A comprehensive collection of Go exercises, algorithms, and data structures implemented with performance analysis. This repository serves as a study guide for mastering Golang concepts, from basics to concurrency.

## 📚 Table of Contents

- [Basics & Algorithms](#basics--algorithms)
    - [Reverse String & Number](#reverse-string--number)
    - [Prime Numbers Check & Range](#prime-numbers-check--range)
    - [Factorial (Recursive)](#factorial-recursive)
    - [Fibonacci Series (Recursive)](#fibonacci-series-recursive)
    - [Length of Last Word](#length-of-last-word)
    - [Valid Parentheses](#valid-parentheses)
    - [Squares of a Sorted Array](#squares-of-a-sorted-array)
- [Data Structures](#data-structures)
    - [Count Vowels & Consonants (Built-in)](#count-vowels--consonants-built-in)
    - [Count Vowels & Consonants (Custom)](#count-vowels--consonants-custom)
    - [Reverse Array](#reverse-array)
    - [Remove Duplicates from Slice](#remove-duplicates-from-slice)
    - [Sort Employees by Age & Name](#sort-employees-by-age--name)
- [Interfaces](#interfaces)
    - [Geometry Shape Pattern](#geometry-shape-pattern)
    - [Payment Method (Factory Pattern)](#payment-method-factory-pattern)
- [Concurrency](#concurrency)
    - [Odd/Even Numbers (Single Channel)](#oddeven-numbers-single-channel)
    - [Odd/Even Numbers (Multiple Channels)](#oddeven-numbers-multiple-channels)
    - [Sum of Odd/Even Numbers](#sum-of-oddeven-numbers)
    - [Prime Numbers from Array](#prime-numbers-from-array)
    - [Index of Prime Numbers](#index-of-prime-numbers)
    - [Synchronous vs Asynchronous Concepts](#synchronous-vs-asynchronous-concepts)
- [Advanced Concurrency Patterns](#advanced-concurrency-patterns)
    - [Concurrent Prime Checker (Worker Pool)](#concurrent-prime-checker-worker-pool)
    - [Concurrent Vowel Counter (Map-Reduce)](#concurrent-vowel-counter-map-reduce)
    - [Concurrent Merge Sort](#concurrent-merge-sort)
    - [Atomic Counters (Sync/Atomic)](#atomic-counters-syncatomic)
    - [Worker Pool (Job Processing)](#worker-pool-job-processing)
    - [Worker Pool (Unbuffered Channel)](#worker-pool-unbuffered-channel)
    - [Concurrent Cache (sync.RWMutex)](#concurrent-cache-syncrwmutex)

---

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

## Data Structures

### Count Vowels & Consonants (Built-in)
Uses `strings` and `unicode` packages to count vowels and consonants.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	str := "abcdefghijklmnopqrstuvwxyz1234567890!@#$%^&*aeiouelephant"
	// Variables to count vowels and consonants respectively.
	var vowelCount int
	var consonantCount int
	for _, char := range str {
		if unicode.IsLetter(char) {
			if strings.ContainsRune("aeiou", char) {
				vowelCount = vowelCount + 1
			} else {
				consonantCount = consonantCount + 1
			}
		}
	}
	fmt.Println("Vowels are :: ", vowelCount)
	fmt.Println("Consonant are :: ", consonantCount)
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Vowels are ::  13
  Consonant are ::  26
  ```
- **Complexity:**
  - **Time:** O(N)
  - **Space:** O(1)
- **Performance:** ~140.1 ns/op (Slower due to package overhead)

[Back to Top](#table-of-contents)

---

### Count Vowels & Consonants (Custom)
Optimized manual implementation without `strings` package.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

func main() {
	str := "abcdefghijklmnopqrstuvwxyz1234567890!@#$%^&*"
	// Manual counters for vowels and consonants.
	var vowel int
	var consonant int
	for _, char := range str {
		if isLetter(char) {
			if isVowel(char) {
				vowel = vowel + 1
			} else {
				consonant = consonant + 1
			}
		}
	}
	fmt.Println("Vowel count :: ", vowel)
	fmt.Println("Consonant count :: ", consonant)
}

func isLetter(char rune) bool {
	return (char >= 'a' && char <= 'z') || (char >= 'A' && char <= 'Z')
}

func isVowel(char rune) bool {
	return char == 'a' || char == 'e' || char == 'i' || char == 'o' || char == 'u' || char == 'A' || char == 'E' || char == 'I' || char == 'O' || char == 'U'
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Vowel count ::  5
  Consonant count ::  21
  ```
- **Complexity:**
  - **Time:** O(N)
  - **Space:** O(1)
- **Performance:** ~44.7 ns/op (~3x faster than built-in approach)

[Back to Top](#table-of-contents)

---

### Reverse Array
Reverses an array in-place.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

func main() {
	arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	fmt.Println("Array :: ", arr)
	fmt.Println("Reverse of Array :: ", reverseArray(arr))
}

func reverseArray(arr []int) []int {
	// Reverse the array in-place by swapping elements from both ends.
	for i := 0; i < len(arr)/2; i++ {
		arr[i], arr[len(arr)-i-1] = arr[len(arr)-i-1], arr[i]
	}
	return arr
}
```
</details>

#### Analysis
- **Expected Output:** `Reverse of Array ::  [10 9 8 7 6 5 4 3 2 1]`
- **Complexity:**
  - **Time:** O(N)
  - **Space:** O(1)
- **Performance:** ~13.8 ns/op

[Back to Top](#table-of-contents)

---

### Remove Duplicates from Slice
Finds unique values, counts occurrences, and tracks indices of duplicates using maps.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

func main() {
	arr := []int{1, 2, 3, 4, 5, 12, 3, 4, 5, 1, 2, 3, 4, 5, 12}
	fmt.Println("Unique Values :: ", duplicateElements(arr))
	fmt.Println("Duplicate Values :: ", duplicateElementsCount(arr))
	for key, val := range duplicateElementsCount(arr) {
		fmt.Println(key, " repeated :: ", val, " times")
	}
	fmt.Println("Duplicate Values :: ", duplicateElementsIndex(arr))
	for key, val := range duplicateElementsIndex(arr) {
		fmt.Println(key, " repeated Index :: ", val)
	}
}

func duplicateElements(arr []int) []int {
	// 'result' slice to store unique elements.
	var result []int
	// 'duplicate' map to track seen elements. Key is the element, value is boolean (true if seen).
	duplicate := make(map[int]bool)
	for i := 0; i < len(arr); i++ {
		if !duplicate[arr[i]] {
			duplicate[arr[i]] = true
			result = append(result, arr[i])
		}
	}
	return result
}

func duplicateElementsCount(arr []int) map[int]int {
	// 'result' map where Key = element, Value = count of occurrences.
	result := map[int]int{}
	for i := 0; i < len(arr); i++ {
		val, ok := result[arr[i]]
		if ok {
			result[arr[i]] = val + 1
		} else {
			result[arr[i]] = 1
		}
	}
	return result
}

func duplicateElementsIndex(arr []int) map[int][]int {
	// 'result' map where Key = element, Value = slice of indices where it appears.
	result := map[int][]int{}
	for i := 0; i < len(arr); i++ {
		val, ok := result[arr[i]]
		if ok {
			val = append(val, i)
			result[arr[i]] = val
		} else {
			result[arr[i]] = []int{i}
		}
	}
	return result
}
```
</details>

#### Analysis
- **Complexity:**
  - **Time:** O(N)
  - **Space:** O(N)
- **Performance:** ~119.4 ns/op | 120 B/op

[Back to Top](#table-of-contents)

---

### Sort Employees by Age & Name
Custom sort implementation for struct slices.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import "fmt"

type Employees struct {
	Id   int
	Name string
	Age  int
}

func main() {
	empData := []Employees{{1, "Alok", 19}, {2, "Sonam", 33}, {3, "Harshit", 33}, {4, "Satyendar", 28}}
	fmt.Println(ManipulateEmpData(empData))
}

func ManipulateEmpData(emps []Employees) []Employees {
	// Nested loops to perform a Bubble Sort based on Age and Name.
	for i := 0; i < len(emps); i++ {
		for j := i + 1; j < len(emps); j++ {
			if emps[i].Age > emps[j].Age {
				emps[i], emps[j] = emps[j], emps[i]
			} else if emps[i].Age == emps[j].Age {
				if emps[i].Name > emps[j].Name {
					emps[i], emps[j] = emps[j], emps[i]
				}
			}
		}
	}
	return emps
}
```
</details>

#### Analysis
- **Expected Output:** `[{1 Alok 19} {4 Satyendar 28} {3 Harshit 33} {2 Sonam 33}]`
- **Complexity:**
  - **Time:** O(N^2) (Bubble Sort)
  - **Space:** O(1)
- **Performance:** ~29.4 ns/op (Small N=4)

[Back to Top](#table-of-contents)

---

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

## Concurrency

### Odd/Even Numbers (Single Channel)
Two goroutines communicating via a single unbuffered channel.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	// 'wg' WaitGroup is used to wait for all goroutines to finish.
	var wg sync.WaitGroup
	// 'ch' is an unbuffered channel for integer communication between goroutines.
	ch := make(chan int)
	wg.Add(2)
	go oddNums(ch, &wg)
	go evenNums(ch, &wg)
	ch <- 1
	wg.Wait()
}

func oddNums(ch chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	for n := range ch {
		fmt.Println(n)
		ch <- n + 1
	}
}

func evenNums(ch chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	for n := range ch {
		if n > 10 {
			close(ch)
			return
		} else {
			fmt.Println(n)
			ch <- n + 1
		}
	}
}
```
</details>

#### Analysis
- **Expected Output:** Prints 1 to 10 sequentially.
- **Complexity:**
  - **Time:** O(N)
  - **Context Switching:** High due to ping-pong communication.

[Back to Top](#table-of-contents)

---

### Odd/Even Numbers (Multiple Channels)
Uses separate channels for odd and even numbers to separate concerns.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	// Separate channels for odd and even numbers to handle them independently.
	oddCh := make(chan int)
	evenCh := make(chan int)
	wg.Add(2)
	go oddNums(oddCh, evenCh, &wg)
	go evenNums(oddCh, evenCh, &wg)
	oddCh <- 1
	wg.Wait()
}

func oddNums(oddCh, evenCh chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	for n := range oddCh {
		fmt.Println(n)
		evenCh <- n + 1
	}
}

func evenNums(oddCh, evenCh chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	for n := range evenCh {
		fmt.Println(n)
		if n == 10 {
			close(oddCh)
			close(evenCh)
			return
		}
		oddCh <- n + 1
	}
}
```
</details>

#### Analysis
- **Expected Output:** Prints 1 to 10 sequentially.
- **Note:** This pattern cleanly separates signal paths for different workers.

[Back to Top](#table-of-contents)

---

### Sum of Odd/Even Numbers
Calculates sums in background goroutines concurrently.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	// Variables to store the final sums.
	var oddSum, evenSum int
	odd := make(chan int)
	even := make(chan int)
	nums := []int{1, 2, 3, 4, 5, 11, 20}
	for _, val := range nums {
		wg.Add(1)
		go findNums(val, odd, even, &wg)
	}
	go func() {
		for n := range odd {
			oddSum = oddSum + n
		}
	}()
	go func() {
		for n := range even {
			evenSum = evenSum + n
		}
	}()
	wg.Wait()
	go func() {
		close(odd)
		close(even)
	}()

	fmt.Println("Sum of Odd Numbers :: ", oddSum)
	fmt.Println("Sum of Even Numbers :: ", evenSum)
}

func findNums(num int, odd, even chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	if num%2 == 0 {
		even <- num
	} else {
		odd <- num
	}
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Sum of Odd Numbers ::  20
  Sum of Even Numbers ::  26
  ```
- **Complexity:** O(N) concurrent. High overhead for creating a goroutine per number for small datasets.

[Back to Top](#table-of-contents)

---

### Prime Numbers from Array
Workers process array elements and send prime numbers to a results channel.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11}
	// Channel to collect prime numbers from concurrent workers.
	primeNums := make(chan int)
	for _, val := range arr {
		wg.Add(1)
		go PrimeNumbers(val, primeNums, &wg)
	}
	go func() {
		wg.Wait()
		close(primeNums)
	}()

	for n := range primeNums {
		fmt.Println(n)
	}
}

func PrimeNumbers(val int, ch chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	if isPrime(val) {
		ch <- val
	}
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
- **Expected Output:** Prints prime numbers (order may vary due to concurrency).

[Back to Top](#table-of-contents)

---

### Index of Prime Numbers
Returns the index of prime numbers found in an array.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	// Channel to communicate indices of prime numbers back to main.
	primeIndex := make(chan int)
	arr := []int{2, 4, 6, 7, 8, 11, 13, 17}
	wg.Add(2)
	go PrimeIndex(arr, primeIndex, &wg)
	go PrintIndex(arr, primeIndex, &wg)
	wg.Wait()
}

func PrimeIndex(arr []int, ch chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	for i, val := range arr {
		if isPrime(val) {
			ch <- i
		}
	}
	close(ch)
}

func PrintIndex(arr []int, ch chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	for index := range ch {
		fmt.Println("Prime Number :: ", arr[index], " index in Array is :: ", index)
	}
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
  Prime Number ::  2  index in Array is ::  0
  Prime Number ::  7  index in Array is ::  3
  ...
  ```

[Back to Top](#table-of-contents)

---

### Synchronous vs Asynchronous Concepts

**Synchronous programming** runs tasks one after another, like following a recipe step-by-step.
**Asynchronous programming** allows multiple tasks to run concurrently, improving performance for I/O-heavy operations.

#### Synchronous Example
<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"time"
)

func task(name string) {
	fmt.Println(name, "started")
	time.Sleep(2 * time.Second) // simulate work
	fmt.Println(name, "finished")
}

func main() {
	task("Task 1")
	task("Task 2")
	fmt.Println("All tasks done synchronously")
}
```
</details>

#### Asynchronous Example
<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"time"
)

func task(name string, ch chan string) {
	fmt.Println(name, "started")
	time.Sleep(2 * time.Second) // simulate work
	ch <- name + " finished"
}

func main() {
	ch := make(chan string)
	
	go task("Task 1", ch)
	go task("Task 2", ch)

	// Wait for both tasks to send their results
	for i := 0; i < 2; i++ {
		fmt.Println(<-ch)
	}

	fmt.Println("All tasks done asynchronously")
}
```
</details>

[Back to Top](#table-of-contents)

---

## Advanced Concurrency Patterns

### Concurrent Prime Checker (Worker Pool)
Efficiently finds prime numbers in a range using a pool of workers.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"sync"
)

// Worker function to process numbers from the jobs channel.
func worker(jobs <-chan int, results chan<- int, wg *sync.WaitGroup) {
	defer wg.Done()
	for n := range jobs {
		if isPrime(n) {
			results <- n
		}
	}
}

func main() {
	start := 2
	end := 100
	
	// Create channels for jobs and results.
	jobs := make(chan int, end-start+1)
	results := make(chan int, end-start+1)
	
	// Define number of workers (simulating a thread pool).
	numWorkers := 4
	var wg sync.WaitGroup
	
	// Start workers.
	for w := 0; w < numWorkers; w++ {
		wg.Add(1)
		go worker(jobs, results, &wg)
	}
	
	// Send jobs.
	for i := start; i <= end; i++ {
		jobs <- i
	}
	close(jobs)
	
	// Close results channel when all workers are done.
	go func() {
		wg.Wait()
		close(results)
	}()
	
	// Collect results.
	var primes []int
	for p := range results {
		primes = append(primes, p)
	}
	
	fmt.Println("Primes found:", len(primes))
	fmt.Println(primes)
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
- **Pattern:** Worker Pool.
- **Benefits:** Controls resource usage by limiting the number of active goroutines, preventing system overload when processing large ranges.

[Back to Top](#table-of-contents)

---

### Concurrent Vowel Counter (Map-Reduce)
Counts vowels in a long string by splitting it into chunks and processing them in parallel.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"strings"
	"sync"
)

func countVowelsChunk(chunk string, result chan<- int, wg *sync.WaitGroup) {
	defer wg.Done()
	count := 0
	for _, char := range chunk {
		if strings.ContainsRune("aeiouAEIOU", char) {
			count++
		}
	}
	result <- count
}

func main() {
	text := "This is a very long string that we want to process concurrently to demonstrate the map reduce pattern in Golang."
	// Simulate "Very Long String" by repeating.
	for i := 0; i < 1000; i++ {
		text += " count me "
	}

	numChunks := 4
	chunkSize := len(text) / numChunks
	
	resultCh := make(chan int, numChunks)
	var wg sync.WaitGroup

	// Map step: distribute chunks to workers.
	for i := 0; i < numChunks; i++ {
		start := i * chunkSize
		end := start + chunkSize
		if i == numChunks-1 {
			end = len(text)
		}
		
		wg.Add(1)
		go countVowelsChunk(text[start:end], resultCh, &wg)
	}

	// Wait for workers in a separate goroutine to close channel.
	go func() {
		wg.Wait()
		close(resultCh)
	}()

	// Reduce step: combine results.
	totalVowels := 0
	for count := range resultCh {
		totalVowels += count
	}

	fmt.Println("Total Vowels:", totalVowels)
}
```
</details>

#### Analysis
- **Pattern:** Map-Reduce (Data Parallelism).
- **Benefits:** Significant speedup for CPU-bound tasks on large datasets by utilizing multiple cores.

[Back to Top](#table-of-contents)

---

### Concurrent Merge Sort
Recursive parallel merge sort implementation.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	arr := []int{9, 4, 3, 2, 8, 5, 1, 6, 7, 0}
	fmt.Println("Original:", arr)
	
	sorted := ParallelMergeSort(arr)
	fmt.Println("Sorted:  ", sorted)
}

func ParallelMergeSort(arr []int) []int {
	if len(arr) <= 1 {
		return arr
	}
	
	// Threshold to switch to sequential sort to avoid overhead.
	if len(arr) < 5 {
		return sequentialMergeSort(arr)
	}

	mid := len(arr) / 2
	var left, right []int
	var wg sync.WaitGroup
	
	wg.Add(2)
	
	go func() {
		defer wg.Done()
		left = ParallelMergeSort(arr[:mid])
	}()
	
	go func() {
		defer wg.Done()
		right = ParallelMergeSort(arr[mid:])
	}()
	
	wg.Wait()
	return merge(left, right)
}

func sequentialMergeSort(arr []int) []int {
	if len(arr) <= 1 {
		return arr
	}
	mid := len(arr) / 2
	left := sequentialMergeSort(arr[:mid])
	right := sequentialMergeSort(arr[mid:])
	return merge(left, right)
}

func merge(left, right []int) []int {
	result := make([]int, 0, len(left)+len(right))
	i, j := 0, 0
	
	for i < len(left) && j < len(right) {
		if left[i] < right[j] {
			result = append(result, left[i])
			i++
		} else {
			result = append(result, right[j])
			j++
		}
	}
	
	result = append(result, left[i:]...)
	result = append(result, right[j:]...)
	return result
}
```
</details>

#### Analysis
- **Pattern:** Divide and Conquer (Parallel Recursion).
- **Benefits:** Reduces sorting time for massive arrays. Note the threshold check to prevent excessive goroutine creation overhead for small sub-arrays.

[Back to Top](#table-of-contents)

---

### Atomic Counters (Sync/Atomic)
Demonstrates safe concurrent state modification using atomic operations instead of mutexes, avoiding race conditions.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

func main() {
	numbers := make([]int, 1000000)

	for i := 0; i < 1000000; i++ {
		numbers[i] = i
	}

	var totalCount int64
	var globalIndex int64
	var wg sync.WaitGroup

	numGoroutines := 10000

	for i := 0; i < numGoroutines; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			for {
				// Atomically increment index to pick a number
				idx := atomic.AddInt64(&globalIndex, 1) - 1
				if idx >= int64(len(numbers)) {
					return
				}
				if numbers[idx]%2 == 0 {
					// Atomically increment counter
					atomic.AddInt64(&totalCount, 1)
				}
			}
		}()
	}

	wg.Wait()
	fmt.Printf("Total: %d\n", totalCount)
}
```
</details>

#### Analysis
- **Expected Output:** `Total: 500000` (Exact count of even numbers).
- **Complexity:**
  - **Time:** O(N/P) where P is num goroutines.
  - **Performance:** Atomic operations are generally faster than Mutex locks for simple counters but can suffer from cache contention under high load.

[Back to Top](#table-of-contents)

---

### Worker Pool (Job Processing)
A generic worker pool implementation where workers process jobs concurrently.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"time"
)

type Job struct {
	ID int
}

func Worker(id int, jobs chan Job, results chan int) {
	for job := range jobs {
		fmt.Printf("Worker %d Processing Job %d\n", id, job.ID)
		time.Sleep(time.Second) // Simulate expensive task
		results <- job.ID * 2
	}
}

func main() {
	numJobs := 5
	numWorkers := 3

	// Buffered channels for jobs and results
	jobs := make(chan Job, numJobs)
	results := make(chan int, numJobs)

	// Start Workers
	for w := 1; w <= numWorkers; w++ {
		go Worker(w, jobs, results)
	}

	// Send Jobs
	for j := 1; j <= numJobs; j++ {
		jobs <- Job{ID: j}
	}
	close(jobs) // Close jobs channel so workers know when to stop

	// Collect Results
	for a := 1; a <= numJobs; a++ {
		fmt.Println("Result :: ", <-results)
	}
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Worker 3 Processing Job 1
  Worker 1 Processing Job 2
  Worker 2 Processing Job 3
  ...
  Result ::  2
  Result ::  4
  Result ::  6
  Result ::  8
  Result ::  10
  ```
  *(Order of worker processing defines the output order)*
- **Complexity:**
  - **Time:** O(N/K) where N is jobs, K is workers.
  - **Space:** O(N) for buffered channels.
- **Benefits:**
  - Limits concurrency to `numWorkers` to prevent resource exhaustion.
  - Efficiently distributes work among available workers.

[Back to Top](#table-of-contents)

---

### Worker Pool (Unbuffered Channel)
A worker pool using **unbuffered channels**, where the sender blocks until a worker is ready to receive a job. This ensures strict synchronization.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"time"
)

type Job struct {
	ID int
}

func Worker(id int, jobs chan Job, results chan int) {
	for job := range jobs {
		fmt.Printf("Worker %d started job %d\n", id, job.ID)
		time.Sleep(time.Second) // Simulate work
		fmt.Printf("Worker %d finished job %d\n", id, job.ID)
		results <- job.ID * 2
	}
}

func main() {
	numJobs := 5
	numWorkers := 3

	// Unbuffered channels
	jobs := make(chan Job)
	results := make(chan int)

	// Start Workers
	for w := 1; w <= numWorkers; w++ {
		go Worker(w, jobs, results)
	}

	// Send jobs in a separate goroutine to prevent deadlock
	go func() {
		for j := 1; j <= numJobs; j++ {
			jobs <- Job{ID: j} // Will block until a worker is ready
		}
		close(jobs)
	}()

	// Collect Results
	for a := 1; a <= numJobs; a++ {
		res := <-results
		fmt.Printf("Result :: %d\n", res)
	}
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Worker 2 started job 2
  Worker 3 started job 3
  Worker 1 started job 1
  Worker 1 finished job 1
  Worker 3 finished job 3
  Worker 1 started job 4
  Result :: 2
  Worker 2 finished job 2
  Result :: 6
  Result :: 4
  Worker 3 started job 5
  Worker 1 finished job 4
  Result :: 8
  Worker 3 finished job 5
  Result :: 10
  ```
- **Key Difference:** unlike buffered channels, the `jobs <-` operation blocks if all workers are busy. The sending goroutine pauses execution until a worker becomes available.
- **Use Case:** When you need strict hand-off or want to prevent the job queue from growing indefinitely.

[Back to Top](#table-of-contents)

---

### Concurrent Cache (sync.RWMutex)
A thread-safe concurrent cache implementation using `sync.RWMutex` which allows multiple concurrent readers but exclusive writers, improving performance over a standard `sync.Mutex` in read-heavy scenarios.

#### Solution 1: Basic
<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
	"fmt"
	"sync"
)

type Cache struct {
	mu sync.RWMutex
	m  map[string]string
}

func NewCache() *Cache {
	return &Cache{
		m: make(map[string]string),
	}
}

// Set adds or updates a key
func (c *Cache) Set(k, v string) {
	c.mu.Lock()
	defer c.mu.Unlock()
	c.m[k] = v
}

// Get retrieves a key
func (c *Cache) Get(k string) (string, bool) {
	c.mu.RLock()
	defer c.mu.RUnlock()
	val, ok := c.m[k]
	return val, ok
}

// Delete removes a key safely
func (c *Cache) Delete(k string) {
	c.mu.Lock()
	defer c.mu.Unlock()
	delete(c.m, k)
}

func main() {
	cache := NewCache()

	cache.Set("A", "1")

	if val, ok := cache.Get("A"); ok {
		fmt.Println("Before Delete:", val)
	}

	cache.Delete("A")

	if _, ok := cache.Get("A"); !ok {
		fmt.Println("Key A deleted successfully")
	}
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Before Delete: 1
  Key A deleted successfully
  ```

#### Solution 2: Cache with Multiple Keys
<details>
<summary><strong>View Solution</strong></summary>

```go
// You can edit this code!
// Click here and start typing.
package main

import (
	"fmt"
	"sync"
)

type Cache struct {
	mu sync.RWMutex
	m  map[string]string
}

func NewCache() *Cache {
	return &Cache{
		m: make(map[string]string),
	}
}

func (c *Cache) Set(k, v string) {
	c.mu.Lock()
	defer c.mu.Unlock()
	c.m[k] = v
}

func (c *Cache) Delete(k string) {
	c.mu.Lock()
	defer c.mu.Unlock()
	delete(c.m, k)
}

func (c *Cache) Get(k string) (string, bool) {
	c.mu.RLock()
	defer c.mu.RUnlock()
	val, ok := c.m[k]
	return val, ok
}

func main() {
	cache := NewCache()
	cache.Set("A", "1")
	cache.Set("B", "2")
	cache.Set("C", "3")
	cache.Set("D", "4")
	if val, ok := cache.Get("A"); ok {
		fmt.Println("Key found in Cache :: ", val)
	}
	cache.Delete("A")
	if _, ok := cache.Get("A"); !ok {
		fmt.Println("Key delete in Cache")
	}
	for key, val := range cache.m {
		fmt.Println(key, " -- ", val)
	}
}
```
</details>

#### Analysis
- **Expected Output:**
  ```text
  Key found in Cache ::  1
  Key delete in Cache
  A  --  1
  B  --  2
  C  --  3
  D  --  4
  ```
*(Note: Output order of keys B, C, D may vary)*

[Back to Top](#table-of-contents)
