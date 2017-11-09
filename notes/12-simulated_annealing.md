[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Simulated Annealing

## The Boltzmann Distribution

* At a fixed temperature `T` the distribution of energy over all possible states/configurations of molecules follows a Boltzmann distribution
	* `Pr(E = E_a) ∝ exp(-E_a/RT)`
	* `R` is the Boltzmann constant `1.381 × 10^-23`

## Metropolis Monte Carlo Method

1. Given is a certain state of a solid, characterized by the positions of its particles
2. Generate a random perturbation by applying a small displacement of a randomly chosen particle
3. If the difference in energy, `∆E`, between the slightly perturbed state and the current state is _negative_ (i.e. perturbed state MORE favourable than the current) then the perturbation is _accepted_ and the process in the above step is continued with the new state
4. However, if `∆E > 0` (i.e. perturbed state is less favourable than the current one) then accept this state with a probability — this is given by the ratio of probabilities for the perturbed vs current state, given by `exp(-∆E/RT)` — this is known as the _Metropolis Criterion_
	* Following the Metropolis criterion, the system eventually evolves into thermal equilibrium
	* That is, after a large number of perturbations, the probability distribution of the states approaches the Boltzmann distribution
	* Once the thermal equilibrium is reached, restart this Monte Carlo method with a slightly lowered temperature
	* Repeat this process until some _miniumum_ threshold of temperature `T_min` is reached

## Simulated Annealing to solve Optimization Problems

| Thermodynamical Analogy | Optimization Problem |
| --- | --- |
| States/configurations of molecules | Potential/candidate solutions |
| Energy of a given state/configuration | Value of objective function |
| Temperature | Control Parameter |

1. Let `x_i` be the current solution to the problem
2. Generate a perturbed solution `y_i`
3. Decide if `y_i` is accepted or rejected
4. If accepted, the new state/solution is updated to `x_{i+1} = y_i`
5. Otherwise the new state remains the same (`x_{i+1} = x_i`)
6. Repeat until some stopping criteria is reached

#### Acceptance Criterion:

* Consider some minimizing function `E(x)`
* Let `c` be the control parameter
* Start with some feasible solution `x_i`
* Perturb `x_i` to some `y_i`
* Acceptance criterion:

```
Pr = {
		1                          if E(y_i) ≤ E(x_i)
		exp(-[E(y_i) - E(x_i)]/c)  if E(y_i) > E(x_i)
	  }
```

### General Algorithm

* `θ` denotes the variables you are optimizing in your problem
* `c` denotes the control paramter (analogous to temperature)
* `n` denotes the number of perturbations to be made
* Steps:
	1. Initialise `θ, c, n`
	2. Perturb `n` times:
		1. Generate a new perturbed solution `ɸ`
		2. If `ɸ` is accepted, set `θ = ɸ`
	3. Update `c`
	4. If the stop criterion is not fulfilled, goto 2, else stop!

## Cooling Schedule

* **Cooling Schedule** — updating rules of control paramter `c` and number of perturbations `n` (for the updated value `c`)
* In practice, simulated annealing cannot be pursued in a systematic way
* Find an ad-hoc cooling schedule which is practical and keeps the distribution close to a thermal equilibrium
* Considerations:
	* Choice of start temperature `c_0` — choose a value sufficiently large to accept all transitions
	* Choice of lowering tempoeratures `c_k` — often 10% lower than original temperature
	* Choice of stop criterion — e.g. when the solution is stuck for several updates to `c`
	* Choice of number of perturbations `n_k` — simplest approach is to make it large enough to be practicel
	* Thumb rule: if updates to `c` is in large steps, it has to be compensated by using larger `n` to reach thermal equilibrium
