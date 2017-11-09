[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Dynamic Programming

* **Dynamic Programming** is a powerful optimization technique used to solve complext optimization/search problems that exhibits certain properties
* The core idea is to _divide_ a problem by breaking it down into simpler subproblems and solving it recursively
* An optimization problem is said to have an **Optimal Substructure** if the optimal solution of the problem can be constructed from the optimal solutions to its subproblems
* A problem is said to have **overlapping subproblems** if the problem can be broken down into subproblems and these subproblems overlap along various branches of the search/solution space
* To solve a problem using dynamic programming:
	* Solve the subproblems
	* Combine solutions of the subproblems to derive the optimal solution of a larger problem (and so on)
	* Since suproblems overlap, solve each subproblem only once and remember the solution
	* Remembering the history of optimal solutions of the subproblems is called **memoization**

## Comparing Sequences

### Edit Distance

* **Edit Distance** — the smallest number of edit operations required to transform one sequence into another
* **Pairwise Alignment** — the alignment of each member of two sequences such that the edit distance is minimised

```
TGCCTA
vs
TGACGAA

Has the optimal alignment:

TG-CC-TA
TGACGA-A
```

* To use dynamic programming to compute the optimal alignment, we break it down into smaller substructures — the _optimal substructure_
* In the case above, we can see that the edit distance of both strings is a sum of the edit distance of each individual one-character-long string

```
editDistance(TG-CC-TA, TGACGA-A)
= editDistance(TG-CC-T, TGACGA-) + editDistance(A, A)
= editDistance(TG-CC-, TGACGA) + editDistance(T, -) + editDistance(A, A)

And so on...
```

* We can then build a _memoization_ matrix that summarises the edit distances for each string as it accumulates in length
* A small example represented in the matrix `D` below:

|   | * | A | G | T | A |
|---|---|---|---|---|---|
| * | 0 | 1 | 2 | 3 | 4 |
| A | 1 | 0 | 1 | 2 | 3 |
| T | 2 | 1 | 1 | 1 | 2 |
| A | 3 | 2 | 2 | 2 | 1 |

* Each element `(i, j)` (representing rows, columns) in the above matrix `D` can be computed using a set of recurrence relationships:
	* `D[0,0] = 0`
	* `D[i,0] = D[i-1,0] + 1`
	* `D[0,j] = D[0,j-1] + 1`
	* `D[i,j] = min{`
		* `D[i-1,j-1] + { 0 if characters are the same, else 1`
		* `D[i-1,j] + 1`
		* `D[i,j-1] + 1`

* Given any two sequences and a table generated the same way above, an optimal alignment can be produced by back-tracking the memoization matrix
* This is done by starting at the last element (bottom right) and following the minimum values until you reach the first element of the matrix (top left) — all while building the strings from the end
* Each 'edit' operation is determined by the path you take
	* i.e. For every cell, compare the top, left or top-left
	* If you go top-left, keep the sequence the same
	* If you go to the left, add a blank space to the second string
	* Else on the top, add a blank space to the first string


### String Similarity

* Strings can be compared based on similarity by 'scoring' the alignment of two strings
	* e.g. for every character that matches, increment the score
	* Else if a deletion or insertion is required, decrement the score
* A **substitution matrix** dictates the score of swapping certain letters with certain other letters in a sequence
	* e.g. the substitution matrix `BLOSUM-62` used for comparing protein sequences suggests a score of `3` when swapping a `Z` with a `Q`
* A **gap penalty** is the score when insertions or deletions are required
	* They can be constant (i.e. for each gap, decrement a constant score) — the result of this is that any dynamic programming solutions for comparing strings is that they are additive (i.e. the recurrence relations above can be used)
	* They can follow a function (i.e. for a stretch of gaps, there is a certain score) — a consequence of this is that the generic dynamic programming recurrence relations cannot be used since scores aren't additive (i.e. the individual sum of the penalty of gaps don't equal the collective sum of gaps given a penalty function)

#### Recurrence Relations for Constant Gap Penalty

* `D[0,0] = 0`
* `D[i,0] = D[i-1,0] + g`
* `D[0,j] = D[0,j-1] + g`
* `D[i,j] = min{`
	* `D[i-1,j-1] + substitutionMatrixScore(char1, char2)`
	* `D[i-1,j] + g`
	* `D[i,j-1] + g`

#### Recurrence Relations for Gap Penalty Functions

* Let `G(l)` be the gap penalty function for a gap length of `l`
* `D[0,0] = 0`
* `D[i,0] = G(l)`
* `D[0,j] = G(l)`
* `D[i,j] = min{`
	* `D[i-1,j-1] + substitutionMatrixScore(char1, char2)`
	* `D[i-1,j] + G(l)`
	* `D[i,j-1] + G(l)`