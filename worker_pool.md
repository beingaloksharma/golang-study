# Worker Pool Pattern 👷‍♂️

## 📌 Table of Contents
- [Concept & Architecture](#🏗-concept--architecture)
- [Why Use Worker Pools?](#🚀-why-use-worker-pools)
- [Real-World Use Cases](#📂-real-world-use-cases)
- [Basic Implementation](#⚒-basic-implementation)
- [Production-Ready Implementation](#🛡-production-ready-implementation)
- [Interview Deep Dive](#🧠-interview-deep-dive)
- [Resources](#📚-resources)

---

## 🏗 Concept & Architecture
A **Worker Pool** is a concurrency pattern where a fixed number of goroutines (workers) process tasks from a shared job queue (channel). This prevents resource exhaustion by limiting the number of active goroutines, regardless of the number of submitted tasks.

### Visual Flow
```text
Tasks → [ Job Queue (Channel) ] → [ Worker 1 ] → [ Results Channel (Optional) ]
                                  [ Worker 2 ]
                                  [ Worker N ]
```

---

## 🚀 Why Use Worker Pools?
- **Resource Control**: Limits CPU and memory usage by capping the number of active goroutines.
- **Backpressure**: Balances the rate of task production with the rate of consumption.
- **Rate Limiting**: Useful when interacting with external systems (APIs, Databases) that have concurrency limits.
- **Improved Performance**: Reduces scheduler overhead and context switching compared to creating "unlimited" goroutines.

---

## 📂 Real-World Use Cases
1. **Background Job Processing**: Sending emails, resizing images, generating reports.
2. **API Interaction**: Calling third-party APIs with specific rate limits.
3. **Data Pipelines**: Batch processing millions of records from a database or message queue (Kafka, RabbitMQ).
4. **Scraping/Crawling**: Fetching data from multiple URLs concurrently while limiting simultaneous connections.

---

## ⚒ Basic Implementation
A fundamental worker pool using channels to distribute tasks.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
    "fmt"
    "time"
)

func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Printf("Worker %d started job %d\n", id, j)
        time.Sleep(time.Second) // Simulate work
        results <- j * 2
    }
}

func main() {
    const numJobs = 5
    const numWorkers = 3

    jobs := make(chan int, numJobs)
    results := make(chan int, numJobs)

    // Start workers
    for w := 1; w <= numWorkers; w++ {
        go worker(w, jobs, results)
    }

    // Submit jobs
    for j := 1; j <= numJobs; j++ {
        jobs <- j
    }
    close(jobs) // Signal that no more jobs will be sent

    // Collect results
    for a := 1; a <= numJobs; a++ {
        fmt.Println("Result:", <-results)
    }
}
```
</details>

---

## 🛡 Production-Ready Implementation
In production, you should use `sync.WaitGroup` for proper termination tracking and `context.Context` for graceful shutdowns.

<details>
<summary><strong>View Solution</strong></summary>

```go
package main

import (
    "context"
    "fmt"
    "sync"
    "time"
)

type Job struct {
    ID int
}

func worker(ctx context.Context, id int, jobs <-chan Job, wg *sync.WaitGroup) {
    defer wg.Done()
    for {
        select {
        case <-ctx.Done():
            fmt.Printf("Worker %d: shutting down\n", id)
            return
        case job, ok := <-jobs:
            if !ok {
                return
            }
            fmt.Printf("Worker %d: processing job %d\n", id, job.ID)
            time.Sleep(500 * time.Millisecond) // Simulate work
        }
    }
}

func main() {
    ctx, cancel := context.WithCancel(context.Background())
    defer cancel()

    const numWorkers = 3
    jobs := make(chan Job, 10)
    var wg sync.WaitGroup

    // Start workers
    for w := 1; w <= numWorkers; w++ {
        wg.Add(1)
        go worker(ctx, 100+w, jobs, &wg)
    }

    // Submit some jobs
    for i := 1; i <= 5; i++ {
        jobs <- Job{ID: i}
    }

    time.Sleep(2 * time.Second)
    fmt.Println("Main: cancelling context...")
    cancel()

    close(jobs)
    wg.Wait()
    fmt.Println("Main: all workers stopped")
}
```
</details>

---

## 🧠 Interview Deep Dive

### High-Level Explanation
> "A Worker Pool is a concurrency pattern used to limit resource consumption. We maintain a set of long-lived goroutines that listen to a shared channel for work. This avoids the overhead of spawning a new goroutine for every single task, which could otherwise lead to memory exhaustion or API rate limiting issues."

### Common Pitfalls
- **Unbounded Queues**: Creating channels with massive buffers can lead to memory explosion if producers are much faster than workers.
- **Stray Goroutines**: Forgetting to `close()` the jobs channel or handle context cancellation can cause workers to hang forever (goroutine leak).
- **Race Conditions during Result Aggregation**: Using shared variables for results without atomics or mutexes.

### Tips for Senior Engineers
- **Backpressure**: Explain how bounded channels provide natural backpressure by blocking producers when the pool is full.
- **Dynamic Scaling**: Mention that sophisticated pools can adjust the number of workers based on the current load.
- **Error Handling**: Discuss how to propagate errors from workers back to the caller (e.g., using an error channel or `errgroup`).

---

## 📚 Resources
- [Go by Example: Worker Pools](https://gobyexample.com/worker-pools)
- [Go Concurrency Patterns (Rob Pike)](https://go.dev/blog/pipelines)
- [Effective Go: Goroutines](https://go.dev/doc/effective_go#goroutines)

---
[Back to Top](#worker-pool-pattern-👷‍♂️)