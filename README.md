# math

A mathematical and numerical computing library for [Bend](https://github.com/HigherOrderCO/Bend).

`math` provides reusable mathematical primitives, vector operations, linear algebra utilities, statistics, and eventually parallel numerical algorithms designed around Bend's execution model.

The project is also an exploration of numerical methods, floating-point computation, parallel algorithms, and GPU-oriented programming.

## Goals

The library will evolve incrementally:

```text
Scalar Math
    ↓
Vectors
    ↓
Matrices
    ↓
Linear Algebra
    ↓
Statistics
    ↓
Parallel Algorithms
    ↓
GPU Numerical Computing
```

The initial goal is not to reproduce large numerical ecosystems such as NumPy.

The goal is to build a small, understandable, and reliable mathematical foundation for Bend.

## Roadmap

### v0.1 — Basic

Basic mathematical operations for `F32`.

* [ ] `abs`
* [ ] `min`
* [ ] `max`
* [ ] `clamp`
* [ ] `sqrt`
* [ ] `pow`

### v0.2 — Constants, Exponential and Trigonometric Functions

Constants:

* [ ] `PI`
* [ ] `E`
* [ ] `TAU`

Exponential and logarithmic functions:

* [ ] `exp`
* [ ] `ln`
* [ ] `log`
* [ ] `log10`

Trigonometric functions:

* [ ] `sin`
* [ ] `cos`
* [ ] `tan`
* [ ] `asin`
* [ ] `acos`
* [ ] `atan`
* [ ] `atan2`

### v0.3 — Vectors

Initial vector types:

* [ ] `Vec2`
* [ ] `Vec3`
* [ ] `Vec4`

Operations:

* [ ] addition
* [ ] subtraction
* [ ] scalar multiplication
* [ ] dot product
* [ ] cross product
* [ ] magnitude
* [ ] normalization
* [ ] distance

### v0.4 — Matrices

* [ ] matrix representation
* [ ] matrix addition
* [ ] matrix multiplication
* [ ] transpose
* [ ] matrix-vector multiplication

### v0.5 — Statistics

* [ ] sum
* [ ] mean
* [ ] variance
* [ ] standard deviation
* [ ] min/max reductions

### Future

Future areas of exploration include:

* tree reductions
* parallel dot products
* parallel matrix multiplication
* GPU execution
* numerical benchmarks
* tensors
* ML-oriented primitives
* optimization algorithms

## Project Structure

```text
math/
├── core/
│   ├── basic.bend
│   ├── constants.bend
│   ├── exp.bend
│   └── trig.bend
│
├── linear/
│   ├── vec2.bend
│   ├── vec3.bend
│   ├── vec4.bend
│   └── matrix.bend
│
├── stats/
│   └── stats.bend
│
├── tests/
│
├── main.bend
├── LICENSE
└── README.md
```

The structure may change as the library and the Bend ecosystem evolve.

## Usage

Import the package as `Math`:

```bend
import Base
import math as Math

def main() -> F32:
  Math.sqrt(25.0)
```

Expected result:

```text
5.0
```

Other functions follow the same public API style:

```bend
Math.abs(-5.0)

Math.min(5.0, 10.0)
Math.max(5.0, 10.0)
Math.clamp(15.0, 0.0, 10.0)

Math.sqrt(25.0)
Math.pow(2.0, 8.0)

Math.sin(1.0)
Math.cos(1.0)

Math.exp(2.0)
Math.log(10.0)
```

The internal implementation remains organized into separate modules while `main.bend` defines the public interface exposed by the package.

Conceptually:

```text
                  math
                    │
                    ▼
                main.bend
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
      core        linear       stats
        │
   ┌────┼────┬────┐
   ▼    ▼    ▼    ▼
 basic constants exp trig
```

This allows the internal structure to evolve without unnecessarily changing the public API.

## Numerical Methods

Functions should not be treated as black boxes.

Whenever possible, implementations should document the mathematical method used internally.

For example, square root can be approximated using Newton's method.

Given:

```text
x² = n
```

an approximation can be iteratively improved using:

```text
xₙ₊₁ = 1/2 × (xₙ + n/xₙ)
```

For `sqrt(25)`, successive approximations converge toward:

```text
25
13
7.46
5.40
5.01
5.00
```

Understanding how these algorithms work is part of the purpose of this project.

## Numerical Accuracy

Floating-point calculations are approximations.

Implementations should therefore document relevant properties such as:

* precision
* convergence behavior
* numerical stability
* valid input range
* edge cases
* approximation error

Tests should avoid strict floating-point equality when an error tolerance is more appropriate.

For example:

```text
abs(expected - result) < epsilon
```

## Parallelism

One of the main goals of `math` is to investigate how numerical algorithms can take advantage of Bend's parallel execution model.

A sequential reduction can be expressed as:

```text
((((a + b) + c) + d) + e)
```

which has approximately linear dependency depth:

```text
O(n)
```

A tree reduction instead groups independent operations:

```text
[a b c d e f g h]
        ↓
[a+b c+d e+f g+h]
        ↓
[a+b+c+d e+f+g+h]
        ↓
[result]
```

This reduces the dependency depth to approximately:

```text
O(log n)
```

and exposes more independent work to the runtime.

This becomes particularly important for operations such as:

* vector reductions
* dot products
* matrix operations
* statistics
* tensor operations

## Philosophy

`math` follows four principles.

### Understand the math

Every important algorithm should be implemented with an understanding of the mathematics behind it.

### Measure correctness

Results should be validated against established implementations such as Python's `math` module and NumPy.

### Design for parallelism

Algorithms should take advantage of Bend's execution model whenever doing so makes sense.

The goal is not to copy sequential implementations from other languages unchanged.

### Keep the foundation small

The mathematical foundation should remain small and understandable.

Complex functionality should be built from simple and well-tested mathematical primitives.

## Testing

Implementations should be compared against known numerical results.

A reference implementation may use Python:

```python
import math

print(math.sqrt(25.0))
print(math.sin(1.0))
print(math.exp(1.0))
```

For larger numerical operations, NumPy can be used as a reference:

```python
import numpy as np

a = np.array([1.0, 2.0, 3.0])
b = np.array([4.0, 5.0, 6.0])

print(np.dot(a, b))
```

The purpose of these comparisons is correctness validation, not API compatibility.

## Contributing

Contributions, experiments, benchmarks, numerical algorithms, and discussions are welcome.

When implementing a mathematical function, preferably document:

1. the mathematical definition;
2. the algorithm used;
3. the expected numerical precision;
4. relevant edge cases;
5. tests against a reference implementation;
6. potential parallelization strategies.

For algorithms with multiple known implementations, explain why a particular approach was selected.

## Status

Experimental.

Both Bend and `math` are evolving projects, so APIs and internal representations may change between releases.

## License

MIT

