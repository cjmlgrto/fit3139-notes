[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Dynamical Systems

* **Dynamical System** — any system evolving with time
* Each **state** of a dynamic system is time-dependent
* **Evolution Rule(s)** — guide the state changes in a dynamical system

## Example: Simple Population Growth

* Differential Equation
	* Let `P(t)` be the current population
	* `dP(t)/dt ∝ P(t)` → `dP(t)/dt = rP(t)`
	* `r` is `birth rate - death rate`
* Analytical Solution
	* `P(t) = P_0 × e^(rt)`
	* `P_0` is the population at `t=0`
* Difference Equation (aka Recurrence Relation)
	* `P(t + ∆t) = P(t) - { number dead } + { number born }`
	* `P(t + ∆t) = (1 + r∆t)P(t)`

## Steady-state Solutions

* When analyzing a dynamical system, a _solution_ refers to the state/quantity measured at some time-stamp as the system evolves from the initial conditions
* A solution in which the measured values _do not change with time_ is called a **steady-state solution**
* Let `x_1, x_2, x_3, ...` denote a sequence of quantities evolving over time
	* A **steady-state** solution for any sequence is achieved when `x_k` does not change with `k`
	* i.e. `x_k = x_{k+1} = x_{k+2} ...`

## Carrying Capacity

* **Carrying Capacity** — population growth limit
* Unconstrained growth:
	* `P_{t+1} = P_t + rP_t`
* Restricted groth:
	* `P_{t+1} = P_t + R(P_t)P_t`
	* Where `R(P_t)` is the growth rate, which is no longer a constant

Given the population example above, if there is a constrained growth rate due to overcrowding:

* Death rate increases and consequently, birth rate decreases, therefore `R(P_t)` decreases as `P_t` increases
* When `P_t` reaches carrying capacity (`K`) growth rate becomes `0`
* As `P_t → 0` the effects of overcrowding diminish and the growth rate tends towards the unconstrained growth rate (i.e. a _constant_, `r`)

## Discreate Logistic Model

* There are many choices for `R(P_t)` that could satisfy the conditions above
* The goal however is to choose the simplest function:
	* `R(P_t) = -(r/K)P_t + r`
	* Where `-(r/K)P_t` is the slope
	* And `r` is the intercept
* Substituting the above into the restricted growth difference equation gives us the **discrete logistic equation**
	* `P_{t+1} = P_t + rP_t(1 - P_{t}/K)`

### General Features of the Discrete Logistic Equation

* Steady-state when `P_{t+1} - P_t = 0 = rP_t(1 - P_{t}/K)`
* Steady-state solutions of `0` and `K`
* If `P_t < K` the total population will increase in the next time interval
* Otherwise, it will decrease
* `P_{t+1} - P_t ∝ r`
	* Small `r` gives steady growth to `K`
	* Large `r` gives oscillations above and below `K`

### Linearizing the non-linear Discrete Logistic Model

```
Start with the discrete logistic equation

P_{t+1} = P_t + rP_t(1 - P_{t}/K)

Divide both sides by K

P_{t+1}/K = P_{t}/K + rP_{t}/K(1 - P_{t}/K)

Denote P_{t}/K as X_t

X_{t+1} = X_t + rX_t(1 - X_t)

Since steady state is when P_t = K, the steady state for above is when X_t = 1

Denote

δ_t = X_t - 1

Substituting this, the above equation can be transformed to

δ_{t+1} = (1 - r)δ_t - r(δ_t)^2

We ignore the last term since it approaches 0 fast, so we get

δ_{t+1} ≈ (1 - r)δ_t 

Which has a closed form solution:

δ_{t+1} ≈ (1 - r)^{t+1} δ_0

Which implies

P_t ≈ K + K(1 - r)^{t+1} δ_0
```

### Other Models

* Logistic ecquation above can give negative numbers when `r > 3`
* Therefore the model is not suitable for populations `r > 3`
* A more complex model is the following:
	* `P_{t+1} = P_t × e^(1-P_{t}/K)`

## Coupled Models

* A **coupled system** is when one variable interacts with another variable in a dynamical system

### Example: Measles

#### Observations

* Anyone not exposed to measles is called _susceptible_
* Once caught, there is a _latent period_ (of about 1 week) where the person is _NOT contagious_ and exhibits _NO symptoms_
* After this, the person enters the _contagious_ period
* The person is now called _infective_ since it is possible for others to now catch the disease
* _Infective_ period lasts approximately 1 week
* After this, red spots appear on the skin for a few days after which they recover
* During this period and subsequently, the person is often _fully recovered_ and _immune_ (cannot be re-infected)

#### Assumptions to Formulate a Model

* The _latent_ period is _exactly 1 week_
* The _infective period_ is also _exactly 1 week_
* Let us suppose that all contact between infectives and susceptibles _only occur on weekends_
	* i.e. the number of infectives and susceptibles are constant over the rest of the week
* Infection rate:
	* assume a single infective infects a constant fraction `f` of the susceptibles
	* Changes to population size, via births and deaths:
		* Assume number of births `B` each week is _constant_
		* Assume _no deaths_

#### Formulating a (Simple) Model

* There is a time delay of _one week_ from when a susceptible first catches the disease until that person becomes _infective_
* There is a further delay of _one week_ before the infective becomes _recovered_
* This suggests a _difference equation model_ where time is measured _discretely_ in intervals/steps of one week

#### Establishing 'Variables' in a Difference Equation Model

* `I_k = ( number of contagious infectives present in k-th week )`
* `S_k = ( number of susceptibles present in k-th week )`
* `I_{k+1} = ( number of susceptibles who caught measles at the beginning of the k-th week)`
* `S_{k+1} = (number of susceptibles in k-th week) - I_{k+1} + (number of births during k-th week)`
* As such we get:
	* `I_{k+1} = (number of susceptibles who caught measles at the beginning of the k-th week) = fS_kI_k`
	* `S_{k+1} = S_k - fS_kI_k + B`

#### Steady-state Solutions

* Let `I_{k+1} = I_k = î` (steady state)
* Let `S_{k+1} = S_k = š` (steady state)
* Applying this to the difference equations, we get
	* `î = fšî`
	* `š = š - fšî + B`
* Or
	* `î(1 - fš) = 0`
	* `fšî - B = 0`
* Giving
	* `î = B`
	* `š = 1/f`

#### Discussion

* The difference equation `I_{k+1} = fS_kI_k` tells us that if `S_k < 1/f` then `I_{k+1}/I_k < 1`
* Also from the coupled difference equation `S_{k+1} = S_k - fS_kI_k + B`, for small `I_k`, `S_k` increases due to births until `S_k` eventually becomes greater than `1/f`
* At this stage, the number of infectives begins to increase again
* This signifies the important part played by the birt rate, as seen in the numerical simulations earlier (done during lectures)
* Also, one can see the advantage of keeping the number of susceptibles below `1/f`, through vaccination