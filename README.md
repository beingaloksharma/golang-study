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

## 📌 Table of Contents
- [Study Topics](#-study-topics)
  - [Core Concepts & Algorithms](#-core-concepts--algorithms)
  - [Data Structures](#-data-structures)
  - [Concurrency](#-concurrency)
- [📂 Project Structure](#-project-structure)
- [🚀 Getting Started](#-getting-started)
- [🤝 Contribution](#-contribution)
- [📝 License](#-license)

---

## 📚 Study Topics

This repository is organized by concepts to make learning Golang structured and efficient.

### 🧠 Core Concepts & Algorithms
| Topic | Description | Link |
| :--- | :--- | :--- |
| **Basics** | Strings, Palindromes, Primes, Recursion | [basics_and_algorithms.md](basics_and_algorithms.md) |
| **Interfaces** | Polymorphism, Factory Patterns | [interfaces.md](interfaces.md) |
| **Functions** | Return Values, Initialization | [return_values.md](return_values.md) |
| **Closures** | Variable capture, Loop caveats | [closures.md](closures.md) |
| **Structs** | Initialization, Nesting, JSON, Memory | [structs.md](structs.md) |
| **Methods** | Value vs Pointer receivers | [methods.md](methods.md) |
| **Escape Analysis** | Stack vs Heap, Performance | [escape_analysis.md](escape_analysis.md) |

### 🏗 Data Structures
| Topic | Description | Link |
| :--- | :--- | :--- |
| **Arrays** | Fixed-size collections, Memory layout | [array.md](array.md) |
| **Slices** | Dynamic arrays, Internals, Capacity | [slice.md](slice.md) |
| **Slice Append** | `append()` internals, Reallocation | [slice_append.md](slice_append.md) |
| **Memory Allocation** | `make()` for Slices and Maps | [make_slice_map.md](make_slice_map.md) |
| **Algorithms** | Vowel counting, Sorting, Reversal | [data_structures.md](data_structures.md) |

### ⚡ Concurrency
| Topic | Description | Link |
| :--- | :--- | :--- |
| **Channels** | Buffered vs Unbuffered, Select, Range | [channels.md](channels.md) |
| **Patterns** | Worker Pools, Mutexes, Atomic Counters | [concurrency.md](concurrency.md) |

---

## 📂 Project Structure

This project follows a flat structure where each markdown file is a self-contained study guide with code examples and analysis.

- `main.go`: Principal playground for running concurrency examples.
- `*.md`: Detailed study guides for specific Go topics.
- `go.mod`: Module definition.

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
