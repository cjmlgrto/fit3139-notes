[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# Computer Arithmetic

## Floating Point Numbers

* In a digital computer, real numbers are represented using floating points
* e.g. `-0.0007396`
	* _Scientific notation_: `7.396 × 10^-4`
	* _Sign_: `-`
	* _Mantissa_: `7.396`
	* _Exponent_: `-4`
	* _Base_: decimal (base-10)
* Any floating-point number `x` has the form:
	* `x = ±( [d_0÷b^0] + [d_1÷b^1] + [d_2÷b^2] + ... + [d_(p-1)÷b^(p-1)] ) × b^E
	* `b` is the base or _radix_
	* `p` is the precision
	* `E ∈ [L,U]` is an exponent
	* Note that this is the general form for an _unnormalized_ floating point representation

## Normalization

* A floating-point system is said to be **normalized** if the leading digit `d_0` is always nonzero (unless the represented number is 0)
* Thus, in a normalized system, the mantissa `m` always satisfies `1 ≤ m ≤ base`
* Floating-point systems are usually normalized because
	* The representation of each number is then unique
	* No digits are wasted on leading zeroes, therby maximising precision
	* In binary (base = 2) system, the leading bit is always 1, thus need not be stored

## Properties of Floating-Point Systems

* The number of normalized floating-point numbers in a given floating-point system is:
	* `2(b-1)b^{p-1}(U - L + 1) + 1`
* The smallest positive normalized floating-point number is:
	* `Underflow level = UFL = b^L`
* The largest positive normalized floating-point number is:
	* `Overflow level = OFL = (1-b^{-p})b^{U+1}`

### Single Precision (32-bit)

| 1 bit | 8 bits | 23 bits |
| --- | --- | --- |
| Sign (S) | Exponent (E) | Mantissa or Fraction (F) |
| 31 | 30 ... 23 | 22 ... 0 |

* Decimal value of floating point
	* `(-1)^S × 2^{E_10 - 127} × (F)_10`
* `+0`
	* `S = 0`, `E = 0`, `F = 0`
* `-0`
	* `S - 1`, `E = 0`, `F = 0`
* `+∞`
	* `S = 0`, `E = 255_10`, `F = 0`
* `-∞`
	* `S = 1`, `E = 255_10`, `F = 0`
* `NaN`
	* `E = 255_10`, `S × F < 0 or > 0`

### Double Precision (64-bit)

| 1 bit | 11 bits | 52 bits |
| --- | --- | --- |
| Sign (S) | Exponent (E) | Mantissa or Fraction (F) |
| 63 | 62 ... 52 | 51 ... 0 |

* Decimal value of floating point
	* `(-1)^S × 2^{E_10 - 1023} × (F)_10`
* `+0`
	* `S = 0`, `E = 0`, `F = 0`
* `-0`
	* `S - 1`, `E = 0`, `F = 0`
* `+∞`
	* `S = 0`, `E = 2047_10`, `F = 0`
* `-∞`
	* `S = 1`, `E = 2047_10`, `F = 0`
* `NaN`
	* `E = 2047_10`, `S × F < 0 or > 0`

## Rounding

* **Rounding by Chopping** — base-b expansion is truncated after the (precision - 1)th digit
* **Rounding to Nearest** — real number is rounded to the nearest floating-point number

## Machine Epsilon

* **Machine Epsilon** gives an _upper bound on the relative error_ due to rounding in floating-point arithmetic
	* Rounding by chopping: `ε_mach = b^{1 - p}`
	* Rounding to nearest: `ε_mach = 0.5b^{1 - p}`
* The unit roundoff is important because it bounds the relative error in representing the real number `x`
	* `ε_mach ≥ |(inexact x - exact x) ÷ exact x|`
