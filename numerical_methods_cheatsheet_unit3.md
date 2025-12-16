# 🎯 NUMERICAL METHODS COMPLETE CHEATSHEET
## UNIT 3: Linear Systems, Eigenvalues & Splines

---

# 📚 PART A: SYSTEMS OF LINEAR EQUATIONS

---

## 1. EXISTENCE OF SOLUTIONS

### 🧠 Intuition First

A system of linear equations is like asking: "Where do these lines/planes/hyperplanes all meet?"

Possible outcomes:
- **One solution**: They all meet at exactly one point
- **No solution**: They never all meet (inconsistent)
- **Infinite solutions**: They overlap along a line/plane

### 📐 Matrix Form

System: Ax = b

Where:
- A is the coefficient matrix (n × n)
- x is the unknown vector
- b is the right-hand side

### 📐 Conditions for Unique Solution

**Theorem**: Ax = b has a UNIQUE solution if and only if:
1. det(A) ≠ 0 (A is non-singular/invertible)
2. rank(A) = rank([A|b]) = n

**Cases**:
| det(A) | rank(A) vs rank([A|b]) | Solutions |
|--------|------------------------|-----------|
| ≠ 0 | rank = n | Unique |
| = 0 | rank(A) = rank([A|b]) < n | Infinite |
| = 0 | rank(A) < rank([A|b]) | None |

### 💡 Memory Trick
**"DET ≠ 0 → One Solution, DET = 0 → Check Ranks"**

---

## 2. GAUSS ELIMINATION METHOD

### 🧠 Intuition First

Transform the messy system into a "staircase" form where you can solve from bottom to top.

It's like organizing a pile of tangled equations into a neat pyramid!

### 📐 The Process

**Phase 1: Forward Elimination** (make upper triangular)
**Phase 2: Back Substitution** (solve from bottom up)

### 📐 Step-by-Step Algorithm

**Given**: Ax = b (n equations, n unknowns)

**Forward Elimination** (for k = 1 to n-1):
```
For i = k+1 to n:
    multiplier = aᵢₖ / aₖₖ
    For j = k to n:
        aᵢⱼ = aᵢⱼ - multiplier × aₖⱼ
    bᵢ = bᵢ - multiplier × bₖ
```

**Back Substitution**:
```
xₙ = bₙ / aₙₙ
For i = n-1 down to 1:
    xᵢ = (bᵢ - Σⱼ₌ᵢ₊₁ⁿ aᵢⱼxⱼ) / aᵢᵢ
```

### 📊 Worked Example

Solve:
```
2x + y - z = 8
-3x - y + 2z = -11
-2x + y + 2z = -3
```

**Augmented matrix**:
```
[ 2   1  -1 |  8 ]
[-3  -1   2 | -11]
[-2   1   2 | -3 ]
```

**Step 1**: Eliminate x from rows 2, 3

R2 → R2 + (3/2)R1:
```
[ 2   1    -1  |   8  ]
[ 0  1/2  1/2  |  1   ]
[-2   1    2   | -3   ]
```

R3 → R3 + R1:
```
[ 2   1    -1  |   8  ]
[ 0  1/2  1/2  |   1  ]
[ 0   2    1   |   5  ]
```

**Step 2**: Eliminate y from row 3

R3 → R3 - 4×R2:
```
[ 2   1    -1  |   8  ]
[ 0  1/2  1/2  |   1  ]
[ 0   0   -1   |   1  ]
```

**Back Substitution**:
- From R3: -z = 1 → z = -1
- From R2: y/2 + (-1)/2 = 1 → y = 3
- From R1: 2x + 3 - (-1) = 8 → x = 2

**Solution**: x = 2, y = 3, z = -1 ✓

### 📐 Computational Cost

**Forward Elimination**:
```
Multiplications: Σₖ₌₁ⁿ⁻¹ (n-k)(n-k+1) ≈ n³/3
Additions: Same order ≈ n³/3
```

**Back Substitution**:
```
Multiplications: n(n+1)/2 ≈ n²/2
Additions: n(n-1)/2 ≈ n²/2
```

**Total**: O(n³/3) multiplications and O(n³/3) additions

Or approximately: **2n³/3 operations** in total

### 💡 Memory Trick
**"Gauss = Triangle then Climb" (make triangle, climb up solving)**

---

## 3. PIVOTING

### 🧠 Why Pivoting?

**Problem**: If the pivot element (diagonal) is zero or very small:
1. Zero → Division by zero (crash!)
2. Very small → Large round-off errors

**Solution**: Swap rows to get a better pivot!

### 📐 Types of Pivoting

**Partial Pivoting** (most common):
- At step k, find the largest |aᵢₖ| for i ≥ k
- Swap that row with row k
- Then proceed with elimination

**Complete Pivoting** (rarely used):
- Find the largest element in the entire remaining submatrix
- Swap both rows AND columns
- More accurate but expensive

**Scaled Partial Pivoting**:
- Account for the scale of each row
- Find max |aᵢₖ|/sᵢ where sᵢ = max|aᵢⱼ| in row i
- Better for badly scaled systems

### 📊 Example: Why Pivoting Matters

Without pivoting in 3-digit arithmetic:
```
0.001x + y = 1
    x + y = 2
```

Multiplier = 1/0.001 = 1000
R2 → R2 - 1000×R1:
```
0.001x + y = 1
      -999y = -998
```
y = 998/999 ≈ 0.999 (3 digits: 1.00)
x = (1 - 1)/0.001 = 0 (WRONG! Should be ≈ 1)

With pivoting (swap rows first):
```
x + y = 2
0.001x + y = 1
```
Multiplier = 0.001
R2 → R2 - 0.001×R1:
```
x + y = 2
0.999y = 0.998
```
y ≈ 1.00, x = 2 - 1 = 1 ✓

---

## 4. GAUSS-JORDAN METHOD

### 🧠 Intuition First

Gauss-Jordan goes further than Gauss - it makes the matrix into the IDENTITY matrix, not just upper triangular. Then the solution just appears in the right-hand side!

### 📐 The Process

1. Forward elimination (like Gauss) → upper triangular
2. Backward elimination → diagonal matrix
3. Divide each row by its diagonal → identity matrix

### 📐 Algorithm

Same forward elimination as Gauss, then:

**Backward Elimination** (for k = n down to 2):
```
For i = k-1 down to 1:
    multiplier = aᵢₖ / aₖₖ
    For j = 1 to n:
        aᵢⱼ = aᵢⱼ - multiplier × aₖⱼ
    bᵢ = bᵢ - multiplier × bₖ
```

**Normalization**:
```
For i = 1 to n:
    bᵢ = bᵢ / aᵢᵢ
```

### 📊 Result

[A|b] → [I|x]

The solution x appears directly!

### 📐 Computational Cost

**Total operations**: O(n³/2) - MORE than Gauss elimination!

Why use it then?
- Finding matrix inverse: [A|I] → [I|A⁻¹]
- Cleaner for theoretical analysis

### 💡 Memory Trick
**"Gauss-Jordan = Complete cleanup to Identity"**

---

## 5. LU DECOMPOSITION

### 🧠 Intuition First

Instead of solving Ax = b directly, factor A = LU where:
- L = Lower triangular matrix (ones on diagonal)
- U = Upper triangular matrix

Then solve in two easy steps:
1. Ly = b (forward substitution)
2. Ux = y (back substitution)

**Advantage**: Once you have L and U, solving for DIFFERENT b vectors is cheap!

### 📐 Doolittle Algorithm

**Condition**: A can be factored as A = LU where L has 1's on diagonal

```
For each column k:
    For i = k to n:  (compute U elements)
        uₖᵢ = aₖᵢ - Σⱼ₌₁ᵏ⁻¹ lₖⱼuⱼᵢ
    
    For i = k+1 to n:  (compute L elements)
        lᵢₖ = (aᵢₖ - Σⱼ₌₁ᵏ⁻¹ lᵢⱼuⱼₖ) / uₖₖ
```

### 📊 Worked Example

Factor:
```
A = [2  1  1]
    [4  3  3]
    [8  7  9]
```

**Step 1**: First column of U, first column of L
```
u₁₁ = 2, u₁₂ = 1, u₁₃ = 1
l₂₁ = 4/2 = 2
l₃₁ = 8/2 = 4
```

**Step 2**: Second row of U, second column of L
```
u₂₂ = 3 - (2)(1) = 1
u₂₃ = 3 - (2)(1) = 1
l₃₂ = (7 - 4×1)/1 = 3
```

**Step 3**: Third row of U
```
u₃₃ = 9 - (4)(1) - (3)(1) = 2
```

**Result**:
```
L = [1  0  0]     U = [2  1  1]
    [2  1  0]         [0  1  1]
    [4  3  1]         [0  0  2]
```

Verify: LU = A ✓

### 📐 Crout's Algorithm

**Difference**: L has the general values, U has 1's on diagonal

Same idea, slightly different formulas.

### 📐 Cholesky Decomposition

**For symmetric positive definite matrices only!**

A = LLᵀ where L is lower triangular

```
For j = 1 to n:
    lⱼⱼ = √(aⱼⱼ - Σₖ₌₁ʲ⁻¹ lⱼₖ²)
    
    For i = j+1 to n:
        lᵢⱼ = (aᵢⱼ - Σₖ₌₁ʲ⁻¹ lᵢₖlⱼₖ) / lⱼⱼ
```

**Advantages**:
- Only need to store L (half the matrix!)
- About HALF the operations of LU
- Numerically very stable
- If Cholesky fails (negative under sqrt), matrix isn't positive definite

### 📐 Computational Costs

| Method | Operations |
|--------|-----------|
| Gauss Elimination | n³/3 |
| LU Decomposition | n³/3 (same as Gauss) |
| LU Solve (given L,U) | n² |
| Gauss-Jordan | n³/2 |
| Cholesky | n³/6 |

### 💡 Memory Trick
**"LU = Factor Once, Solve Many Times Cheaply"**

---

# 📚 PART B: EIGENVALUE PROBLEMS

---

## 6. POWER METHOD

### 🧠 Intuition First

Repeatedly multiply a vector by matrix A. The vector will eventually point in the direction of the "dominant" eigenvector, and the scaling factor gives the largest eigenvalue!

Like repeatedly stretching something - it naturally aligns with the strongest direction.

### 📐 The Algorithm

**Goal**: Find λ₁ (largest eigenvalue) and v₁ (corresponding eigenvector)

```
1. Start with initial guess x⁽⁰⁾ (random nonzero vector)
2. Repeat until convergence:
   a. y⁽ᵏ⁾ = A × x⁽ᵏ⁻¹⁾
   b. λ⁽ᵏ⁾ = max component of y⁽ᵏ⁾ (or use Rayleigh quotient)
   c. x⁽ᵏ⁾ = y⁽ᵏ⁾ / λ⁽ᵏ⁾ (normalize)
3. λ₁ ≈ λ⁽ᵏ⁾, v₁ ≈ x⁽ᵏ⁾
```

**Alternative**: Rayleigh quotient for eigenvalue:
```
λ⁽ᵏ⁾ = (x⁽ᵏ⁾)ᵀ A x⁽ᵏ⁾ / (x⁽ᵏ⁾)ᵀ x⁽ᵏ⁾
```

### 📊 Worked Example

Find dominant eigenvalue of:
```
A = [2  1]
    [1  2]
```

Starting with x⁽⁰⁾ = [1, 1]ᵀ

**Iteration 1**:
```
y⁽¹⁾ = A × [1,1]ᵀ = [3, 3]ᵀ
λ⁽¹⁾ = 3
x⁽¹⁾ = [1, 1]ᵀ
```

**Iteration 2**:
```
y⁽²⁾ = A × [1,1]ᵀ = [3, 3]ᵀ
λ⁽²⁾ = 3
x⁽²⁾ = [1, 1]ᵀ
```

Already converged! λ₁ = 3, v₁ = [1, 1]ᵀ

(Actual eigenvalues: 3 and 1)

### 📐 Convergence Analysis

Let eigenvalues be |λ₁| > |λ₂| ≥ ... ≥ |λₙ|

**Convergence rate**: |λ₂/λ₁|

```
Error at step k ≈ C × |λ₂/λ₁|ᵏ
```

**Faster convergence when**:
- |λ₁| >> |λ₂| (dominant eigenvalue is much larger)

**Slow convergence when**:
- λ₂ ≈ λ₁ (eigenvalues are close)

### ⚠️ Conditions and Limitations

**Works when**:
1. There IS a unique dominant eigenvalue (|λ₁| > |λ₂|)
2. Initial vector has a component in direction of v₁

**Fails when**:
1. Two eigenvalues with same magnitude (e.g., λ = ±3)
2. Initial vector is orthogonal to v₁ (unlikely in practice with rounding)

### 📐 Inverse Power Method

To find the SMALLEST eigenvalue:

Apply Power method to A⁻¹!

**Why?** If λ is eigenvalue of A, then 1/λ is eigenvalue of A⁻¹.
The smallest λ of A becomes the largest eigenvalue of A⁻¹.

**Shifted Inverse Power**:
To find eigenvalue closest to a guess μ:
Apply power method to (A - μI)⁻¹

### 💡 Memory Trick
**"Power = Pump up the dominant direction"**

---

# 📚 PART C: SPLINE FUNCTIONS

---

## 7. FIRST-DEGREE SPLINES (Linear Splines)

### 🧠 Intuition First

Just connect the dots with straight lines! Simple but not smooth.

### 📐 Definition

Given points (x₀,y₀), (x₁,y₁), ..., (xₙ,yₙ):

On interval [xᵢ, xᵢ₊₁]:
```
S(x) = yᵢ + [(yᵢ₊₁ - yᵢ)/(xᵢ₊₁ - xᵢ)] × (x - xᵢ)
```

### 📐 Properties

- **Continuous**: Yes ✓
- **Smooth (C¹)**: No ✗ (corners at nodes)
- **Easy to compute**: Yes ✓

### 📐 Error

```
|f(x) - S(x)| ≤ (h²/8) × max|f''|
```
where h = max spacing

---

## 8. SECOND-DEGREE SPLINES (Quadratic Splines)

### 📐 Definition

Piecewise quadratic polynomials:
```
Sᵢ(x) = aᵢ + bᵢ(x - xᵢ) + cᵢ(x - xᵢ)²
```

### 📐 Conditions

For n+1 points (n intervals):
- Interpolation: Sᵢ(xᵢ) = yᵢ and Sᵢ(xᵢ₊₁) = yᵢ₊₁ → 2n equations
- Smoothness at interior nodes: S'ᵢ₋₁(xᵢ) = S'ᵢ(xᵢ) → n-1 equations
- Total: 3n unknowns, 3n-1 equations

**Need 1 extra condition** (e.g., S'(x₀) = 0 or given slope)

### 📐 Properties

- **Continuous**: Yes ✓
- **C¹ smooth**: Yes ✓
- **C² smooth**: No ✗

---

## 9. NATURAL CUBIC SPLINES

### 🧠 Intuition First

Cubic splines are like a flexible ruler (drafting spline) bent through the data points. The name literally comes from the physical tool!

A cubic spline minimizes "bending energy" - it's the smoothest way to connect points.

### 📐 Definition

Piecewise cubic polynomials:
```
Sᵢ(x) = aᵢ + bᵢ(x - xᵢ) + cᵢ(x - xᵢ)² + dᵢ(x - xᵢ)³
```

### 📐 Conditions

For n+1 points (n intervals), we need 4n coefficients:

1. **Interpolation** at left endpoints: Sᵢ(xᵢ) = yᵢ → n equations
2. **Interpolation** at right endpoints: Sᵢ(xᵢ₊₁) = yᵢ₊₁ → n equations
3. **First derivative continuity**: S'ᵢ₋₁(xᵢ) = S'ᵢ(xᵢ) → n-1 equations
4. **Second derivative continuity**: S''ᵢ₋₁(xᵢ) = S''ᵢ(xᵢ) → n-1 equations

Total: 4n-2 equations for 4n unknowns

**Need 2 extra conditions!**

### 📐 Natural Spline Boundary Conditions

**Natural**: S''(x₀) = 0 and S''(xₙ) = 0

(The spline is "straight" at the endpoints - zero curvature)

### 📐 Other Boundary Conditions

- **Clamped**: S'(x₀) = f'(x₀) and S'(xₙ) = f'(xₙ) (given slopes)
- **Not-a-knot**: Third derivative continuous at x₁ and xₙ₋₁

### 📐 Algorithm for Natural Cubic Splines

**Step 1**: From conditions, derive a tridiagonal system for the second derivatives Mᵢ = S''(xᵢ)

Let hᵢ = xᵢ₊₁ - xᵢ

The system is:
```
hᵢ₋₁Mᵢ₋₁ + 2(hᵢ₋₁ + hᵢ)Mᵢ + hᵢMᵢ₊₁ = 6[(yᵢ₊₁-yᵢ)/hᵢ - (yᵢ-yᵢ₋₁)/hᵢ₋₁]
```
for i = 1, ..., n-1, with M₀ = Mₙ = 0

**Step 2**: Solve the tridiagonal system (O(n) operations!)

**Step 3**: Compute coefficients:
```
aᵢ = yᵢ
cᵢ = Mᵢ/2
dᵢ = (Mᵢ₊₁ - Mᵢ)/(6hᵢ)
bᵢ = (yᵢ₊₁ - yᵢ)/hᵢ - hᵢ(Mᵢ₊₁ + 2Mᵢ)/6
```

### 📐 Properties of Cubic Splines

1. **C² continuity**: Smooth up to second derivative
2. **Minimum curvature property**: Among all C² functions interpolating the data, cubic spline minimizes ∫[S''(x)]² dx
3. **Local modification**: Changing one data point only affects nearby spline pieces
4. **Stability**: Well-conditioned, robust to perturbations

### 📐 Error Bound

```
|f(x) - S(x)| ≤ (5/384) × h⁴ × max|f⁽⁴⁾|
```

O(h⁴) - same as Simpson's rule!

### 💡 Memory Trick
**"Cubic Spline = Flexible ruler through points = Smooth + Natural"**

---

## 10. B-SPLINES (Basis Splines)

### 🧠 Intuition First

Instead of defining splines piece by piece, define special "bump" functions that you can combine. Each B-spline is like a smooth bump centered at a knot.

### 📐 Definition

B-splines of degree k are defined recursively:

**Degree 0** (step function):
```
Bᵢ,₀(x) = 1 if tᵢ ≤ x < tᵢ₊₁, else 0
```

**Higher degrees** (Cox-de Boor recursion):
```
Bᵢ,ₖ(x) = [(x - tᵢ)/(tᵢ₊ₖ - tᵢ)]Bᵢ,ₖ₋₁(x) + [(tᵢ₊ₖ₊₁ - x)/(tᵢ₊ₖ₊₁ - tᵢ₊₁)]Bᵢ₊₁,ₖ₋₁(x)
```

### 📐 Properties of B-Splines

1. **Local support**: Bᵢ,ₖ(x) is nonzero only for x ∈ [tᵢ, tᵢ₊ₖ₊₁]
2. **Non-negativity**: Bᵢ,ₖ(x) ≥ 0
3. **Partition of unity**: ΣBᵢ,ₖ(x) = 1 for x in the proper range
4. **Smoothness**: Cᵏ⁻¹ continuous
5. **Recursion**: Built from lower-degree B-splines

### 📐 Spline as B-Spline Combination

Any spline of degree k can be written as:
```
S(x) = Σᵢ cᵢ Bᵢ,ₖ(x)
```

The coefficients cᵢ are called **control points**.

### 📐 Advantages of B-Splines

1. **Numerical stability**: Better conditioned than polynomial basis
2. **Local control**: Changing one cᵢ only affects local region
3. **Efficient evaluation**: De Boor's algorithm is O(k²)
4. **Widely used**: Computer graphics, CAD, data fitting

---

## 11. INTERPOLATION VS APPROXIMATION

### 📐 Key Distinction

| Interpolation | Approximation |
|---------------|---------------|
| Pass through ALL data points | Get "close" to data points |
| Exact at nodes | Minimizes overall error |
| Can oscillate wildly | Typically smoother |
| Sensitive to noise | More robust to noise |

### 📐 Least Squares Approximation

For noisy data, fit a simpler curve that minimizes:
```
E = Σᵢ [yᵢ - P(xᵢ)]²
```

**Linear least squares** (fitting a line):
```
y = a + bx

b = [nΣxᵢyᵢ - ΣxᵢΣyᵢ] / [nΣxᵢ² - (Σxᵢ)²]
a = (Σyᵢ - bΣxᵢ) / n
```

### 💡 Memory Trick
**"Interpolation = Through the points; Approximation = Near the points"**

---

# 📊 UNIT 3 SUMMARY TABLE

| Method | Purpose | Complexity | Key Feature |
|--------|---------|------------|-------------|
| Gauss Elimination | Solve Ax=b | O(n³/3) | Upper triangular |
| Gauss-Jordan | Solve Ax=b / Inverse | O(n³/2) | Identity matrix |
| LU (Doolittle/Crout) | Factor A=LU | O(n³/3) | Reuse for multiple b |
| Cholesky | Factor A=LLᵀ | O(n³/6) | Symmetric positive definite |
| Power Method | Largest eigenvalue | O(n² per iter) | Simple, iterative |
| Linear Spline | Interpolation | O(n) | Simple, not smooth |
| Cubic Spline | Interpolation | O(n) | C² smooth, natural |
| B-Spline | Interpolation/Approx | O(k²) | Local control |

---

# 🎯 EXAM CHECKLIST - UNIT 3

□ Can perform Gauss elimination with back substitution?
□ Know when/why to use pivoting?
□ Understand LU decomposition concept?
□ Can derive Cholesky for 2×2 matrix?
□ Know the Power method algorithm?
□ Understand convergence rate of Power method?
□ Know properties of natural cubic splines?
□ Can state boundary conditions for natural splines?
□ Understand difference between interpolation and approximation?
