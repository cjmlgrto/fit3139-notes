[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Eigenvalues and Eignvectors

Given an square matrix `A` representing some linear transformation, we can find a non-zero vector `x` and a scalar `λ` such that `Ax = λx`. To solve this:

```
Ax = λx
Ax - λx = 0
(A-λI)x = 0 

where I is the identity 
and (A-λI) must be singular (its determinant = 0)
```

* The **eigenvalues** of a matrix are found by solving for `λ` given `det(A-λI) = 0`.
* The **eigenvectors** are found by substituting back each eigenvalue back into the above linear equation
* A column vector formed by the diagonal of each eigenvector is a vector that can satisfy the equation `(A-λI)x = 0`

## Diagonal Factorization (Eigen Decomposition)

* **Eigen decomposition** gives a factorizaton of a given matrix into a form that is in terms of its eigenvalues and eigenvectors
* **Eigen Decomposition Theorem**
	* Any `n×n` non-singular matrix `A` can be factorized as
		* `A = SΛS^-1`
	* Where
		* `S` is an n×n` matrix of eigenvectors (as its columns)
		* `Λ` is a corresponding diagonal matrix of eigenvalues (as its diagonal elements)
* Eigen decomposition allows easy computation of **matrix exponentiation**:

```
A^2 = (A)(A) = (SΛS^-1)(SΛS^-1) = SΛ(S^-1S)ΛS^-1 = SΛ^2S-1
...
A^n = SΛ^nS^-1
```
