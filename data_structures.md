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
