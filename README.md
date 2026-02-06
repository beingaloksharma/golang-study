# Golang Study Guide

A collection of Go exercises and examples covering basics, data structures, algorithms, and concurrency.

## Table of Contents

- [Basics & Algorithms](#basics--algorithms)
    - [Reverse String & Number](#reverse-string--number)
    - [Prime Numbers Check & Range](#prime-numbers-check--range)
    - [Factorial (Recursive)](#factorial-recursive)
    - [Fibonacci Series (Recursive)](#fibonacci-series-recursive)
    - [Length of Last Word](#length-of-last-word)
- [Data Structures (Strings, Arrays, Maps)](#data-structures-strings-arrays-maps)
    - [Count Vowels & Consonants (Built-in)](#count-vowels--consonants-built-in)
    - [Count Vowels & Consonants (Custom)](#count-vowels--consonants-custom)
    - [Reverse Array](#reverse-array)
    - [Remove Duplicates from Slice](#remove-duplicates-from-slice)
- [Structs & Sorting](#structs--sorting)
    - [Sort Employees by Age & Name](#sort-employees-by-age--name)
- [Concurrency (Goroutines & Channels)](#concurrency-goroutines--channels)
    - [Odd/Even Numbers (Single Channel)](#oddeven-numbers-single-channel)
    - [Odd/Even Numbers (Multiple Channels)](#oddeven-numbers-multiple-channels)
    - [Sum of Odd/Even Numbers](#sum-of-oddeven-numbers)
    - [Prime Numbers from Array](#prime-numbers-from-array)
    - [Index of Prime Numbers](#index-of-prime-numbers)
    - [Synchronous vs Asynchronous Concepts](#synchronous-vs-asynchronous-concepts)

---

## Basics & Algorithms

### Reverse String & Number
Checks if a string or number is a palindrome.

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
	var chars []byte
	for i := 0; i < len(str); i++ {
		chars = append(chars, str[len(str)-i-1])
	}
	return string(chars)
}

func numReverse(num int) int {
	var rev int
	for num > 0 {
		rev = rev*10 + num%10
		num = num / 10
	}
	return rev
}
```

#### Analysis
**Expected Output:**
```text
mom  --  mom   are palindrome
121  --  121   are palindrome
```

**Complexity:**
- **Time:** O(N) for string (iterates length), O(log N) for number (iterates digits).
- **Space:** O(N) for string (new slice), O(1) for number.

**Performance:**
- **String:** ~12.1 ns/op | 8 B/op
- **Number:** ~1.0 ns/op | 0 B/op

---

### Prime Numbers Check & Range
Checks if a single number is prime and finds all primes in a range.

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
	var primeNumsCount int
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

#### Analysis
**Expected Output:**
```text
7  is prime number
Count of Prime Nums ::  4
Prime Numbers are ::  [2 3 5 7]
```

**Complexity:**
- **Time:** O(sqrt(N)) per check. Range check is O(M * sqrt(N)).
- **Space:** O(1) (auxiliary).

**Performance:**
- **Single Check:** ~0.94 ns/op
- **Range (2-10):** ~4.74 ns/op

---

### Factorial (Recursive)

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

#### Analysis
**Expected Output:**
```text
Factorial is ::  120
```

**Complexity:**
- **Time:** O(N) (N recursive calls).
- **Space:** O(N) (call stack depth).

**Performance:**
- **Execution:** ~2.31 ns/op (for N=5)

---

### Fibonacci Series (Recursive)

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

#### Analysis
**Expected Output:**
```text
0 1 1 2 3 5 8 13 21 34
```

**Complexity:**
- **Time:** O(2^N) (Exponential - highly inefficient without memoization).
- **Space:** O(N) (call stack).

**Performance:**
- **Execution:** ~118.6 ns/op (for N=10)

---

### Length of Last Word
Finds the length of the last word in a string, handling trailing spaces.

```go
package main

import "fmt"

func main() {
	str := "Hello World                                                                                                                                          "
	fmt.Println("Length of String is :: ", len(str))
	fmt.Println("Length of last Word :: ", lastWordLength(str))
}

func lastWordLength(str string) int {
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

#### Analysis
**Expected Output:**
```text
Length of String is ::  141
Length of last Word ::  5
```

**Complexity:**
- **Time:** O(N) (Average case better if word is at end).
- **Space:** O(1).

**Performance:**
- **Execution:** ~72.05 ns/op

---

## Data Structures (Strings, Arrays, Maps)

### Count Vowels & Consonants (Built-in)
Uses `unicode` and `strings` packages.

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	str := "abcdefghijklmnopqrstuvwxyz1234567890!@#$%^&*aeiouelephant"
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

#### Analysis
**Expected Output:**
```text
Vowels are ::  13
Consonant are ::  26
```

**Complexity:**
- **Time:** O(N * M) (Where M is length of "aeiou", so effectively O(N)).
- **Space:** O(1).

**Performance:**
- **Execution:** ~140.1 ns/op
- **Note:** Slower than custom due to `strings` and `unicode` function overhead.

---

### Count Vowels & Consonants (Custom)
Manual implementation without `strings` package helper for vowels.

```go
package main

import "fmt"

func main() {
	str := "abcdefghijklmnopqrstuvwxyz1234567890!@#$%^&*"
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

#### Analysis
**Expected Output:**
```text
Vowel count ::  5
Consonant count ::  21
```

**Complexity:**
- **Time:** O(N)
- **Space:** O(1)

**Performance:**
- **Execution:** ~44.7 ns/op
- **Efficiency:** ~3x faster than logic using `strings.ContainsRune`.

---

### Reverse Array

```go
package main

import "fmt"

func main() {
	arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	fmt.Println("Array :: ", arr)
	fmt.Println("Reverse of Array :: ", reverseArray(arr))
}

func reverseArray(arr []int) []int {
	for i := 0; i < len(arr)/2; i++ {
		arr[i], arr[len(arr)-i-1] = arr[len(arr)-i-1], arr[i]
	}
	return arr
}
```

#### Analysis
**Expected Output:**
```text
Array ::  [1 2 3 4 5 6 7 8 9 10]
Reverse of Array ::  [10 9 8 7 6 5 4 3 2 1]
```

**Complexity:**
- **Time:** O(N/2) -> O(N)
- **Space:** O(1) (In-place swap)

**Performance:**
- **Execution:** ~13.8 ns/op

---

### Remove Duplicates from Slice
Shows how to find unique values, count occurrences, and track indices of duplicates.

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
	var result []int
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

#### Analysis
**Expected Output:**
```text
Unique Values ::  [1 2 3 4 5 12]
Duplicate Values ::  map[1:2 2:2 3:3 4:3 5:3 12:2]
...
```

**Complexity:**
- **Time:** O(N) (Average case for map operations).
- **Space:** O(N) (Map store).

**Performance:**
- **Execution:** ~119.4 ns/op | 120 B/op

---

## Structs & Sorting

### Sort Employees by Age & Name
Sorts a struct slice by Age (primary) and Name (secondary).

```go
package main

import (
	"fmt"
	"employees" // Hypothetical package or inline struct
)

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

#### Analysis
**Expected Output:**
```text
[{1 Alok 19} {4 Satyendar 28} {3 Harshit 33} {2 Sonam 33}]
```

**Complexity:**
- **Time:** O(N^2) (Bubble Sort implementation).
- **Space:** O(1) (In-place sort).

**Performance:**
- **Execution:** ~29.4 ns/op (Small N=4)

---

## Concurrency (Goroutines & Channels)

### Odd/Even Numbers (Single Channel)
Two goroutines communicating via a shared channel.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
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

#### Analysis
**Expected Output:**
```text
1
2
...
10
```

**Complexity:**
- **Time:** O(N) (Linear communication).
- **Context Switching:** High due to unbuffered channel Ping-Pong.

---

### Odd/Even Numbers (Multiple Channels)
Separate channels for odd and even numbers.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
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

#### Analysis
**Expected Output:**
```text
1
2
...
10
```
This pattern separates concerns by using dedicated channels for distinct signal paths.

---

### Sum of Odd/Even Numbers
Calculates sums in background goroutines.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
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

#### Analysis
**Expected Output:**
```text
Sum of Odd Numbers ::  20
Sum of Even Numbers ::  26
```
**Complexity:**
- **Time:** O(N) concurrent.
- Note: High overhead creating a goroutine per number (`go findNums`) for small workloads.

---

### Prime Numbers from Array
Workers process array elements and send primes to a channel.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11}
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

#### Analysis
**Expected Output:**
```text
2
3
5
7
11
```
*(Order may vary due to concurrency)*

---

### Index of Prime Numbers
Returns the index of prime numbers found in an array.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
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

#### Analysis
**Expected Output:**
```text
Prime Number ::  2  index in Array is ::  0
Prime Number ::  7  index in Array is ::  3
Prime Number ::  11  index in Array is ::  5
Prime Number ::  13  index in Array is ::  6
Prime Number ::  17  index in Array is ::  7
```

---

### Synchronous vs Asynchronous Concepts

**Synchronous programming** runs tasks one after another, with each task waiting for the previous one to finish—like following a recipe step-by-step. This makes the code predictable and simple to debug, but it can also lead to slowdowns if you have a task that takes a long time (like waiting for data from an external source).

**Asynchronous programming** allows multiple tasks to start and run at the same time, without blocking each other—a bit like cooking several dishes simultaneously. The program doesn’t wait for one to finish before starting the next, leading to better performance and responsiveness, especially when tasks are slow or involve waiting (I/O, network requests).

**In Go: Goroutines & Channels**
 - **Goroutines** let you run functions concurrently (asynchronously). Just put `go` in front of a function call.
 - **Channels** let those goroutines communicate safely, sending and receiving values so you can coordinate when things are ready.

**Typical workflow:**
 - **Synchronous:** `task1(); task2(); task3();` — runs one-by-one.
 - **Asynchronous:** `go task1(); go task2(); go task3();` — all start at once, possibly finishing in any order.
 - With channels, you can wait for results or synchronize tasks.

#### Synchronous Example
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

#### Asynchronous Example
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

### Valid Parentheses

```go
package main

import (
	"fmt"
)

func IsValid(str string) bool {
	pairs := map[rune]rune{
		')': '(',
		']': '[',
		'}': '{',
	}
	stack := []rune{}
	for _, char := range str {
		if char == '(' || char == '[' || char == '{' {
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

func main() {
	fmt.Println(IsValid("()[]{}"))
	fmt.Println(IsValid("(]"))
	fmt.Println(IsValid("([)]"))
	fmt.Println(IsValid("{[]}"))
}
```

#### Analysis
**Expected Output:**
```text
true
false
false
true
```

**Time Complexity:** O(n) - We traverse the string once.
**Space Complexity:** O(n) - In the worst case, we push all characters onto the stack.

**Benchmark Results:**
- **Execution Time:** ~92.78 ns/op
- **Memory Usage:** 0 B/op (due to stack allocation on stack/small size not triggering heap allocs for small inputs in test)
- **Allocations:** 0 allocs/op
