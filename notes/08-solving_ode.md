[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# First-Order Ordinary Differential Equations

* **Ordinary Differential Equation (ODE)** — an equation that contains only ordinary derivatives with respect to a single independent variable
	* e.g. `15 d^2y/dx^2 + 21 dy/dx + 8y = 0`
* **Order** of an ordinary differential equation is determined by the order of the highest appearing derivative
* **Degree** of an ODE is the power to which the highest-order derivative is raised
* For a **Linear** ODE:
	* The derivatives and the dependent variable `y` are of the first degree
	* The coefficient (i.e. functions before each derivative) depend only on the independent variable, `x`
	* The dependent variable `y` is _not_ a result of a transcendental function
* **Slope (or directional) field** — graphical representation of first-order ODEs
* **Initial Value Problem** — involves an ODE with a specified value, called the _initial condition_, of an unknown function at a given point in the domain of the solution

## Solving 1st-Order ODE Initial Value Problems

* Given an ODE of the form:
	* `y' = dy/dx = f(x,y)`
	* With initial values `(x_0, y_0)` where `y(x_0) = y_0`

### Euler's Method

```
y_{i+1} = y_i + f(x_i, y_i)h

h = (x_{i+1} - x_i)
```

1. Set `i = 0`
2. Set step size `h`
3. Start with the initial values `x_0` and `y_0 = y(x_0)`
4. Use the above formula
5. Set `i = i+1` and repeat

### Range-Kutta-2 (Predictor-Corrector) Method

```
y_{i+1} = y_i + (ak_1 + bk_2)h

k_1 = f(x_i, y_i)
k_2 = f(x_i + αh, y_i + βk_1h)

a + b = 1
bα = 1/2
bβ = 1/2
```

* Range-Kutta-2 (meaning _second order_) comes from the idea of truncating the Taylor series after first three terms in the RHS
* However, `f'(x_i, y_i)` is an inconvenient term as it requires the evaluation of the derivative symbolically — so it is approximated as above
* The methods that follow are special cases of the RK2 general form above

### Heun's Method

```
b = 1/2

a + b = 1 → a = 1/2
bα = 1/2 → α = 1
bβ = 1/2 → β = 1

y_{i+1} = y_i + (0.5k_1 + 0.5k_2)h
k_1 = f(x_i, y_i)
k_2 = f(x_i + h, y_i + k_1h)
```

### Midpoint Method

```
b = 1

a + b = 1 → a = 0
bα = 1/2 → α = 1/2
bβ = 1/2 → β = 1/2

y_{i+1} = y_i + (k_2)h
k_1 = f(x_i, y_i)
k_2 = f(x_i + h/2, y_i + k_{1}h/2)
```

### Ralston's Method

```
b = 2/3

a + b = 1 → a = 1/3
bα = 1/2 → α = 3/4
bβ = 1/2 → β = 3/4

y_{i+1} = y_i + (1/3k_1 + 2/3k_2)h
k_1 = f(x_i, y_i)
k_2 = f(x_i + 3h/4, y_i + 3k_{1}h/4)
```