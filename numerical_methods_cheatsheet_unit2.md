# 🎯 NUMERICAL METHODS COMPLETE CHEATSHEET
## UNIT 2: Interpolation & Numerical Integration

---

# 📚 PART A: INTERPOLATION

---

## 1. FUNDAMENTALS OF INTERPOLATION

### 🧠 Intuition First

Imagine you measured temperature at 9 AM (20°C) and 12 PM (26°C). What was the temperature at 10 AM? You don't know exactly, but you can make an educated GUESS by drawing a line between the two known points and reading off the value.

**Interpolation** = Finding unknown values BETWEEN known data points
**Extrapolation** = Finding values OUTSIDE known data (more dangerous!)

### 📐 The Problem Statement

**Given**: n+1 data points: (x₀, y₀), (x₁, y₁), ..., (xₙ, yₙ)
- The xᵢ are called **nodes** or **abscissas**
- The yᵢ are called **ordinates**

**Find**: A function P(x) such that P(xᵢ) = yᵢ for all i

### 📐 Why Polynomials?

We typically use polynomials because they:
1. Are easy to evaluate (just add and multiply)
2. Are infinitely differentiable
3. Can be integrated easily
4. Are unique for given constraints (Weierstrass theorem)

**Fundamental Theorem**: Given n+1 distinct points, there exists a UNIQUE polynomial of degree ≤ n passing through all of them.

### ⚠️ Assumptions for Interpolation

1. **Distinct nodes**: All xᵢ must be different
2. **Exact data**: We assume yᵢ values are exact (not noisy)
3. **Underlying function exists**: There's some true f(x) we're approximating
4. **Smoothness**: f(x) is sufficiently smooth (differentiable)

---

## 2. ERRORS IN POLYNOMIAL INTERPOLATION

### 📐 The Error Formula

If P(x) is the interpolating polynomial of degree n for f(x), the error is:

```
E(x) = f(x) - P(x) = [f⁽ⁿ⁺¹⁾(ξ) / (n+1)!] × ∏(x - xᵢ)
```

Where:
- ξ is some point in the interval containing x and all nodes
- ∏(x - xᵢ) = (x - x₀)(x - x₁)...(x - xₙ)

### 📐 Error Bound

```
|E(x)| ≤ [Mₙ₊₁ / (n+1)!] × |∏(x - xᵢ)|
```

Where Mₙ₊₁ = max|f⁽ⁿ⁺¹⁾(t)| over the interval.

### ⚠️ Runge's Phenomenon

**Problem**: More points don't always mean better approximation!

For f(x) = 1/(1 + 25x²) on [-1, 1] with equally spaced points:
- As n increases, oscillations GROW near the edges!
- Error explodes instead of decreasing!

**Solution**: Use Chebyshev nodes (not equally spaced):
```
xᵢ = cos[(2i+1)π / (2n+2)]  for i = 0, 1, ..., n
```

### 💡 Memory Trick
**"More points isn't always better - watch for Runge!"**

---

## 3. FINITE DIFFERENCES

### 🧠 Intuition First

Finite differences are like discrete derivatives. Instead of asking "what's the slope at a point?", we ask "what's the slope between two adjacent points?"

### 📐 Forward Differences

**First forward difference**:
```
Δy₀ = y₁ - y₀
Δyᵢ = yᵢ₊₁ - yᵢ
```

**Second forward difference**:
```
Δ²y₀ = Δy₁ - Δy₀ = (y₂ - y₁) - (y₁ - y₀) = y₂ - 2y₁ + y₀
```

**General pattern**:
```
Δⁿyᵢ = Δⁿ⁻¹yᵢ₊₁ - Δⁿ⁻¹yᵢ
```

**Explicit formula**:
```
Δⁿy₀ = Σₖ₌₀ⁿ (-1)ⁿ⁻ᵏ C(n,k) yₖ
```

### 📐 Backward Differences

**First backward difference**:
```
∇yₙ = yₙ - yₙ₋₁
∇yᵢ = yᵢ - yᵢ₋₁
```

**Second backward difference**:
```
∇²yₙ = ∇yₙ - ∇yₙ₋₁
```

### 📐 Central Differences

```
δyᵢ = yᵢ₊½ - yᵢ₋½
```

(Used when data is centered around a point)

### 📊 Forward Difference Table

For data: (0,1), (1,4), (2,9), (3,16) [these are x²+1 values]

| x | y | Δy | Δ²y | Δ³y |
|---|---|-----|------|------|
| 0 | 1 | 3 | 2 | 0 |
| 1 | 4 | 5 | 2 | |
| 2 | 9 | 7 | | |
| 3 | 16 | | | |

**Pattern**: n+1 points → n first differences → n-1 second differences → ... → 1 nth difference

### 💡 Memory Trick
**"Δ looks like a triangle pointing forward → Forward difference"**
**"∇ looks like a triangle pointing backward → Backward difference"**

---

## 4. GREGORY-NEWTON FORWARD INTERPOLATION

### 🧠 When to Use

- Data points are **equally spaced**
- You want to interpolate near the **BEGINNING** of the table

### 📐 Setup

Let nodes be equally spaced: xᵢ = x₀ + ih, where h is the step size.

Define: **u = (x - x₀)/h** (how many steps from the start)

### 📐 The Formula

```
P(x) = y₀ + uΔy₀ + [u(u-1)/2!]Δ²y₀ + [u(u-1)(u-2)/3!]Δ³y₀ + ...

P(x) = Σₖ₌₀ⁿ C(u,k) × Δᵏy₀
```

Where C(u,k) = u(u-1)(u-2)...(u-k+1)/k! (generalized binomial coefficient)

### 📊 Step-by-Step Procedure

1. **Build the forward difference table**
2. **Calculate u = (x - x₀)/h**
3. **Apply the formula** using Δy₀, Δ²y₀, etc. (first row of differences)

### 📊 Worked Example

Find f(1.5) given:
| x | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| y | 1 | 2 | 9 | 28 |

**Step 1**: h = 1, x₀ = 0

**Step 2**: Forward difference table:
| x | y | Δy | Δ²y | Δ³y |
|---|---|-----|------|------|
| 0 | 1 | 1 | 6 | 6 |
| 1 | 2 | 7 | 12 | |
| 2 | 9 | 19 | | |
| 3 | 28 | | | |

**Step 3**: u = (1.5 - 0)/1 = 1.5

**Step 4**: Apply formula:
```
P(1.5) = 1 + 1.5(1) + [1.5(0.5)/2](6) + [1.5(0.5)(-0.5)/6](6)
       = 1 + 1.5 + 2.25 + (-0.375)
       = 4.375
```

### 💡 Memory Trick
**"Forward = From First, Near the Beginning"**
- Uses FIRST row of difference table
- Works best NEAR the BEGINNING

---

## 5. GREGORY-NEWTON BACKWARD INTERPOLATION

### 🧠 When to Use

- Data points are **equally spaced**
- You want to interpolate near the **END** of the table

### 📐 Setup

Define: **v = (x - xₙ)/h** (how many steps from the end)

Note: v will typically be negative or small positive.

### 📐 The Formula

```
P(x) = yₙ + v∇yₙ + [v(v+1)/2!]∇²yₙ + [v(v+1)(v+2)/3!]∇³yₙ + ...
```

### 📊 Step-by-Step Procedure

1. **Build the backward difference table**
2. **Calculate v = (x - xₙ)/h**
3. **Apply the formula** using ∇yₙ, ∇²yₙ, etc. (last row of differences)

### 📊 Worked Example

Same data, find f(2.5):
| x | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| y | 1 | 2 | 9 | 28 |

**Step 1**: xₙ = 3, h = 1

**Step 2**: Backward difference table (same numbers, different arrangement):
| x | y | ∇y | ∇²y | ∇³y |
|---|---|-----|------|------|
| 0 | 1 | | | |
| 1 | 2 | 1 | | |
| 2 | 9 | 7 | 6 | |
| 3 | 28 | 19 | 12 | 6 |

**Step 3**: v = (2.5 - 3)/1 = -0.5

**Step 4**: Apply formula:
```
P(2.5) = 28 + (-0.5)(19) + [(-0.5)(0.5)/2](12) + [(-0.5)(0.5)(1.5)/6](6)
       = 28 - 9.5 - 1.5 - 0.375
       = 16.625
```

### 💡 Memory Trick
**"Backward = From Back, Near the End"**
- Uses LAST row of difference table
- Works best NEAR the END

---

## 6. LAGRANGE'S INTERPOLATION

### 🧠 Intuition First

Lagrange's approach: Build special "basis polynomials" that are 1 at one point and 0 at all others, then combine them!

Like having a "dimmer switch" for each data point that lights up only its contribution.

### 📐 Lagrange Basis Polynomials

For n+1 points, define:
```
Lᵢ(x) = ∏ⱼ≠ᵢ (x - xⱼ)/(xᵢ - xⱼ)
```

**Properties**:
- Lᵢ(xᵢ) = 1 (product of (xᵢ - xⱼ)/(xᵢ - xⱼ) = 1)
- Lᵢ(xⱼ) = 0 for j ≠ i (numerator contains (x - xⱼ) term)

### 📐 The Interpolating Polynomial

```
P(x) = Σᵢ₌₀ⁿ yᵢ × Lᵢ(x)
```

Each term contributes yᵢ only at x = xᵢ!

### 📊 Worked Example

Interpolate through (1, 1), (2, 4), (3, 9)

**Basis polynomials**:
```
L₀(x) = [(x-2)(x-3)]/[(1-2)(1-3)] = [(x-2)(x-3)]/2

L₁(x) = [(x-1)(x-3)]/[(2-1)(2-3)] = [(x-1)(x-3)]/(-1)

L₂(x) = [(x-1)(x-2)]/[(3-1)(3-2)] = [(x-1)(x-2)]/2
```

**Interpolating polynomial**:
```
P(x) = 1 × [(x-2)(x-3)]/2 + 4 × [-(x-1)(x-3)] + 9 × [(x-1)(x-2)]/2
```

Simplifying: P(x) = x² (correct! Data was y = x²)

### ✅ Advantages of Lagrange

1. **Works for unequally spaced points**
2. **Easy to understand conceptually**
3. **No difference table needed**
4. **Direct formula**

### ❌ Disadvantages

1. **Adding a new point** requires recomputing everything
2. **Computationally expensive** for many points
3. **Numerically unstable** for many points

### 💡 Memory Trick
**"L for Lagrange = Lights up one point at a time"**

---

## 7. NEWTON'S DIVIDED DIFFERENCE INTERPOLATION

### 🧠 Intuition First

Newton's divided difference is like Lagrange but written more cleverly. It builds up the polynomial term by term, making it EASY to add new points without starting over.

### 📐 Divided Differences

**Zeroth divided difference**:
```
f[xᵢ] = f(xᵢ) = yᵢ
```

**First divided difference**:
```
f[xᵢ, xᵢ₊₁] = [f(xᵢ₊₁) - f(xᵢ)] / [xᵢ₊₁ - xᵢ]
```

**Second divided difference**:
```
f[xᵢ, xᵢ₊₁, xᵢ₊₂] = [f[xᵢ₊₁, xᵢ₊₂] - f[xᵢ, xᵢ₊₁]] / [xᵢ₊₂ - xᵢ]
```

**General formula**:
```
f[x₀, x₁, ..., xₙ] = [f[x₁, ..., xₙ] - f[x₀, ..., xₙ₋₁]] / [xₙ - x₀]
```

### 📐 Newton's Divided Difference Formula

```
P(x) = f[x₀] + f[x₀,x₁](x-x₀) + f[x₀,x₁,x₂](x-x₀)(x-x₁) + ...
     + f[x₀,...,xₙ](x-x₀)(x-x₁)...(x-xₙ₋₁)
```

### 📊 Divided Difference Table

For (1,1), (2,4), (4,16):

| xᵢ | f[xᵢ] | f[xᵢ,xᵢ₊₁] | f[xᵢ,xᵢ₊₁,xᵢ₊₂] |
|----|-------|-------------|------------------|
| 1 | 1 | (4-1)/(2-1)=3 | (6-3)/(4-1)=1 |
| 2 | 4 | (16-4)/(4-2)=6 | |
| 4 | 16 | | |

```
P(x) = 1 + 3(x-1) + 1(x-1)(x-2)
     = 1 + 3x - 3 + x² - 3x + 2
     = x²  ✓
```

### ✅ Major Advantage

**Adding a new point (x₃, y₃)**:
- Just compute ONE new divided difference column
- Add one more term to the polynomial
- No need to redo everything!

### 💡 Memory Trick
**"Divided Differences = Difference of neighbors / Distance of endpoints"**

---

# 📚 PART B: NUMERICAL INTEGRATION

---

## 8. DEFINITE INTEGRAL AND QUADRATURE

### 🧠 Intuition First

**Problem**: Calculate ∫ₐᵇ f(x) dx when you can't find an antiderivative.

**Solution**: Approximate the area using shapes you CAN calculate (rectangles, trapezoids, parabolas).

### 📐 The Basic Idea

Replace f(x) with an interpolating polynomial P(x), then integrate P(x):
```
∫ₐᵇ f(x) dx ≈ ∫ₐᵇ P(x) dx
```

Different polynomial degrees → Different formulas!

---

## 9. NEWTON-COTES QUADRATURE FORMULAS

### 📐 General Setup

- Divide [a, b] into n equal parts: h = (b-a)/n
- Nodes: xᵢ = a + ih for i = 0, 1, ..., n
- Interpolate f(x) by polynomial of degree n
- Integrate the polynomial

### 📐 Closed Newton-Cotes (includes endpoints)

| n | Name | Formula | Error Order |
|---|------|---------|-------------|
| 1 | Trapezoidal | h/2[f₀ + f₁] | O(h³) |
| 2 | Simpson's 1/3 | h/3[f₀ + 4f₁ + f₂] | O(h⁵) |
| 3 | Simpson's 3/8 | 3h/8[f₀ + 3f₁ + 3f₂ + f₃] | O(h⁵) |
| 4 | Boole's | 2h/45[7f₀ + 32f₁ + 12f₂ + 32f₃ + 7f₄] | O(h⁷) |

---

## 10. TRAPEZOIDAL RULE

### 🧠 Intuition First

Approximate the curve by a straight line (trapezoid shape). Like connecting dots with straight lines instead of the actual curve.

### 📐 Single Interval (Simple Trapezoidal)

```
∫ₐᵇ f(x) dx ≈ (b-a)/2 × [f(a) + f(b)]
```

This is just: Area of trapezoid = (sum of parallel sides)/2 × height

### 📐 Composite Trapezoidal Rule

For n subintervals of width h = (b-a)/n:

```
∫ₐᵇ f(x) dx ≈ h/2 [f₀ + 2f₁ + 2f₂ + ... + 2fₙ₋₁ + fₙ]

         = h/2 [f₀ + fₙ + 2Σᵢ₌₁ⁿ⁻¹ fᵢ]
```

### 📐 Error Analysis

**Single interval error**:
```
E = -h³/12 × f''(ξ)
```

**Composite error**:
```
E = -(b-a)h²/12 × f''(ξ) = O(h²)
```

Doubling n (halving h) reduces error by factor of 4!

### 📊 Worked Example

Calculate ∫₀² x² dx using n=4 trapezoids

h = 2/4 = 0.5

| xᵢ | 0 | 0.5 | 1 | 1.5 | 2 |
|----|---|-----|---|-----|---|
| fᵢ=xᵢ² | 0 | 0.25 | 1 | 2.25 | 4 |

```
I ≈ 0.5/2 × [0 + 2(0.25) + 2(1) + 2(2.25) + 4]
  = 0.25 × [0 + 0.5 + 2 + 4.5 + 4]
  = 0.25 × 11 = 2.75
```

Exact: ∫₀² x² dx = [x³/3]₀² = 8/3 ≈ 2.667

Error = 2.75 - 2.667 = 0.083

---

## 11. SIMPSON'S ONE-THIRD RULE

### 🧠 Intuition First

Instead of straight lines, fit PARABOLAS through every three consecutive points. Parabolas are smoother and hug curved functions better!

### 📐 Derivation

For three points (x₀, y₀), (x₁, y₁), (x₂, y₂) where x₁ - x₀ = x₂ - x₁ = h:

Fit a parabola P(x) and integrate:
```
∫ₓ₀ˣ² P(x) dx = h/3 × [f(x₀) + 4f(x₁) + f(x₂)]
```

The "1/3" comes from h/3!

### 📐 Composite Simpson's 1/3 Rule

**Requirement**: n must be EVEN (need pairs of intervals for parabolas)

For n subintervals:
```
∫ₐᵇ f(x) dx ≈ h/3 [f₀ + 4f₁ + 2f₂ + 4f₃ + 2f₄ + ... + 4fₙ₋₁ + fₙ]

         = h/3 [f₀ + fₙ + 4(f₁+f₃+f₅+...) + 2(f₂+f₄+f₆+...)]
```

**Pattern**: 1-4-2-4-2-...-4-2-4-1

### 📐 Error Analysis

**Single interval** (2 subintervals):
```
E = -h⁵/90 × f⁽⁴⁾(ξ)
```

**Composite** (n subintervals):
```
E = -(b-a)h⁴/180 × f⁽⁴⁾(ξ) = O(h⁴)
```

Doubling n reduces error by factor of 16! (Much better than trapezoidal)

### 📊 Worked Example

Calculate ∫₀² x² dx using Simpson's rule with n=4

h = 2/4 = 0.5

| xᵢ | 0 | 0.5 | 1 | 1.5 | 2 |
|----|---|-----|---|-----|---|
| fᵢ | 0 | 0.25 | 1 | 2.25 | 4 |

```
I ≈ 0.5/3 × [0 + 4(0.25) + 2(1) + 4(2.25) + 4]
  = (1/6) × [0 + 1 + 2 + 9 + 4]
  = (1/6) × 16 = 2.667
```

Exact = 8/3 = 2.667 - EXACT for polynomials of degree ≤ 3!

### 💡 Memory Trick
**"Simpson 1/3 = coefficients 1-4-2-4-...-4-1 and divide by 3"**

---

## 12. SIMPSON'S THREE-EIGHTHS RULE

### 🧠 When to Use

When you have 3 subintervals (4 points) - can't use 1/3 rule which needs even number.

### 📐 The Formula

For 3 equal subintervals:
```
∫ₓ₀ˣ³ f(x) dx ≈ 3h/8 × [f₀ + 3f₁ + 3f₂ + f₃]
```

**Pattern**: 1-3-3-1 with 3h/8 multiplier

### 📐 Composite Form

For n = 3m subintervals:
```
∫ₐᵇ f(x) dx ≈ 3h/8 [f₀ + 3f₁ + 3f₂ + 2f₃ + 3f₄ + 3f₅ + 2f₆ + ... + fₙ]
```

### 📐 Error

```
E = -(b-a)h⁴/80 × f⁽⁴⁾(ξ) = O(h⁴)
```

Same order as 1/3 rule but slightly larger constant.

### 💡 Memory Trick
**"3/8 rule = 1-3-3-1 pattern, three-eighths multiplier"**

---

## 13. ERRORS IN QUADRATURE FORMULAS

### 📊 Summary Table

| Method | Order | Error Term | Exact for Polynomials of Degree |
|--------|-------|------------|--------------------------------|
| Trapezoidal | O(h²) | -(b-a)h²f''/12 | ≤ 1 |
| Simpson 1/3 | O(h⁴) | -(b-a)h⁴f⁽⁴⁾/180 | ≤ 3 |
| Simpson 3/8 | O(h⁴) | -(b-a)h⁴f⁽⁴⁾/80 | ≤ 3 |

### 📐 Key Observation

Simpson's rules have O(h⁴) error even though they use quadratic/cubic polynomials!

**Why?** The errors cancel out due to symmetry - a "free" order of accuracy!

---

## 14. ROMBERG'S ALGORITHM

### 🧠 Intuition First

Use cheap calculations (Trapezoidal rule with different step sizes) and "extrapolate" to get the answer you would have gotten with infinitely many intervals!

Like predicting where a trend is going by looking at how it's changing.

### 📐 The Idea

If T(h) is the trapezoidal approximation with step h, we know:
```
Error = c₁h² + c₂h⁴ + c₃h⁶ + ...
```

By combining T(h) and T(h/2), we can ELIMINATE the h² error!

### 📐 Richardson Extrapolation

```
Improved estimate = [4×T(h/2) - T(h)] / 3
```

This gives Simpson's rule accuracy (O(h⁴))!

Continue to eliminate h⁴, h⁶, etc.

### 📐 Romberg Table Construction

Let Rₖ,ⱼ denote the entry in row k, column j.

**Column 1**: Trapezoidal estimates with h, h/2, h/4, ...
```
R₁,₁ = T(h)
R₂,₁ = T(h/2)
R₃,₁ = T(h/4)
...
```

**General formula**:
```
Rₖ,ⱼ = [4ʲ⁻¹ × Rₖ,ⱼ₋₁ - Rₖ₋₁,ⱼ₋₁] / (4ʲ⁻¹ - 1)
```

### 📊 Example Table Structure

```
R₁,₁
R₂,₁  R₂,₂
R₃,₁  R₃,₂  R₃,₃
R₄,₁  R₄,₂  R₄,₃  R₄,₄
```

- Column 1: O(h²) - Trapezoidal
- Column 2: O(h⁴) - Simpson's
- Column 3: O(h⁶) - Boole's
- Column 4: O(h⁸)

### 💡 Memory Trick
**"Romberg = Trapezoidal + Richardson magic = high accuracy cheaply"**

---

## 15. GAUSSIAN QUADRATURE

### 🧠 Intuition First

Newton-Cotes uses equally spaced points. What if we choose point locations OPTIMALLY for accuracy?

Gaussian quadrature does exactly this - it picks the BEST positions for evaluation points!

### 📐 The Idea

Approximate: ∫₋₁¹ f(x) dx ≈ Σᵢ wᵢ f(xᵢ)

Choose both the weights wᵢ AND the nodes xᵢ to maximize accuracy!

### 📐 Key Result

With n points, Gaussian quadrature is exact for polynomials of degree ≤ 2n-1!

Compare: Newton-Cotes with n points is exact only for degree ≤ n-1 or n.

### 📊 Gaussian Quadrature Nodes and Weights

For ∫₋₁¹ f(x) dx:

**n = 1 (Midpoint)**:
- x₁ = 0, w₁ = 2
- Exact for degree ≤ 1

**n = 2**:
- x₁ = -1/√3 ≈ -0.5774, w₁ = 1
- x₂ = 1/√3 ≈ 0.5774, w₂ = 1
- Exact for degree ≤ 3

**n = 3**:
- x₁ = -√(3/5) ≈ -0.7746, w₁ = 5/9
- x₂ = 0, w₂ = 8/9
- x₃ = √(3/5) ≈ 0.7746, w₃ = 5/9
- Exact for degree ≤ 5

### 📐 Changing Interval from [-1, 1] to [a, b]

Transform: t = (b-a)x/2 + (a+b)/2

Then: ∫ₐᵇ f(t) dt = (b-a)/2 × ∫₋₁¹ f((b-a)x/2 + (a+b)/2) dx

Apply Gaussian quadrature to the transformed function!

### 📊 Worked Example

Calculate ∫₀¹ eˣ dx using 2-point Gaussian quadrature

Transform: t = (x+1)/2, so x = 2t-1

∫₀¹ eᵗ dt = (1/2) ∫₋₁¹ e^((x+1)/2) dx

Nodes: x = ±1/√3 → t = (1 ± 1/√3)/2

```
I ≈ (1/2) × [1 × e^((1-1/√3)/2) + 1 × e^((1+1/√3)/2)]
  ≈ (1/2) × [e^0.2113 + e^0.7887]
  ≈ (1/2) × [1.2353 + 2.2004]
  ≈ 1.7179
```

Exact: e - 1 ≈ 1.7183

Error ≈ 0.0004 (amazing with just 2 points!)

### 💡 Memory Trick
**"Gaussian = Smart placement beats brute force!"**

---

# 📊 UNIT 2 SUMMARY TABLE

| Method | Type | Prerequisites | Best For | Error Order |
|--------|------|---------------|----------|-------------|
| Newton Forward | Interpolation | Equal spacing | Near start | - |
| Newton Backward | Interpolation | Equal spacing | Near end | - |
| Lagrange | Interpolation | Any spacing | Simple problems | - |
| Divided Diff | Interpolation | Any spacing | Adding points | - |
| Trapezoidal | Integration | None | Quick estimate | O(h²) |
| Simpson 1/3 | Integration | Even n | Most cases | O(h⁴) |
| Simpson 3/8 | Integration | n = 3k | n = 3, 6, 9... | O(h⁴) |
| Romberg | Integration | None | High accuracy | O(h^2k) |
| Gaussian | Integration | None | Smooth functions | O(h^2n) |

---

# 🎯 EXAM CHECKLIST - UNIT 2

□ Can build forward/backward difference tables?
□ Know when to use forward vs backward interpolation?
□ Can apply Lagrange's formula?
□ Understand divided differences?
□ Can derive simple trapezoidal rule?
□ Know Simpson's 1/3 pattern (1-4-2-4-...-4-1)?
□ Know Simpson's 3/8 pattern (1-3-3-1)?
□ Understand Romberg extrapolation concept?
□ Know 2-point Gaussian nodes (±1/√3)?
□ Can transform integrals for Gaussian quadrature?
