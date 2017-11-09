[← Return to Index](https://github.com/cjmlgrto/fit3139-notes/)

# High-Dimensional Data Analysis

## Data Analysis Basics

* **Sample Mean**
	* `X` = Sum each element and divide by the number of elements
* **Standard Deviation**
	* `σ` = For each element, subtract the mean and square the result, then sum it all, divide by the number of elements and square root
* **Variance**
	* `σ^2`
* **Covariance**
	* A measure of how multi-dimensional variables vary
	* `Cov(x,y)` = For each element, subtract from the dimension's mean, then multiply together for each dimension, sum for each element and divide by the total number of elements per dimension
	* For `> 2` dimensions, a matrix can be used to represent the covariance between any pairwise dimension
	* A **positive** covariance indicates both dimensions increase together
	* A **negative** covariance indicates that one increases while the other decreases
	* If **zero**, the two dimensions are independent of each other
	* A covariance matrix is **real** and square-symmetric
	* Every coveriance matrix is **positive semidefinite** — i.e. if `C` is the matrix, then `transpose(z)Cz ≥ 0` for all possible vectors `z` (with real entries)
	* Eigenvalues are real and non-negative
	* Eigenvectors are perpendicular to each other

## Principal Component Analysis (PCA)

* PCA is a way to identify paterns in data which highlights similarities and differences
* Patterns are hard to find in high-dimensional data — PCA offers a powerful way to analyze such data
* PCA helps reduce the dimensions of data and hence compress it without losing much information (used in image compression)

### Steps

Given `x1, x2, .. xn` as high-dimensional data sets (that is, each `xi` is some `k`-dimensional vector represented as a column matrix of data:

1. Translate the mean/centroid of the data to origin
	* Subtract each data vector by the mean vector
	* The mean vector is simply the sum of every single element of the column vectors per data set, all each divided by the number of data sets
2. Construct a covariance matrix
	* `C = transpose(XX)/n`
	* Where `X` is the data matrix where each column (vector) of this matrix represents a datum (vector)
3. Eigenvalue decomposition
	* `C = SΛS^-1` (any real matrix `A` has perpendicular eigenvectors if and only if `transpose(A)A = Atranspose(A)`)
	* Arrange the eigenvectors in the descreasing order of their eignvalues
		* i.e. `S = s1, s2, s2, ..., sk`
	* With corresponding eigenvalue matrix
		* `Λ = λ1, λ2, λ2, ..., λk`
		* Such that `λ1 ≥ λ2 ≥ ... ≥ λk`
	* Why do this?
		* Eigendecomposition of the covariance matrix reveals the 'internal structure' of the data in a way which best explains the variance in the data
		* The vector associated with the largest eigenvalue is called the _first principal component_ (and so on for the 2nd largest, etc.)
	* **The PCA Theorem** states that each vector `xi` can be written as:
	* `µ + sum(ζ*s)`
	* Where `s` are the eigenvector associated with the largest eigenvalue of `C`, and `ζ` is the projection length of the vector `xi - µ` onto `s`
4. Choosing the principal components
	* At this stage, we have a set of eigenvectors ordered (decreasingly) by their corresponding values
	* As the eigenvalues become smaller and smaller, the corresponding eigenvectors are less significant components where the relationship between the original data becomes increasingly independent
	* Once strategy to reduce the high dimensionality of the data is to choose a top few principal components (say, up to element `d`)
5. Stating the original data solely in terms of chosen principal components
	* Project the original data onto the chosen eigenvector components, therby reducing the dimensionality of the data
	* To project a coordinate onto the component:
		* Ensure that the component eigenvector is a unit vector
		* From the PCA theorem, each `xi` can be written as:
			* `xi = µ + sum(ζ*s)` done up to the first `d` terms
		* The above can be performed by finding a dot product of `xi - µ` with each of the eigenvectors `s` up to element `d`
		* This give the values `ζ` for the above equation

## Clustering

### K-Means Clustering (Flat Clustering)

* Assume we have `n` (multi-dimensional) data points
* The problem:
	* Partition the data into `K` clusters that optimizes some objective function
	* This problem is NP-hard for many objective functions
	* The commonly used objective function involves Euclidean Distance
* Solution: K-Means
	* Uses the mean of data belonging to a cluster to formulate how good the cluster is
	* Iteratively reassigns the cluster based on the distance of data from cluster centers

#### Iterative Algorithm

1. Since the value `K` is given, select `K` random points from the data
2. Use these randomly selected points as cluster centers
3. Until the clustering converges (or based on some other stopping criteria):
	* Go through each data and assign it a cluster whose center is the nearest
	* Once all data is assigned, based on the assignment, recompute the centroids of each cluster and repeat the process

#### Termination Condition

* A fixed number of iterations
* Or terminate when the clustering does not change

#### Convergence

* The method converges because the average distance of points to the centers of their respective clusters decreases monotonically

### Unweighted Pair Group Method with Arithmetic Mean (Hierarchical Clustering)

Assume we are given `n` data vectors

1. Construct a pairise `n×n` matrix of distances `D` which give the distance between any two pairs of vectors
2. At each step, the closes two clusters are combined into a super cluster as follows:
	* The distance between two clusters `A` and `B` is the average over all pairwise distances of the members in those clusters
		* `distance(A,B) = 1/|A||B| × sum(distances in clusters)`
	* Merge two clusters that are closest into one larger cluster
	* Repear the process until the number of clusters remaining is 1

