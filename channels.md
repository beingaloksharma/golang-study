# Channels

Channels are a core concurrency feature in Go used for communication between goroutines.

Go follows the philosophy:
> "Do not communicate by sharing memory; share memory by communicating."

Channels allow goroutines to send and receive data safely without explicit locks.

## Table of Contents
- [1️⃣ Channel Basics](#1️⃣-channel-basics)
- [2️⃣ Unbuffered Channels (Synchronous)](#2️⃣-unbuffered-channels-synchronous)
- [3️⃣ Buffered Channels (Asynchronous)](#3️⃣-buffered-channels-asynchronous)
- [4️⃣ Channel Directions](#4️⃣-channel-directions)
- [5️⃣ Closing Channels](#5️⃣-closing-channels)
- [6️⃣ Range Over Channel](#6️⃣-range-over-channel)
- [7️⃣ Common Channel Mistakes](#7️⃣-common-channel-mistakes)
- [8️⃣ Channel Internals](#8️⃣-channel-internals)
- [9️⃣ Real Use Cases](#9️⃣-real-use-cases)
- [🔟 Interview Questions](#🔟-interview-questions)
- [✅ Summary](#✅-summary)

---

## 1️⃣ Channel Basics

A channel is a typed conduit through which goroutines communicate.

**Syntax**
```go
ch := make(chan Type)
```

**Example:**
```go
package main
import "fmt"

func main() {
	ch := make(chan int)

	go func() {
		ch <- 10
	}()

	val := <-ch
	fmt.Println(val)
}
```

**Output:**
```text
10
```

**Explanation:**
- `ch <- 10`  → send value
- `<-ch`      → receive value

[Back to Top](#table-of-contents)

---

## 2️⃣ Unbuffered Channels (Synchronous)

Default channels are unbuffered.

This means:
- sender waits until receiver is ready
- receiver waits until sender sends

So communication is synchronous.

**Example**
```go
package main

import (
	"fmt"
)

func main() {
	ch := make(chan int)

	go func() {
		fmt.Println("Sending")
		ch <- 5
		fmt.Println("Sent")
	}()

	fmt.Println("Receiving")
	v := <-ch

	fmt.Println("Received:", v)
}
```

**Output:**
```text
Receiving
Sending
Sent
Received: 5
```

**Key concept:**
`send blocks until receive happens`

[Back to Top](#table-of-contents)

---

## 3️⃣ Buffered Channels (Asynchronous)

Buffered channels allow sending without immediate receiver.

**Syntax:**
```go
ch := make(chan int, capacity)
```

**Example:**
```go
package main
import "fmt"

func main() {
	ch := make(chan int, 2)

	ch <- 1
	ch <- 2

	fmt.Println(<-ch)
	fmt.Println(<-ch)
}
```

**Output:**
```text
1
2
```

Here: `capacity = 2`. So two sends can happen without blocking.

**Blocking Behavior**

| Operation | When it blocks |
|---|---|
| Send | buffer full |
| Receive | buffer empty |

**Example:**
```go
ch := make(chan int, 1)

ch <- 1
ch <- 2  // blocks
```

[Back to Top](#table-of-contents)

---

## 4️⃣ Channel Directions

Channels can be restricted to send-only or receive-only.

**Send-only Channel**
```go
func send(ch chan<- int) {
	ch <- 10
}
```
`chan<- int`
Meaning:
- send allowed
- receive not allowed

**Receive-only Channel**
```go
func receive(ch <-chan int) {
	val := <-ch
	fmt.Println(val)
}
```
`<-chan int`
Meaning:
- receive allowed
- send not allowed

**Example**
```go
package main

import "fmt"

func send(ch chan<- int) {
	ch <- 100
}

func receive(ch <-chan int) {
	fmt.Println(<-ch)
}

func main() {
	ch := make(chan int)
	go send(ch)
	receive(ch)
}
```

**Output:**
```text
100
```

**Benefits:**
- better API safety
- clear communication intent

[Back to Top](#table-of-contents)

---

## 5️⃣ Closing Channels

Channels can be closed using:
```go
close(ch)
```

Closing means: no more values will be sent.

**Example**
```go
package main
import "fmt"

func main() {
	ch := make(chan int)

	go func() {
		ch <- 1
		ch <- 2
		close(ch)
	}()

	for v := range ch {
		fmt.Println(v)
	}
}
```

**Output:**
```text
1
2
```

**Behavior After Closing**

Receiving from closed channel:
```go
v, ok := <-ch
```

If closed:
- `v` = zero value
- `ok` = false

**Example:**
```go
ch := make(chan int,1)

ch <- 5
close(ch)

v1, ok1 := <-ch
v2, ok2 := <-ch

fmt.Println(v1, ok1)
fmt.Println(v2, ok2)
```

**Output:**
```text
5 true
0 false
```

[Back to Top](#table-of-contents)

---

## 6️⃣ Range Over Channel

`range` reads values until channel is closed.

**Example:**
```go
package main
import "fmt"

func main() {
	ch := make(chan int)

	go func() {
		for i := 1; i <= 5; i++ {
			ch <- i
		}
		close(ch)
	}()

	for val := range ch {
		fmt.Println(val)
	}
}
```

**Output:**
```text
1
2
3
4
5
```

[Back to Top](#table-of-contents)

---

## 7️⃣ Common Channel Mistakes

**1️⃣ Writing to closed channel**
```go
close(ch)
ch <- 10
```
Runtime panic: `panic: send on closed channel`

**2️⃣ Closing channel twice**
```go
close(ch)
close(ch)
```
Panic: `panic: close of closed channel`

**3️⃣ Deadlock**

**Example:**
```go
ch := make(chan int)
ch <- 10
```
Error: `fatal error: all goroutines are asleep - deadlock!`
Because no receiver exists.

[Back to Top](#table-of-contents)

---

## 8️⃣ Channel Internals

Channels internally maintain:
- buffer queue
- send queue
- receive queue
- mutex

Simplified structure:
```text
channel
 ├── buffer
 ├── senders waiting
 └── receivers waiting
```

[Back to Top](#table-of-contents)

---

## 9️⃣ Real Use Cases

Channels are used for:
- worker pools
- pipelines
- fan-in / fan-out
- event streaming
- task coordination

[Back to Top](#table-of-contents)

---

## 🔟 Interview Questions

**1️⃣ Difference between buffered and unbuffered channels?**

| Feature | Unbuffered | Buffered |
|---|---|---|
| Communication | synchronous | asynchronous |
| Capacity | 0 | >0 |
| Blocking | sender waits | depends on buffer |

**2️⃣ Who should close a channel?**
Best practice:
- sender closes the channel
- receiver should NOT close

**3️⃣ Can we detect closed channel?**
Yes: `v, ok := <-ch`

**🔥 Senior Go Interview Question**

What will this print?
```go
func main() {
	ch := make(chan int,1)
	ch <- 1
	close(ch)

	fmt.Println(<-ch)
	fmt.Println(<-ch)
}
```

**Output:**
```text
1
0
```

Because:
- first receive → buffered value
- second receive → zero value (channel closed)

[Back to Top](#table-of-contents)

---

## ✅ Summary

Channels provide safe communication between goroutines.

Key concepts:
- channel basics
- unbuffered channels
- buffered channels
- channel directions
- closing channels
- range over channels

[Back to Top](#table-of-contents)
