[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Linear Programming

**Linear Programming** is a method that allows to determine how to best operate some system optimally under the constraints of the available resource

## Modelling a Problem with Linear Programming

### Problem

* A small manufacturer makes two products `A` and `B`
* Each of these products require 2 resources, `R1` and `R2`
* Each unit of `A` requires 1 unit of `R1` and 3 units of `R2`
* Each unit of `B` requires 1 unit of `R1` and 2 units of `R2`
* But the manufacturer only has 5 units of `R1` and 12 units of `R2`
* On every unit of `A` sold the manufacturer makes $6 profit
* On every unit of `B` sold the manufacturer makes $5 profit
* How many units of the product `A` and `B` should the manufacturer produce to _maximize profit_?

### Formalism

* Decision variables
	* `x` = number of units of product `A`
	* `y` = number of units of product `B`
* Objective function
	* `maximize(profit) z = 6x + 5y`
* Constraints
	* `x + y ≤ 5`
	* `3x + 2y ≤ 12`
	* `x,y ≥ 0`

## Linear Programming problem in its Canonical Form

* Maximise
	* `z = c1x1 + c2x2 + ... + cnxn`
* Subject to the constraints
	* `a11x1 + a12x2 + ... + a1nxn ≤ b1`
	* `a21x1 + a22x2 + ... + a2nxn ≤ b2`
	* `...`
	* `am1x1 + am2x2 + ... + amnxn ≤ bm`
* And
	* `x1, x2, ... , xn ≥ 0`

## Graphical Solutions

* A graphical solution (graphing the constraints) is applicable in practice to solutions involving 2 (or even 3) decision variables — otherwise it is _practically useless_
* **Feasible Region** — region where the given linear program has feasible solutions satisfying all its constraints
* For any point in the feasible region, there is always a point on the boundary of the feasible region that will _dominate_ it
	* i.e. give a better evaluation of the objective function
* Among all points on the boundary of the feasible regions, there is at least one corner point of the feasible region that will always dominate, given the objective function

## Algebraic Solution

* The first step is to conver the _inequality_ constraints into _equality_ constraints — this is achieved by adding _slack variables_
* First constraint: `x + y ≤ 5` becomes
	* `x + y + s = 5`
	* `s ≥ 0`
* Second constraint: `3x + 2y ≤ 12` becomes
	* `3x + 2y + t = 12`
	* `t ≥ 0`
* Objective remains the same:
	* `maximize(profit) z = 6x + 5y`
* 4 variables, 2 equations — a solution can be found for 2 variables if other 2 variables are fixed

| Fixed pair | Solve for |
| ---        | ---       |
| x,y        | s, t      |
| x,s        | y, t      |
| x,t        | y, s      |
| y,s        | x, t      |
| y,t        | x, s      |
| s,t        | x, y      |

* Given the above chart, if we fix the left column to zeroes, we get the chart below
* Further, values can be substituted to find a value for the objective

| Set to zero | Solutions     | Value of Objective |
| ---         | ---           | ---                |
| x,y         | s = 5, t = 12 | z = 0              |
| x,s         | y = 5, t = 2  | z = 25             |
| x,t         | y = 6, s = -1 | Infeasible         |
| y,s         | x = 5, t = -3 | Infeasible         |
| y,t         | x = 4, s = 1  | z = 24             |
| s,t         | x = 2, y = 3  | z = 27             |

* **Non-basic variables** are those that lie outside the _basis_ of the linear programming problem — _they are the variables that are set to zero_
* **Basic variables** are those within the basis of the linear programming problem

### Complexity of the Algebraic Solution

* `N` decision variables
* `m` linear constraints
* `m` slack variables for `m` linear constraints
* Assign `N` out of `N+m` variables to 0, then solve for the remaining `m` variables
* A total of `^{N+M}C_N` possibilities
* Substitute the values of that `N` decision variables taken into the objective function
* Once all possibilities are explored, choose the optimum over all the solutions
* As such, we get `O(N!)` complexity

### Disadvantages of the Algebraic Solution

* Grows combinatorially for large `N` (factorial complexity)
* Evaluates infeasible solutions
* Does not say if optima has been reached — the search is exhaustive (brute force)
* Does not progressively give better solutions (in terms of objective function) — the search is random

## Simplex Method

* Polynomial time
* Does not evaluate infeasible solutions
* Tells when optima has been reached and stops
* Explores progressively better solutions

### Algebraic Form of Simplex Method

1. Choose two non-basic variables and set them to 0
2. Re-write the constraints in terms of the basic variables
3. Given the objective function, modify one of the non-basic variables (increase or decrease) such that:
	* The objective increases when either is modified
	* Decrease the non-basic variables without making them go past 0 (do not modify if they tend to become negative)
4. Modify all equations to take on the new value for the non-basic variables (and notice that some variables become non-basic)
5. Repeat from step 2 until any of the non-basic variables cannot be further modified


#### Example

* Start by choosing `x` and `y` as the non-basic variables (i.e. variables set to 0)
* This gives solutions to `s` and `t` as `s = 5` and `t = 12`
* **First iteration**: re-write the constraints in terms of `s` and `t` (now basic variables)
	* `s = 5 - x - y`
	* `t = 12 - 3x - 2y`
* This gives us the same problem before, but re-organised to this form:
	* Maximize `z = 6x + 5y`
	* Subject to constraints
		* `s = 5 - x - y`
		* `t = 12 - 3x - 2y`
		* `x,y,s,t ≥ 0`
* Recall currently `x,y = 0`
* Increase `x` or `y` one at a time
	* Choosing `x`, we can increase it from `[0,5]` according to the first constraint
	* Or choose `x` from `[0,4]` according to the second constraint
	* Choosing the minimum of `x = 5` and `x = 4` will ensure that both `s, t ≥ 0`
* Since we have chosen the minimum increase of `x` from `x = 0` to `x = 4`, `x` enters the basis while `t` exists the basis (`x` = 4 implies `t = 0`)
* The new set of non-basic variables are therefore `y` and `t` and basic variables are `x` and `s`
* **Second iteration**: re-write the constraints in terms of the new basic variables
	* `x = 4 - 2y/3 -t/3`
	* `s = 1 - y/3 + t/3`
	* `z = 24 + y - 2t`
* Previously, `z=0` (with `x` and `y` as the non-basic) — now `z = 24` when `y=0,t=0`
* `z` can be increased further if:
	* `y` can be increased (since coefficient in the objective is positive)
	* `t` can be decreased (since coefficient in the objective is negative)
	* Decreasting `t` is infeasible becuse `t` is already zero
	* So the only choice is to increase `y`
	* However, `y` can only be increased from `y=0` to `y=3` (based on the secon equation) for the solution to be feasible
* Since we have chosen to increase `y`, the new basic variables are `x = 2` and `y=3` and the new non-basic variables are `s=0, t=0`
* **Third iteration**: re-write the constraints in terms of the new basic variables
	* `x = 2 + 2s - t`
	* `y = 3 - 3s + t`
	* `z = 27 - 3s - t`
* Notice that `z` can't be increased any further because:
	* To increase `z` we have to decrease `s` but we can't since `s = 0` (same for `t`)
* STOP! As such, we have now found the best value for `z`

### Tableau Form of Simplex Method

Same as above, albeit tabulating every iteration.