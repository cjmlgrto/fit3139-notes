[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Non-Linear Equations

* Non-linear equations cannot be solved in a finite number of steps
* One resorts to iterative methods that produces increasingly accurate approximation to a solution
* The process terminates once the result is "sufficiently" accurate
* **Convergence**
	* Let the error at some `k`-th iteration be denoted by `e_k = x_k - x*` where `x*` is the true solution and `x_k` is the approximate answer at that iteration
	* An iterative numberical algorithm has a convergence rate of `r` if:
		* `lim_{k → ∞} |e_{k+1}| ÷ |e_k|^r = constant`
	* Where `e_k` is the error observed in the `k`-th step

## Interval Bisection Method

1. Stat with an initial bracket `[lower, upper]` such that `Sign(f(L)) ≠ Sign(f(U))`
2. Iteratively reduce the length until the true solution has been approached as much as the arithmetic precision will permit
3. At each iteration, evaluate the function at the midpoint of the bracketed interval and discard half the interval

### Advantages

* Error is bounded by the interval considered at each step
* Always converges at `O(log n)` rate

### Disadvantages

* Works only for a non-linear function `f` which is real and continous within the initial bracket
* Makes no use of the magntitudes of the function values, uses only the signs (hence convergence is slow)
* Cannot find roots in some equations
	* e.g. `f(x) = 1/x` or `f(x) = x^2 + a, a ≥ 0`

## Newton's Method

```
x_{i+1} = x_i = f(x_i)/f'(x_i)
```

1. Set `i = 0` and choose an initial value `x_0`
2. Find a result to the equation above
3. Find `|e_a| = |x_{i+1}-x_i / x_i| * 100`
4. If `|e_a| < tol` then stop
5. Else, repeat from step 2 using `x_i = x_{i+1}` and `i = i+1`

### Convergence

* Converges at a quadratic rate to the root

### Issues

* Relies on being able to compute the derivative (symbolically or otherwise)
* Division by zero (when `f'(x) = 0`)
* Root jumping (e.g. `f(x) = sin(x)`)

## Secant Method

```
x_{i+1} = x_i - [f(x_i)(x_i - x_{i-1}) / f(x_i) - f(x_{i-1}) ]
```

1. Set `i = 1` and choose two initial values `x_0` and `x_1`
2. Find a solution to the above equation
3. Find `|e_a| = |x_{i+1}-x_i / x_i| * 100`
4. If `|e_a| < tol` then stop
5. Else, repeat from step 2 using `x_i = x_{i+1}` and `i = i+1`

Essentially, the same as Newton's method but involves finding the difference between the function's results at the difference of the two previous terms

### Advantages

* Converges superlinearly
* Only one new function evaluation per iteration
* In comparison to Newton's method, low cost per iteration more than offsets the (relatively) larger number of iterations required for convergence