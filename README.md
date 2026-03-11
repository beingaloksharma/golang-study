<div align="center">
  <h1>Golang Study Guide 🚀</h1>
  <p>A comprehensive collection of Go exercises, algorithms, and data structures implemented with performance analysis. This repository serves as a study guide for mastering Golang concepts, from basics to concurrency.</p>
  
  <p>
    <img alt="Go Version" src="https://img.shields.io/badge/Go-1.21%2B-blue?style=flat&logo=go" />
    <img alt="License" src="https://img.shields.io/badge/License-MIT-green?style=flat" />
    <img alt="Status" src="https://img.shields.io/badge/Status-Active-success?style=flat" />
  </p>
</div>

---

## 📚 Study Topics

This repository is organized by concepts to make learning Golang structured and efficient.

### 🧠 Core Concepts & Algorithms
- **[Basics & Algorithms](basics_and_algorithms.md)**: Reverse strings/numbers, Palindromes, Primes, Recursion, Length of Last Word, Valid Parentheses, Sorted Squares.
- **[Interfaces](interfaces.md)**: Polymorphism, Geometry Shape Pattern, Payment Method Factory Pattern.
- **[Functions & Return Values](return_values.md)**: Unnamed vs Named Return Values, Initialization concepts.
- **[Closures](closures.md)**: Anonymous functions, variable capture, escape analysis, and loop closure caveats.
- **[Structs](structs.md)**: Struct declaration, initialization, nesting, JSON tags, and memory layout.
- **[Methods](methods.md)**: Methods in Go, Value vs Pointer receivers, method sets, and method chaining.

### 🏗 Data Structures
- **[Arrays](array.md)**: Complete guide on arrays, sizes, contiguity, and memory behavior.
- **[Slices](slice.md)**: Dynamic arrays, slice internals, `make`, `append`, `len`/`cap`, memory layout, and `copy`.
- **[Data Structures Implementation](data_structures.md)**: Counting vowels/consonants, array reversal, removing duplicates, custom sorting.
- **[Memory Allocation with make()](make_slice_map.md)**: Slice vs Map allocation, capacity, and `make()` function internals.
- **[Slice Append Internals](slice_append.md)**: How `append()` truly works, capacity, array reuse vs reallocation, and common interview traps.

### ⚡ Concurrency
- **[Channels](channels.md)**: Channel basics, unbuffered vs buffered, directions, closing channels, range over channels, and common traps.
- **[Concurrency Basics & Advanced Patterns](concurrency.md)**: Goroutines, Map-Reduce, WaitGroups, Mutexes, Atomic Counters, Worker Pools, Concurrent Caches, and Merge Sort.

---

## 🚀 Getting Started

### Prerequisites
Make sure you have Go installed on your machine (version 1.21+ recommended).
```bash
go version
```

### Running the Exercises
Most exercises are self-contained. You can copy the code from the respective study guides and run them locally using:
```bash
go run main.go
```

Alternatively, you can explore the repository structure and run specific `.go` files if introduced.

---

## 🤝 Contribution
Contributions, issues, and feature requests are welcome!

## 📝 License
This project is [MIT](https://opensource.org/licenses/MIT) licensed.

