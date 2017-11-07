[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Modelling using Differential Equations

* **Differential Equation** — an equation containing derivatives
* Solutions to a differential equation implies _finding a function_ that satisfies the differential equation
* **Independent variable**
	* Is the variable that is varied or manipulated
	* Is the presumed cause
	* Is the antecendent
* **Dependent variable**
	* Is the response to the variation or manipulation of the independent variable
	* Is the presumed effect
	* Is the consequent

## Example: Unconstrained Population Growth Model

* Differential Equation
	* `dP(t)/dt = rP(t)`
* Analytical Solution
	* `P(t) = P_0 e^{rt}`
* Logistic Equation
	* `dP(t)/dt = R(P(t))P(t)`
	* `dP(t)/dt = r(1 - P(t)/K)P(t)`
* And its corresponding analytical solution:
	* `P(t) = K / [ (K/P_0 - 1)e^{-rt} + 1 ]`

## Example: Lotka-Volterra Model

### The Variables

* `x = (number of fish at time t)`
* `y = (number of sharks at time t)`
* The change in populations are denoted by `δx` and `δy`
	* `δx = (fish born in isolation) - (fish deaths in isolation) - (fish eaten by sharks) - (fish caught by fishermen)`
	* `δy = (sharks born in isolation) - (shark deaths in isolation) + (surviving sharks that depend on fish) - (sharks caught by fishermen)`

### The Assumptions

* The change in shark and fish populations in isolation is proportional to the present population of sharks and fish respectively
* The number of sharks and fish caught by fishermen is directly proportional to the present population of the shark and fish population
* The number of fish eaten by fish is directly proportional to the product of the number of fishes present and the number of sharks present
* The additional number of sharks surviving is directly proportional to the number of fish eaten

### Modelling using Differential Equations

* Variables for fishes:
	* `b` as birth rate of fish
	* `d` as death rate of fish
	* `r = (b - d)` 
	* `α` as proportional constant of fish being eaten by shark
	* `f` as fraction of fish caught
* Rate of change of fishes:
	* `dx/dt = bx - dx - αxy - fx`
	* `dx/dt = (b - d - f)x - αxy`
	* `dx/dt = (r -f)x - αxy`
* Variables for sharks:
	* `b'` as birth rate of sharks
	* `d'` as death rate of sharks
	* `s = (b' - d')`
	* `β` as prop. constant of sharks surviving
	* `f` as fraction of shark caught (same as fish)
* Rate of change of sharks:
	* `dy/dt = b'y - d'y + βxy - fy`
	* `dy/dt = (b' - d' - f)y + βxy`
	* `dy/dt = (s - f)y + βxy`

### Steady-state (time-independent constant) solutions

* `dx/dt = (r -f)x - αxy` (fishes)
* `dy/dt = (s - f)y + βxy` (sharks)
* The rate of change with time of both equations is 0 at steady state, therefore:
	* `(r -f)x - αxy = 0`
	* `(s - f)y + βxy = 0`
	* Solutions are:
		* `(x,y) = (0,0)`
		* `(x,y) = (-(s-f)/β, (r-f)/α)`

### Insights

* Solution at origin `(0,0)` corresponds to extinction which is not necessary for this model
* At `(x,y) = (-(s-f)/β, (r-f)/α)`
	* Steady-state of fish depends on shark growth rate parameter `s` and not on fish growth rate paramter `r`
	* Vice-versa

## Phase-Plane Analysis

1. Write down the chain rule
2. Solve the differential equation for `x` and `y` and sketch the solution in the `(x,y)`-plane using the given initial values
3. Result is a graph/sketch of curves called **Phase-Plane Trajectories**

## Example: Guerilla Warfare

### Variables

* `x = (# of friendly soldiers at time t)`
* `y = (# of enemy soldiers at time t)`

### Assumptions

* Soldiers are approximated as continous variables
* Neither army take prisoners
* Both armies use only blasters to hit/kill the opponent

### Changes

* `δx = -(# of friendlies hit by enemies)`
* `δy = -(# of enemies hit by friendlies)`

### Considerations

* The number of soldiers hit in a small time interval `δt` equals the product of
	* The rate at which each solder shoots (`R_x` and `R_y`)
	* The probability that a single shot hits its target (`P_x` and `P_y`)
	* The number of soliders in combat

### Modelling

* `δx = -R_yP_y yδt`
* `δy = -R_xP_x xδt`
* Can we assume that the firing rates are constant? Yes.
* Can we assume that the probability of a single shot taking down opponent `P_x` and `P_y` are constant? No.
	* If the target is exposed (i.e. friendlies) it is reasonable to assume each shot (by the hidden enemy) has a constant probability of hitting its target, independent of the number of friendlies
	* If the target is hidden, then the probablity of a single shot fired at random into a given area (by the exposed army) hitting a soldier will depend on the concentration of hidden soldiers in the area `α` of thie hidden soldiers that is exposed: `P_x = αy/A`

### Differential Equations

* `dx/dt = -ay`
* `dy/dt = -bxy`
* Where the positive constants are:
	* `a = R_yP_y`
	* `b = αR_x/A`