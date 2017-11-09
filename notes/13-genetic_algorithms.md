[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Genetic Algorithms

1. **Start**
	* Generate a random population of `n` solutions
	* Each solution is represented as a 'chromosome'
2. **Fitness**
	* Evalute the fitness `f(x)` for each chromosome `x`
3. **Breed**
	* **Selection**
		* Select two parent chromosomes from the population with good fitness
	* **Crossover**
		* With a _crossover probability_, crossover the selected parent chromosomes to create a new offspring
	* **Mutation**
		* With a _mutation probability_ mutate new offspring at each locus
	* **Acceptting**
		* Add the new offspring's chromosome to the new population
4. **Replace**
	* Replace the old population with the new one
5. **Test**
	* If termination criteria is satisfied, stop!
	* The fittest chromosome in the current population is the best solution the algorithm found — otherwise, loop.

## Important Features of Genetic Algorithms

* _Randomness_ forms an essential part of the algorithm
* Always considers a _population of solutions_ at a time (in contrast, simulated annealing considers only 1 solution at each stage)
* _Versatile_ — it can be used in a broad range of circumstances

## Encoding Solutions of Problems as Chromosomes

* **Binary Encoding** 
	* Solutions encoded as a string of bits 
	* e.g. `0` or `1` to represent whether an objects is to be stored for a _knapsack_ problem
* **Permuation Encoding**
	* Chromosomes are strings of numbers, which represents a number in a sequence
	* e.g. solutions to the _travelling salesman's problem_
* **Value Encoding**
	* Arrays of some values
	* e.g. Optimization problem with decision variables as a set of linear or non-linear constraints

## Crossover

* **Single Point Crossover**
	* **Binary Encoding** — Once the crossover point is selected, the binary string from beginning of chromosome to the crossover point is copied from the parent, rest is copied from the second parent
	* **Permutation Encoding** — Same as above
	* **Value Encoding** — Same as above
* **Two Point Crossover**
	* **Binary Encoding** — Two crossover points are selected, alternating copying from sections from either parent
	* **Permutation Encoding** — Same as above
	* **Value Encoding** — Same as above

## Mutation

* **Binary Encoding** — Selected bits are inverted
* **Permutation Encoding** — Randomly select two numbers and swap it
* **Value Encoding** — Select a value and make a small modification to that value

## Selection

### Roulette Wheel Selection

* Parents selected according to their fitness
* General Algorithm
	* Sum — calculate sum of all chromosomes fitnesses in population
	* Probability — convert each chromosome fitness into a probability by dividing by the sum
	* Select — generate a _uniform random number_ `r` from the interval `[0,1]` — find the chromosome corresponding to the random number `r`

### Rank Selection

* Rank the population and then every chromosome receives this ranking
* The worst has a rank of `N` (number of chromosomes in population)
* Best has rank of `1`
* Select those with the best ranks

### Steady-state Solution

* In every generation, a few _good_ (high fitness) chromosomes are selected for creating new offspring
* Then some _bad_ (low fitness) are removed and new offspring are replaced against them
* The rest of the population survives to a new generation

### Elitism

* When creating a new population by crossover and mutation, we have a big chance that we will lose the best chromosome
* **Elitism** involves first copying the best chromosome (or a few of the best) to a new population
* The rest is done in a classical way
* Elitism can very rapidly increase the time performance of a Genetic Algorithm, because it prevents losing the best found solution

## Paramters of Genetic Algorithms

### Crossover Probability

* Crossover probability says how often crossover will be performed
* If there is no crossover, offspring are exact copies of the parents
* If there is, offspring are made of parts of parents
* Although choice depends on the problem, the typical probabilites chosen range `[0,.65, 0.85]`

### Mutation Probability

* Mutation probability dictates how often will parts of a chromosome be mutated
* If there is no mutation, offspring is take after crossover without changes
* If mutation is performed, chromosome is changed
* Mutation is made to prevent the algorithm get caught in a local extrema

### Population Size

* Population size says how many chromsomes are in a population (in one generation)
* If there are too few chromosomes, Genetic Algorithms have a few possibilities to perform crossover and only a small part of the search space is explored
* On the other hand, if there are too many, it slows down
