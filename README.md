# 1. Reverse of string and number

```go
// You can edit this code!
// Click here and start typing.

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

# 2. Count Vowel and Consonant using built-In function 

```go
// You can edit this code!
// Click here and start typing.
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

# 3.  Count Vowel and Consonant using custom function 

```go
// You can edit this code!
// Click here and start typing.
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

# 4. Print even odd number using only one channel

```go
// You can edit this code!
// Click here and start typing.
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

# 5. Print even odd numbers using built-in function

```go
// You can edit this code!
// Click here and start typing.
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

# 6. Print prime numbers

```go
// You can edit this code!
// Click here and start typing.
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

# 7. Print factorial with recursive method

```go
// You can edit this code!
// Click here and start typing.
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

# 8. Print fibonacci series with recursive method

```go
// You can edit this code!
// Click here and start typing.
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
# 8. Print prime numbers from a array using channel

```go
// You can edit this code!
// Click here and start typing.
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

# 9. Write a program to Reverse of an array

```go
// You can edit this code!
// Click here and start typing.
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

# 9. Write a program to Remove duplicate value from an array/slice

```go
// You can edit this code!
// Click here and start typing.
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

# 10. Write a program to sort an Employee Struct based on their age. If age is same then sort based on their name.

```go
// You can edit this code!
// Click here and start typing.
package main

import (
	"fmt"
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

# 11. Write a program to find the length of last word in string.

```go
// You can edit this code!
// Click here and start typing.
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

# 12. Write a program to calculate the sum of odd and even numbers using goroutines and channels.

```go
// You can edit this code!
// Click here and start typing.
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	nums := []int{1, 2, 3, 4, 5}
	oddNums := make(chan int)
	evenNums := make(chan int)
	wg.Add(3)
	go findNumbers(nums, oddNums, evenNums, &wg)
	go sumOfOddNums(oddNums, &wg)
	go sumOfEvenNums(evenNums, &wg)
	wg.Wait()
}

func findNumbers(nums []int, odd, even chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	for _, val := range nums {
		if val%2 == 0 {
			even <- val
		} else {
			odd <- val
		}
	}
	close(even)
	close(odd)
}

func sumOfOddNums(odd chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	var sum int
	for n := range odd {
		sum = sum + n
	}
	fmt.Println("Sum of Odd Numbers :: ", sum)
}

func sumOfEvenNums(even chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	var sum int
	for n := range even {
		sum = sum + n
	}
	fmt.Println("Sum of Even Numbers :: ", sum)
}

```

```go
// You can edit this code!
// Click here and start typing.
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

# 13. Print the index of prime number using goroutines and channels.  

```go
// You can edit this code!
// Click here and start typing.
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

# 14. What is difference between synchronous and asynchronous. How It can be implemented in golang with goroutines and channels. Explain it with an example.  

<b>Synchronous programming</b> runs tasks one after another, with each task waiting for the previous one to finish—like following a recipe step-by-step. This makes the code predictable and simple to debug, but it can also lead to slowdowns if you have a task that takes a long time (like waiting for data from an external source).


<b>Asynchronous programming</b> allows multiple tasks to start and run at the same time, without blocking each other—a bit like cooking several dishes simultaneously. The program doesn’t wait for one to finish before starting the next, leading to better performance and responsiveness, especially when tasks are slow or involve waiting (I/O, network requests).

<b>In Go: Goroutines & Channels </b>  
 - Goroutines let you run functions concurrently (asynchronously). Just put go in front of a function call.
 - Channels let those goroutines communicate safely, sending and receiving values so you can coordinate when things are ready.

<b> Typical workflow: </b> 
 - Synchronous: task1(); task2(); task3(); — runs one-by-one.
 - Asynchronous: go task1(); go task2(); go task3(); — all start at once, possibly finishing in any order.
 - With channels, you can wait for results or synchronize tasks.

### Example

<b> Synchronous Example </b>
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

<b> Asynchronous Example </b>
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
