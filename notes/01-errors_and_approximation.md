[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Errors and Approximation

## Sources of Error in Scientific Computing

* **Modelling Errors**
	* Comes from _simplifying_ assumptions (or omitting aspects of the problem)
* **Data Errors**
	* Comes from inaccurate and imprecise empirical measurements
	* Can also come from using potentially incorrect results from some previous computational process
* **Computational Errors**
	* Can come from truncating a computational process
	* Can come from rounding

## Absolute and Relative Error

* **Absolute Error** — `| Approx. value - True value |`
* **Relative Error** — `Absolute error ÷ true value × 100%`

## Data Error and Computational Error

* Let:
	* `f` be a _true function_
	* `x` be a _true value_ to be used in a computation
	* `f(x)` be the _desired result_ of the true function on the true value
	* `g` be an _approximation_ of `f`
	* `y` be an _inexact measurement_ of `x`
	* `g(y)` be the _inexact_ result
* Then:
	* **Computational Error** = `g(y) - f(y)`
	* **Propagated Data Error** = `f(y) - f(x)`
	* **Total Error** = Computational Data Error + Propagated Error =  `g(y) - f(x)`

## Forward & Backward Errors

* **Forward Error**
	* Discrepancy between the computed and true values
	* `inexact output - exact output`
	* Quality of results: `|inexact output - exact output|` is the _relative magnitude of the forward error_
* **Backward Error**
	* Discrepancy between the initial output that produced the discrepancy in the result
	* `inexact input - exact input`

## Sensitivity and Conditioning

* **Sensitivity** — _qualitative_ statement of propagated data error
* **Conditioning** — _quantitative_ measure of propagated data error
* A problem is _insensitive_ or _well-conditioned_ if a given relative change in the input data causes a reasonably commensurate relative change in the solution
	* `|input a - input b| ~ |output a - output b|`
* Otherwise, it is _sensitive_ or _ill-conditioned_
	* `|input a - input b| >> or << |output a - output b|`
* **Condition Number**
	* Ratio of relative change
	* `| relative forward error | ÷ | relative backward error |`
	* Ill-conditioned problems have a condition number larger than 1
	* `~ xf'(x) ÷ f(x)`
