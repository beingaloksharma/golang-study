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
