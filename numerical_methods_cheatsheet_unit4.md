# 🎯 NUMERICAL METHODS COMPLETE CHEATSHEET
## UNIT 4: Ordinary & Partial Differential Equations

---

# 📚 PART A: ORDINARY DIFFERENTIAL EQUATIONS (ODEs)

---

## 1. INTRODUCTION TO NUMERICAL ODE METHODS

### 🧠 Intuition First

A differential equation describes HOW a quantity changes. For example:
- dy/dx = 2x means "y changes at rate 2x"
- Given y(0) = 1, what is y(3)?

Often we can't solve analytically, so we "march forward" in small steps, predicting the next value from the current one.

### 📐 Standard Form: Initial Value Problem (IVP)

```
dy/dx = f(x, y)
y(x₀) = y₀
```

**Goal**: Find y(x) for x > x₀

### 📐 Classification of Methods

| Type | Description | Examples |
|------|-------------|----------|
| Single-step | Uses only current point | Euler, Runge-Kutta |
| Multi-step | Uses several previous points | Adams, Milne |
| Predictor-Corrector | Predict then refine | Adams-Moulton |
| Explicit | yₙ₊₁ computed directly | Forward Euler |
| Implicit | yₙ₊₁ appears on both sides | Backward Euler |

---

## 2. PICARD'S METHOD (Successive Approximation)

### 🧠 Intuition First

Start with a guess for y(x), plug it into the differential equation, integrate to get a better guess. Repeat until convergence!

### 📐 The Method

Transform dy/dx = f(x,y), y(x₀) = y₀ into integral form:

```
y(x) = y₀ + ∫ₓ₀ˣ f(t, y(t)) dt
```

**Iteration**:
```
y⁽⁰⁾(x) = y₀  (initial guess - constant)
y⁽ⁿ⁺¹⁾(x) = y₀ + ∫ₓ₀ˣ f(t, y⁽ⁿ⁾(t)) dt
```

### 📊 Worked Example

Solve: dy/dx = x + y, y(0) = 1

**Iteration 0**: y⁽⁰⁾ = 1

**Iteration 1**:
```
y⁽¹⁾ = 1 + ∫₀ˣ (t + 1) dt = 1 + [t²/2 + t]₀ˣ = 1 + x²/2 + x
```

**Iteration 2**:
```
y⁽²⁾ = 1 + ∫₀ˣ (t + 1 + t²/2 + t) dt
     = 1 + ∫₀ˣ (1 + 2t + t²/2) dt
     = 1 + x + x² + x³/6
```

**Iteration 3**:
```
y⁽³⁾ = 1 + x + x² + x³/3 + x⁴/24
```

Pattern: Approaches y = 2eˣ - x - 1 (exact solution!)

### ⚠️ Limitations

1. **Tedious integration** required at each step
2. **Slow convergence** for many problems
3. **Theoretical importance** but rarely used in practice

### 💡 Memory Trick
**"Picard = Plug In, Calculate Integral, Repeat, Done (eventually)"**

---

## 3. TAYLOR SERIES METHOD

### 🧠 Intuition First

Use Taylor series to predict y at the next step, computing higher derivatives from the differential equation itself.

### 📐 The Method

Expand y(x+h) around x:
```
y(x+h) = y(x) + hy'(x) + (h²/2!)y''(x) + (h³/3!)y'''(x) + ...
```

From dy/dx = f(x,y), we get:
```
y' = f(x,y)
y'' = df/dx = ∂f/∂x + (∂f/∂y)(dy/dx) = fₓ + f·fᵧ
y''' = d²f/dx² = ... (chain rule, more complex)
```

### 📐 Taylor Series of Order p

```
yₙ₊₁ = yₙ + hf + (h²/2!)f' + ... + (hᵖ/p!)f⁽ᵖ⁻¹⁾
```

**Truncation Error**: O(hᵖ⁺¹)

### 📊 Worked Example

Solve: dy/dx = x + y, y(0) = 1, find y(0.1) using 3rd order Taylor

At x = 0, y = 1:
```
y' = f = x + y = 0 + 1 = 1
y'' = 1 + y' = 1 + 1 = 2
y''' = y'' = 2
```

```
y(0.1) = 1 + (0.1)(1) + (0.01/2)(2) + (0.001/6)(2)
       = 1 + 0.1 + 0.01 + 0.00033
       = 1.11033
```

Exact: y = 2e⁰·¹ - 0.1 - 1 = 1.11034 ✓

### ⚠️ Limitations

1. **Derivatives become complex** for high orders
2. **Tedious for complicated f(x,y)**
3. Works well when derivatives are easy to compute

---

## 4. EULER'S METHOD (Forward Euler)

### 🧠 Intuition First

The simplest possible method! Just follow the tangent line from current point to the next.

Like driving while only looking at the direction of your steering wheel - you go straight in that direction for a small distance.

### 📐 The Formula

```
yₙ₊₁ = yₙ + h·f(xₙ, yₙ)
```

This is Taylor series truncated after the LINEAR term!

### 🎨 Visual Understanding

```
    True solution
        ___---
    ___/   
   /    Euler approximation (tangent)
  /   ___----
 /___/
●──────────●
xₙ         xₙ₊₁
```

We follow the tangent, which drifts away from the true curve.

### 📐 Algorithm

```
Input: f, x₀, y₀, h, N (number of steps)

For n = 0, 1, ..., N-1:
    xₙ₊₁ = xₙ + h
    yₙ₊₁ = yₙ + h × f(xₙ, yₙ)
    
Return sequence (xₙ, yₙ)
```

### 📊 Worked Example

Solve: dy/dx = x + y, y(0) = 1, find y(0.2) with h = 0.1

| n | xₙ | yₙ | f(xₙ,yₙ)=xₙ+yₙ | yₙ₊₁ = yₙ + 0.1f |
|---|-----|------|------------------|------------------|
| 0 | 0 | 1 | 1 | 1.1 |
| 1 | 0.1 | 1.1 | 1.2 | 1.22 |
| 2 | 0.2 | 1.22 | | |

So y(0.2) ≈ 1.22

Exact: y(0.2) = 2e⁰·² - 0.2 - 1 ≈ 1.2428

Error ≈ 0.023

### 📐 Error Analysis

**Local Truncation Error** (error in one step, assuming no prior error):
```
LTE = |y(xₙ₊₁) - yₙ₊₁| = O(h²)
```

**Global Error** (accumulated error after N steps where Nh = constant):
```
GE = O(h)
```

Why? N ∝ 1/h, so N × LTE = (1/h) × h² = h

**Euler is a first-order method**: Global error O(h)

### ⚠️ Stability Issues

For the test equation y' = λy:
- Euler is stable only if |1 + λh| < 1
- For λ < 0 (decay): need h < 2/|λ|
- For stiff equations (large |λ|): tiny h required!

### 💡 Memory Trick
**"Euler = Easy + Erroneous (simple but inaccurate)"**

---

## 5. RUNGE-KUTTA METHODS

### 🧠 Intuition First

Euler uses the slope at the START of the interval. What if we sampled the slope at MULTIPLE points and averaged them? This is the Runge-Kutta idea!

Like navigating by checking your compass several times along the way, not just at the start.

### 📐 General Runge-Kutta Form

```
yₙ₊₁ = yₙ + h × (weighted average of slope estimates)
```

### 📐 Midpoint Method (RK2)

**Idea**: Use slope at the MIDPOINT of the interval

```
k₁ = f(xₙ, yₙ)
k₂ = f(xₙ + h/2, yₙ + (h/2)k₁)
yₙ₊₁ = yₙ + h·k₂
```

**Order**: 2 (Global error O(h²))

### 📐 Heun's Method (Improved Euler / RK2 variant)

```
k₁ = f(xₙ, yₙ)
k₂ = f(xₙ + h, yₙ + h·k₁)
yₙ₊₁ = yₙ + (h/2)(k₁ + k₂)
```

Average of slopes at start and (predicted) end.

### 📐 Classical 4th Order Runge-Kutta (RK4) ⭐

**THE most widely used method!**

```
k₁ = f(xₙ, yₙ)
k₂ = f(xₙ + h/2, yₙ + (h/2)k₁)
k₃ = f(xₙ + h/2, yₙ + (h/2)k₂)
k₄ = f(xₙ + h, yₙ + h·k₃)

yₙ₊₁ = yₙ + (h/6)(k₁ + 2k₂ + 2k₃ + k₄)
```

**Memory device**: Weights are 1-2-2-1, divide by 6

### 📊 Worked Example (RK4)

Solve: dy/dx = x + y, y(0) = 1, find y(0.1) with h = 0.1

```
k₁ = f(0, 1) = 0 + 1 = 1
k₂ = f(0.05, 1 + 0.05×1) = f(0.05, 1.05) = 1.1
k₃ = f(0.05, 1 + 0.05×1.1) = f(0.05, 1.055) = 1.105
k₄ = f(0.1, 1 + 0.1×1.105) = f(0.1, 1.1105) = 1.2105

y(0.1) = 1 + (0.1/6)(1 + 2(1.1) + 2(1.105) + 1.2105)
       = 1 + (0.1/6)(6.6205)
       = 1 + 0.11034
       = 1.11034
```

Exact: 1.11034 - EXCELLENT agreement!

### 📐 Error Analysis for RK4

**Local Truncation Error**: O(h⁵)
**Global Error**: O(h⁴)

RK4 is a **fourth-order method** - very accurate!

### 📐 Computational Cost

| Method | Order | f evaluations per step |
|--------|-------|----------------------|
| Euler | 1 | 1 |
| RK2 (Midpoint) | 2 | 2 |
| RK4 (Classical) | 4 | 4 |

RK4 gives 4th order accuracy for 4 function evaluations - very efficient!

### 💡 Memory Trick for RK4
**"k's at 0, 1/2, 1/2, 1 of the step; weights 1-2-2-1; divide by 6"**

---

## 6. PREDICTOR-CORRECTOR METHODS

### 🧠 Intuition First

Two-phase approach:
1. **PREDICT**: Make a rough estimate using an explicit formula
2. **CORRECT**: Improve the estimate using an implicit formula

Like making a first draft, then editing it!

### 📐 Euler's Predictor-Corrector (Modified Euler)

**Predictor** (Forward Euler):
```
ỹₙ₊₁ = yₙ + h·f(xₙ, yₙ)
```

**Corrector** (Trapezoidal rule):
```
yₙ₊₁ = yₙ + (h/2)[f(xₙ, yₙ) + f(xₙ₊₁, ỹₙ₊₁)]
```

Can iterate the corrector for better accuracy!

### 📐 Order and Properties

- **Order 2** (same as RK2)
- More accurate than pure Euler
- Uses information from both ends of interval

---

## 7. ADAMS-BASHFORTH METHOD (Multi-step)

### 🧠 Intuition First

Why compute f at the current point only? Use the history! If you know how f has been behaving over the last few steps, you can predict the future better.

### 📐 General Idea

Multi-step methods use values from previous steps:
```
yₙ₊₁ = yₙ + h × (combination of fₙ, fₙ₋₁, fₙ₋₂, ...)
```

### 📐 Adams-Bashforth Formulas (Explicit)

**2-step (AB2)**:
```
yₙ₊₁ = yₙ + (h/2)(3fₙ - fₙ₋₁)
```

**3-step (AB3)**:
```
yₙ₊₁ = yₙ + (h/12)(23fₙ - 16fₙ₋₁ + 5fₙ₋₂)
```

**4-step (AB4)**:
```
yₙ₊₁ = yₙ + (h/24)(55fₙ - 59fₙ₋₁ + 37fₙ₋₂ - 9fₙ₋₃)
```

Where fⱼ = f(xⱼ, yⱼ)

### 📐 Starting Values

**Problem**: AB methods need y₀, y₁, ..., yₖ₋₁ to start!

**Solution**: Use RK4 or another single-step method to compute starting values.

### 📐 Adams-Moulton Formulas (Implicit)

**2-step (AM2 = Trapezoidal)**:
```
yₙ₊₁ = yₙ + (h/2)(fₙ₊₁ + fₙ)
```

**3-step (AM3)**:
```
yₙ₊₁ = yₙ + (h/12)(5fₙ₊₁ + 8fₙ - fₙ₋₁)
```

### 📐 Adams Predictor-Corrector

**Predictor**: Adams-Bashforth (explicit)
**Corrector**: Adams-Moulton (implicit)

Example (ABM4):
```
Predict: ỹₙ₊₁ = yₙ + (h/24)(55fₙ - 59fₙ₋₁ + 37fₙ₋₂ - 9fₙ₋₃)
Correct: yₙ₊₁ = yₙ + (h/24)(9f̃ₙ₊₁ + 19fₙ - 5fₙ₋₁ + fₙ₋₂)
```
where f̃ₙ₊₁ = f(xₙ₊₁, ỹₙ₊₁)

---

## 8. MILNE'S METHOD

### 📐 The Formulas

**Milne's Predictor** (uses 4 past values):
```
ỹₙ₊₁ = yₙ₋₃ + (4h/3)(2fₙ - fₙ₋₁ + 2fₙ₋₂)
```

**Milne's Corrector**:
```
yₙ₊₁ = yₙ₋₁ + (h/3)(fₙ₊₁ + 4fₙ + fₙ₋₁)
```

The corrector is Simpson's rule!

### 📐 Order

Milne's method is **4th order** (like RK4).

### ⚠️ Stability Issue

Milne's method is **weakly unstable**: small errors can grow over time, even for stable problems!

For long integrations, Adams methods are preferred.

---

# 📚 PART B: PARTIAL DIFFERENTIAL EQUATIONS (PDEs)

---

## 9. CLASSIFICATION OF PDEs

### 📐 Second-Order Linear PDEs

General form:
```
A·uₓₓ + B·uₓᵧ + C·uᵧᵧ + D·uₓ + E·uᵧ + F·u = G
```

**Classification** (based on discriminant B² - 4AC):

| Type | Condition | Example | Physical Meaning |
|------|-----------|---------|------------------|
| **Elliptic** | B² - 4AC < 0 | Laplace: uₓₓ + uᵧᵧ = 0 | Steady-state, equilibrium |
| **Parabolic** | B² - 4AC = 0 | Heat: uₜ = α·uₓₓ | Diffusion, time evolution |
| **Hyperbolic** | B² - 4AC > 0 | Wave: uₜₜ = c²·uₓₓ | Waves, vibrations |

### 💡 Memory Trick
**"Elliptic = Equilibrium, Parabolic = Heat, Hyperbolic = Waves"**

---

## 10. PARABOLIC EQUATIONS (Heat Equation)

### 📐 The Heat Equation

```
∂u/∂t = α · ∂²u/∂x²
```

With initial condition u(x, 0) = f(x) and boundary conditions.

### 📐 Discretization

**Grid**: Points (xᵢ, tⱼ) where xᵢ = ih, tⱼ = jk

**Notation**: uᵢʲ ≈ u(xᵢ, tⱼ)

**Finite Differences**:
```
∂u/∂t ≈ (uᵢʲ⁺¹ - uᵢʲ)/k  (forward in time)
∂²u/∂x² ≈ (uᵢ₊₁ʲ - 2uᵢʲ + uᵢ₋₁ʲ)/h²  (central in space)
```

### 📐 Explicit Method (FTCS)

Forward Time, Central Space:

```
uᵢʲ⁺¹ = uᵢʲ + r(uᵢ₊₁ʲ - 2uᵢʲ + uᵢ₋₁ʲ)
```

Where r = αk/h² (mesh ratio)

**Rearranged**:
```
uᵢʲ⁺¹ = r·uᵢ₋₁ʲ + (1-2r)·uᵢʲ + r·uᵢ₊₁ʲ
```

### 📐 Stability Condition

**CRITICAL**: The explicit method is stable only if:
```
r = αk/h² ≤ 1/2
```

If violated, the solution will oscillate wildly and blow up!

### 📐 Implicit Method (Backward Euler)

```
uᵢʲ⁺¹ - r(uᵢ₊₁ʲ⁺¹ - 2uᵢʲ⁺¹ + uᵢ₋₁ʲ⁺¹) = uᵢʲ
```

This is a tridiagonal system! Solve using Thomas algorithm.

**Advantage**: Unconditionally stable (any r)!

### 📐 Crank-Nicolson Method

Average of explicit and implicit (most popular!):

```
uᵢʲ⁺¹ - (r/2)(uᵢ₊₁ʲ⁺¹ - 2uᵢʲ⁺¹ + uᵢ₋₁ʲ⁺¹) = 
uᵢʲ + (r/2)(uᵢ₊₁ʲ - 2uᵢʲ + uᵢ₋₁ʲ)
```

**Properties**:
- Unconditionally stable
- Second-order accurate in both time and space: O(k² + h²)
- Solves a tridiagonal system per time step

### 💡 Memory Trick
**"Explicit = Easy but unstable; Implicit = Stable but solve system; Crank-Nicolson = Best of both!"**

---

## 11. HYPERBOLIC EQUATIONS (Wave Equation)

### 📐 The Wave Equation

```
∂²u/∂t² = c² · ∂²u/∂x²
```

With initial conditions u(x,0) = f(x), uₜ(x,0) = g(x)

### 📐 Explicit Finite Difference Scheme

```
uᵢʲ⁺¹ = 2uᵢʲ - uᵢʲ⁻¹ + λ²(uᵢ₊₁ʲ - 2uᵢʲ + uᵢ₋₁ʲ)
```

Where λ = ck/h (Courant number)

### 📐 Stability Condition (CFL)

**Courant-Friedrichs-Lewy (CFL) condition**:
```
λ = ck/h ≤ 1
```

**Physical meaning**: The numerical domain of dependence must contain the physical domain of dependence.

### 📐 Starting the Method

Need u at j = 0 AND j = 1 to start!

For j = 1, use initial conditions:
```
u(x, k) ≈ u(x, 0) + k·uₜ(x, 0) = f(x) + k·g(x)
```

Or more accurately:
```
uᵢ¹ = (1-λ²)f(xᵢ) + (λ²/2)[f(xᵢ₊₁) + f(xᵢ₋₁)] + k·g(xᵢ)
```

---

## 12. ELLIPTIC EQUATIONS (Laplace/Poisson)

### 📐 Laplace's Equation

```
∂²u/∂x² + ∂²u/∂y² = 0  (Laplace)
∂²u/∂x² + ∂²u/∂y² = f(x,y)  (Poisson)
```

**Physical meaning**: Steady-state temperature, potential fields, equilibrium distributions.

### 📐 Five-Point Formula

Discretize on a grid with spacing h:

```
uᵢ₊₁,ⱼ + uᵢ₋₁,ⱼ + uᵢ,ⱼ₊₁ + uᵢ,ⱼ₋₁ - 4uᵢ,ⱼ = h²fᵢ,ⱼ
```

Or for Laplace (f = 0):
```
uᵢ,ⱼ = (uᵢ₊₁,ⱼ + uᵢ₋₁,ⱼ + uᵢ,ⱼ₊₁ + uᵢ,ⱼ₋₁)/4
```

**The value at each interior point is the AVERAGE of its four neighbors!**

### 📐 Solution Methods

**Direct Methods**: Form the big linear system, solve with LU decomposition
- For N interior points: N×N system!

**Iterative Methods** (preferred for large problems):

**Jacobi Iteration**:
```
uᵢ,ⱼ⁽ⁿ⁺¹⁾ = (uᵢ₊₁,ⱼ⁽ⁿ⁾ + uᵢ₋₁,ⱼ⁽ⁿ⁾ + uᵢ,ⱼ₊₁⁽ⁿ⁾ + uᵢ,ⱼ₋₁⁽ⁿ⁾)/4
```
All updates use OLD values only.

**Gauss-Seidel Iteration**:
```
uᵢ,ⱼ⁽ⁿ⁺¹⁾ = (uᵢ₊₁,ⱼ⁽ⁿ⁾ + uᵢ₋₁,ⱼ⁽ⁿ⁺¹⁾ + uᵢ,ⱼ₊₁⁽ⁿ⁾ + uᵢ,ⱼ₋₁⁽ⁿ⁺¹⁾)/4
```
Use NEW values as soon as available - converges faster!

**SOR (Successive Over-Relaxation)**:
```
uᵢ,ⱼ⁽ⁿ⁺¹⁾ = (1-ω)uᵢ,ⱼ⁽ⁿ⁾ + ω × (Gauss-Seidel update)
```
Where 1 < ω < 2 (optimal ω depends on problem)

### 📐 Convergence

| Method | Convergence Rate |
|--------|-----------------|
| Jacobi | Slow |
| Gauss-Seidel | 2× faster than Jacobi |
| SOR (optimal ω) | Much faster |

---

# 📊 UNIT 4 SUMMARY TABLE

| Method | Type | Order | Key Feature | Best For |
|--------|------|-------|-------------|----------|
| Picard | ODE | - | Theoretical | Understanding |
| Taylor | ODE | p (chosen) | Uses derivatives | Simple f(x,y) |
| Euler | ODE | 1 | Simplest | Teaching, rough estimates |
| RK2 | ODE | 2 | 2 evaluations | Moderate accuracy |
| RK4 | ODE | 4 | 4 evaluations | **Most practical** |
| Adams-Bashforth | ODE (multi) | 1-4 | Uses history | Long integrations |
| Milne | ODE (multi) | 4 | Simpson-like | Not recommended (unstable) |
| FTCS (Explicit) | Heat PDE | 1 | Simple | When r ≤ 0.5 |
| Implicit | Heat PDE | 1 | Always stable | Stiff problems |
| Crank-Nicolson | Heat PDE | 2 | Best accuracy | **Most practical** |
| Explicit | Wave PDE | 2 | Simple | When λ ≤ 1 |
| Five-point | Elliptic | 2 | Averaging | Laplace/Poisson |

---

# 🎯 EXAM CHECKLIST - UNIT 4

## ODEs
□ Can apply Picard's iteration for one or two steps?
□ Know Euler's formula: yₙ₊₁ = yₙ + hf?
□ Can derive local and global error order of Euler?
□ Know RK4 formula (k₁, k₂, k₃, k₄ and weights)?
□ Can compute one step of RK4?
□ Understand predictor-corrector concept?
□ Know Adams-Bashforth is multi-step, needs starting values?

## PDEs
□ Can classify PDE as elliptic/parabolic/hyperbolic?
□ Know heat equation discretization (explicit/implicit)?
□ Know stability condition r ≤ 1/2 for explicit heat?
□ Know wave equation stability condition λ ≤ 1?
□ Know five-point formula for Laplace?
□ Understand Jacobi vs Gauss-Seidel iteration?

---

# 🏆 GRAND SUMMARY: ALL UNITS

## Key Formulas to MEMORIZE

### Unit 1
- Taylor: f(x) = Σ f⁽ⁿ⁾(a)(x-a)ⁿ/n!
- Newton: xₙ₊₁ = xₙ - f(xₙ)/f'(xₙ)
- Secant: xₙ₊₁ = xₙ - f(xₙ)(xₙ-xₙ₋₁)/(f(xₙ)-f(xₙ₋₁))

### Unit 2
- Lagrange: P(x) = Σ yᵢLᵢ(x)
- Trapezoidal: ∫ ≈ h[f₀ + 2f₁ + ... + 2fₙ₋₁ + fₙ]/2
- Simpson: ∫ ≈ h[f₀ + 4f₁ + 2f₂ + 4f₃ + ... + fₙ]/3

### Unit 3
- Gauss: Forward elimination → Back substitution
- LU: A = LU, then Ly = b, Ux = y
- Power: x⁽ᵏ⁾ = Ax⁽ᵏ⁻¹⁾/||Ax⁽ᵏ⁻¹⁾||

### Unit 4
- Euler: yₙ₊₁ = yₙ + hf(xₙ,yₙ)
- RK4: yₙ₊₁ = yₙ + h(k₁ + 2k₂ + 2k₃ + k₄)/6
- Heat explicit: uᵢʲ⁺¹ = r·uᵢ₋₁ʲ + (1-2r)·uᵢʲ + r·uᵢ₊₁ʲ

## Error Orders Quick Reference

| Method | Error Order |
|--------|-------------|
| Bisection | O(1/2ⁿ) - linear |
| Newton | O(eₙ²) - quadratic |
| Secant | O(eₙ^1.618) |
| Trapezoidal | O(h²) |
| Simpson 1/3 | O(h⁴) |
| Gauss Quadrature (n pts) | O(h^2n) |
| Euler | O(h) |
| RK4 | O(h⁴) |

## When to Use What

| Problem | Best Method |
|---------|-------------|
| Root finding, have f' | Newton |
| Root finding, no f' | Secant or Bisection |
| Integration, smooth function | Simpson or Gaussian |
| Interpolation, equally spaced | Newton Forward/Backward |
| Interpolation, unequal spacing | Lagrange or Divided Diff |
| Linear system, one RHS | Gauss elimination |
| Linear system, multiple RHS | LU decomposition |
| ODE, general purpose | RK4 |
| Heat equation | Crank-Nicolson |
| Laplace equation | SOR iteration |

---

# 💪 FINAL EXAM TIPS

1. **Show ALL Steps**: Partial credit for methodology!
2. **State Formulas First**: Write the formula before plugging in numbers
3. **Check Conditions**: Note assumptions (converges if..., stable if...)
4. **Organize Tables**: Use neat tables for iterative methods
5. **Error Analysis**: Always mention error order when asked
6. **Units and Precision**: Carry enough decimal places
7. **Verify When Possible**: Substitute answer back to check
8. **Time Management**: Don't get stuck on one problem

**GOOD LUCK! 🎯**
