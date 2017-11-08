[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Stochastic Modelling

* A **deterministic** system model is a model with no stochastic (i.e. random) components — every state of the variables is uniquely determined by the previous state of these variables
* A **stochastic** system model has inherent randomness, and the states each variable take are not described by unique values, but rather by probability distributions — i.e. stochastic models produce different states for the same given set of initial conditions

## Basics: Probability

* **Empirical (experimental) Probability**
	* If a random experiment is repeated `n` times and if `n_a` is the number of times an event `E` occurs, then the _relative frequency_ of occurence of event E is `n_a/n`
	* The _frequency theory of probability_ states that this relative frequency converges to the probability of _E_ as `n → ∞`
	* `Pr(E) lim_{n→∞} n_a/n`
* **Axiomatic (theoretical) Probability**
	* Relies on the mathematical construction of a _sample space_ with the associated probability function `Pr(E)` defined over all the events `E` in the space
	* e.g. rolling two diice and observing that some value is sum of two up faces
		* `Pr(X = 7) = 6/36`
		* `Pr(X = 12) = 1/36`
* **Random Variable**
	* A variable whose value results from a measurement on some type of random process
	* Can be _discrete_ or _continous_
* **Probability Density Function (PDF)**
	* Function of a continous variable
	* A function that describes the relative likelihood for a random variable to occur at a given point
* **Probability Mass Function**
	* A function that gives the probability that a discrete random variable which is exactly equal to some value

## Monte Carlo Simulation

### The Buffon Needle Problem

> Suppose that an infinite family of long lines are spaced one unit apart in the `xy`-plane. If a needle of length `r > 0` (and negligible width) is dropped at random onto the plane, what is the probability that it will land crossing at least one line?

* Let `u` be the distance from the center of the needle to the closest line
* Let `θ` be the angle that the needles makes from the lines
* Then a needle crosses a line if:
	* `u ≤ 0.5 sinθ`
* The Monte Carlo Approach involves executing the event `x` times and inferring the probability based on the results
* In this case, we would randomly generate values of `u` and `θ` a large amount of times, check if it satisfies the above criteria, then average out the results

## Random Number Generators

### Linear Congruential Uniform RNG

* `X_{n+1} ≡ (aX_n + c) mod m`
	* `X_n` is the sequence of pseudo random variates
	* `m` is the modulus
	* `0 < a < m` is the multiplier
	* `0 ≤ c < m` is the increment
* To generate a sequence of random numbers, start with an initial value (**seed**) `X_0` where `0 ≤ X_0 < m`

### Inverse Transform Method

* Let `C(X)` be an invertible _cumulative density function_ of a random variable `X` distrubuted over some probability distribution
* Sample a uniform random number between 0 and 1
* There exists a value `x` for the random variable `X` such that `C(x) = u`
* Compute `x` as `x = C^-1(u)`
* It can be proven that sampling `u`s uniformly between 0 and 1 and generating `x`s yields random numbers distributed according to the probability distribution of `X`
* For an exponential distribution:
	* For a random variable `X ∈ [0, ∞)` with a _rate_ parameter `λ`, the probability density function is:
		* `f(X = x; λ) = λ exp(-λx)`
	* The cumulative density function is:
		* `C(X ≤ x; λ) = 1 - exp(-λx)`
	* To generate a random number:
		* `x = -1/λ log(1 - u)` 
		* where `u` is a number between 0 and 1

### Samling from Arbitrary Distributions (Monte Carlo Rejection Sampling)

* To randomly sample from a distribution `f(x)`:
	* Choose a _proposal distribution_ `p(x)` (for example, normal distribution) such that `c * p(x) ≥ f(x)`
	* Given above, the rejection sampling works as follows:
		1. Generate a random sample `x_i` from `p(x)`
		2. Generate a uniform random number `y_i` from `[0, c * p(x_i)]`
		3. If `y_i ≤ f(x)` _accept_ `x_i` as a sample
		4. Else reject `x_i` and repeat from step 1

## Stochastic Simulation Example: Measles

* This is based off the problem introduced in [Dynamical Systems](https://github.com/cjmlgrto/fit3139-notes/blob/master/notes/06-dynamical_systems.md)
* It contained three categories of people:
	* Susceptibles (`S`)
	* Infectives (`I`)
	* Recovered (`R`)

### Assumptions

* Birth and death rates `β` and `δ` are proportional to the present population
* `β` and `δ` are treated to be common across all the three categories
* Each `I` infects a constant fraction `f` of `S`
* A constant fraction `α` of `I` recover from the infection

### ODE Model

* `dS/dt = β(S + I + R) - fSI - δS`
* `dI/dt = fSI - αI - δI`
* `dR/dt = αI - δR`

### A Case for Stochasticity

* In reality, we observe population fluctuations due to the random nature of the events involved
* Even if the probability associated with each event is fixed, individuals experience different chances of birth, infection, etc.
* Under the above assumptions, we have six _stochastic_ events to consider — the probability of these events is proportional to the
	* number of births: `β(S+I+R)`
	* number of deaths in `S`: `δS`
	* number of deaths in `I`: `δI`
	* number of deaths in `R`: `δR`
	* number of infections: `fSI`
	* number of recoveries: `αI`

### Stochastic Model

* The following _actions/updates_ are needed to the respective states when each of these stochastic evens occur:
	* When birth (either in `S`, `I` or `R`) occurs, then update `S = S + 1`
	* When death occurs in `S`, then update `S = S - 1`
	* When death occurs in `I`, then update `I = I - 1`
	* When death occurs in `R`, then update `R = R - 1`
	* When infection occurs, update `I = I + 1` and `S = S - 1`
	* When someone recovers, update `R = R + 1` and `I = I - 1`

## Gillespie's Algorithm

1. **Initialization**
	* Start the stochastic system with initialization of its variables to the initial values
2. **Monte Carlo Step**
	* Generate random numbers to determine the next event to occur as well as the time interval
	* The probability of a given event to be chosen is proportional to the current values of the variables
3. **Update**
	* Increase the time step of the simulation by the randomly generated time in Step 2
	* Update the number of some variables based on the event that was randomly chosen to have occurred
4. **Iterate**
	* Go back to step 2 unless simulation time has exceeded

### Applied to the Measles Model

1. **Initialization**
	* The measles model here has 6 stochastic events
	* Call them `e_1, ... , e_6`
	* For each event, determine the probability of its occurence
	* Denote these as `p_1, ... , p_6`
2. **Monte Carlo Step**
	* For each of these six events, calculate which event happens the earliest (this is the Monte Carlo part)
	* One way is to compute, for each event `e_i`, the time interval of next occurence as `t_i = (-logu)/p_i` where `u ∈ uniform(0,1)`
	* Compute the event `E` that happens the earliest, i.e. at `t = min_{1≤i≤6}(t_i)`
3. **Update**
	* Update time form current `t_curr` to `t_prev + t` and the required action for the chosen event `E` when `e` is performed
4. **Iterate**
	* Repeat from step 2 until the time period of the simulation has exceeded
