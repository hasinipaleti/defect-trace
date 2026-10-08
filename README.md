# DefectTrace
Defect detection in metal castings using linear algebra: eigenvectors, projection and least-squares reconstruction error.
We teach the computer what a *good* casting looks like as a small subspace of "eigen-images". A defective casting cannot be rebuilt from those good patterns, so its reconstruction error is large.

# Problem Statement
Factories need to inspect cast pump impellers for defects (holes, cracks, rough edges). Defective parts are rare and varied, but good parts are easy to collect. So instead of learning what defects look like, we learn what **normal** looks like and flag anything far from normal.

# Dataset
[Casting product image data for quality inspection](https://www.kaggle.com/datasets/ravirajsinh45/real-life-industrial-dataset-of-casting-product) (Kaggle, uploaded by ravirajsinh45).
It contains photos of pump impellers in two classes: `ok_front` (good) and `def_front` (defective), split into train and test sets.

# Workflow
Real-world data → Matrix representation → RREF / LU → Rank, nullity and basis → Gram–Schmidt → Eigenvalues and diagonalization → Projection and least squares → Defect detection

# Linear Algebra Concepts Used
|Stage | Concepts |
| Data and matrix representation | Images as vectors, data matrix X, vector space of images, mean vector and centering |
| Matrix simplification and structure | RREF, LU decomposition, rank, nullity, null space |
| Remove redundancy | Linear independence, basis selection |
| Orthogonalization | Gram–Schmidt process, orthonormal basis, QR |
| Pattern discovery | Eigenvalues and eigenvectors, symmetric matrices, diagonalization (PCA) |
| Projection and approximation | Orthogonal projection, least squares |
| Final application | Reconstruction error, threshold, accuracy, precision, recall, confusion matrix, defect heatmap |

# Approach
1. Data matrix: each grayscale image is resized to 100 × 100 and flattened into a column of 10,000 pixel values. With 500 good images, X is 10000 × 500.
2. Centering: subtract the mean casting from every image.
3. Redundancy: RREF, rank and LU on a small image matrix show that good images are highly redundant. Gram–Schmidt builds an orthonormal basis.
4. Eigen-images: instead of the huge 10000 × 10000 covariance matrix, we use the small symmetric matrix S = XcᵀXc (500 × 500), which has the same non-zero eigenvalues. We keep the top k eigenvectors that explain 95% of the variance and convert them into image-sized eigen-images.
5. Projection: any new image is projected onto the "good" subspace. The projection is the least-squares best fit, and the leftover error is perpendicular to the subspace.
6. Detection: the mean squared reconstruction error is the defect score. A threshold chosen on validation data decides GOOD or DEFECTIVE.
7. Heatmap: |input − reconstruction| shows where the defect is.

# Results
- Number of eigen-images k (95% variance): 77
- Chosen threshold: 0.001805
- Test accuracy: 84.48%
- Precision: 82.88% |  Recall: 95.14%

# Libraries Used
`numpy`, `matplotlib`, `Pillow`, `sympy`, `scipy`

# How to Run
1. Open the notebook on Kaggle with the dataset attached (**+ Add Input**, search for the dataset name above).
2. Run the common setup cell first. It should print `Data matrix X shape (pixels x images): (10000, 500)`.
3. Run the remaining cells in order, or use **Run All**.

To run locally, clone the repo, download the dataset from Kaggle, update the `ROOT` path in the setup cell, and open the notebook in Jupyter:
```
git clone https://github.com/hasinipaleti/defect-trace.git
pip install numpy matplotlib pillow sympy scipy
```
