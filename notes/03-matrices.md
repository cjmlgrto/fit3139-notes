[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Matrices

## Basics

* **Column Space** — set of all possible linear combinations of its column vectors
* **Linear Independence** — if a set of matrix's vectors cannot be constructed as a linear combination of other vectors in the set
* **Rank** — maximum number of linearly independent column vectors
* **Dot Product** — `A·B = |A||B|cosθ` or the sum of multiplying each element in the same position of both matrices
* **Multiplication Properties**
	* `AB ≠ BA`
	* `A(BC) = (AB)C`
	* `(A+B)C = AC + BC`
	* `transpose(AB) = transpose(B)transpose(A)`
	* If `AB = BA = I` where `I` is the **Identity Matrix**, then `B` is the inverse of A
* **Nonsingular** — a matrix `A` is non-singular if:
	* `A` is invertible
	* `determinant(A) ≠ 0`
	* `rank(A) = n`
* **Consistent** — a matrix `A` is consistent if:
	* Given `Ax = b`
	* Does `b` lie in the **span** if `A`?
	* i.e. Can `b` be expressed as a linear combination of columns in the matrix `A`?
* **Properties of Triangular Matrices**
	* The **sum** of two lower (or upper) triangular matrices results in a lower (or correspondingly, upper) triangular matrix
	* The **product** of two triangular matrices result in triangular matrices
	* The **inverse** does the same thing

## Modelling using Linear Systems

* A linear relationship can be expressed by a _linear transformation_ (i.e. a matrix, `A`) that relates a _vector of 'causes'_ (`x`) to a _vector of 'effects'_ (`b`)
	* `Ax = b`
* Possibilities of solution space
	* **Unique** if A is nonsingular
	* **Infinite** if A is singular and b is an element of `span(A)`
	* **No Solutions** if A is singular and b is not an element of `span(A)`

## Solving Linear Equations with Gaussian Elimination

### Basic Idea

Given `Ax = b`

1. **Forward Elimination** — transform `A` into an upper-triangular matrix
2. **Back Substitution** — substitute in each row of `A` back into itself

### Elementary Elimination Matrices

* Below is an example of an elementary elimination matrix
* Notice that `a1` a common term that appears as the denominator — this is known as the **pivot**

```
|1      0 0| |a1|    |a1|
|-a2/a1 1 0| |a2|  = |0 |
|-a3/a1 0 1| |a3|    |0 |
```

### Naive Forward Elimination

* To eliminate terms below the _first_ diagonal element (first column) of `A`, we multiply both sides by a transformation `M = a_i/a_k` where `a_k` is the pivot
* Repeat for the next column in the next row
* Continue until `A` becomes an upper-triangular matrix

An example:

```
Ax = b
|1 2 2||x1|   |3 |
|4 4 2||x2| = |6 |
|4 6 4||x3|   |10|

Subtract four times the first row from each of the second and third rows

M_1 × A
|1  0 0||1 2 2|   |1  2  2|
|-4 1 0||4 4 2| = |0 -4 -6|
|-4 0 1||4 6 4|   |0 -2 -4|

Do the same for the other side of the equation

M_1 × b
|1  0 0||3 |   | 3|
|-4 1 0||3 | = |-6|
|-4 0 1||10|   |-2|

Then, for the next row, we subtract 0.5 times the second row from the third row (doing the same for b)

M_2 × A
|1  0   0||1  2  2|   |1  2  2|
|0  1   0||0 -4 -6| = |0 -4 -6|
|0 -0.5 1||0 -2 -4|   |0  0 -1|

M_2 × b
|1  0   0|| 3|   | 3|
|0  1   0||-6| = |-6|
|0 -0.5 1||-2|   | 1|

And as such, we now have an upper triangle matrix:

|1  2  2||x1|   | 3|
|0 -4 -6||x2| = |-6|
|0  0 -1||x3|   | 1|

With which we can now perform back-substitution
```

## LU Factorization

```
|a11 a12 a13 a14||x1|   |b1|
|a21 a22 a23 a24||x2| = |b2|
|a31 a32 a33 a34||x3|   |b3|
|a41 a42 a43 a44||x4|   |b4|

Forward elimination gives us:

|u11 u12 u13 u14||x1|   |y1|                  |b1|
|0   u22 u23 u24||x2| = |y2| = M3 × M2 × M1 × |b2|
|0   0   u33 u34||x3|   |y3|                  |b3|
|0   0   0   u44||x4|   |y4|                  |b4|

Pre-multiply inverse of the elimination matrices:

                        |u11 u12 u13 u14||x1|   |b1|
M1^-1 × M2^-1 × M3^-1 × |0   u22 u23 u24||x2| = |b2|
                        |0   0   u33 u34||x3|   |b3|
                        |0   0   0   u44||x4|   |b4|
                        
Which is equivalent to:

|l11 0   0   0  ||u11 u12 u13 u14||x1|   |b1|
|l21 l22 0   0  ||0   u22 u23 u24||x2| = |b2|
|l31 l32 l33 0  ||0   0   u33 u34||x3|   |b3|
|l41 l42 l43 l44||0   0   0   u44||x4|   |b4|

Then becomes:

|l11 0   0   0  ||y1|   |b1|
|l21 l22 0   0  ||y2| = |b2|
|l31 l32 l33 0  ||y3|   |b3|
|l41 l42 l43 l44||y4|   |b4|

Then y1, y2, y3, y4 can be solved using forward substitution
```

Note that in the example above, `U` is the upper-triangular matrix formed by row-reducing `A` and `L` is the lower-triangular matrix formed by the product of the inverses of the row reduction transformations.

## Problems with Gaussian Elimination/LU Factorization

* Division by zero
* Round-off error
* Smaller pivots produce larger multipliers and hence larger errors
* Larger pivots produce smaller multipliers and hence smaller errors
* In a column, if you use the largest entry on or below the diagonal as the pivot, then the multipliers are bounded by 1

## Over-determined Systems

* There may be no solution for `x` that solves `Ax = b`
* So we look for an approximate solution in the span defined by `A`
* i.e. try to solve the system for some `k` such that `Ak` (which is a vector in the span of `A`) is a close approximation of `b`
* **Linear Least-Squares Problem**:
	* Given an **over-determined** system of linear equations `Ax = b`
	* Find a `k` such that `||e||^2` is a minimum, where `e = b - Ak`

### Normal Equation to solve the Linear Least-Squares Problem

By the construction of the least-squares problem to solve over-determined systems:

* The error vector `e` is perpendicular to the vector `Ak`
* The error vector `e` is in fact, perpendicular to any vector in the span of `A`
* This is to say that the _dot product_ between `e` and any vector in the span of `A` will be zero
* This implies:

```
transpose(A) × e = 0
transpose(A) × (b - Ak) = 0
transpose(A)×b - transpose(A)×Ak = 0
transpose(A)×Ak = transpose(A)b
```