# 🎯 NUMERICAL METHODS COMPLETE CHEATSHEET
## UNIT 1: Foundations, Root Finding & Optimization

---

# 📚 PART A: MATHEMATICAL FOUNDATIONS

---

## 1. TAYLOR SERIES

### 🧠 Intuition First (Think Like a 15-Year-Old)

Imagine you're trying to describe a curvy road to a friend who can only understand straight lines. The Taylor series is like giving them better and better descriptions:

1. **First attempt**: "The road is at height 5" (just a point - constant)
2. **Better**: "The road is at height 5, going uphill" (straight line - linear)
3. **Even better**: "The road is at height 5, going uphill, and curving left" (parabola - quadratic)
4. **More terms**: Each additional term captures more curvature details!

**Real-World Analogy**: Think of it like zooming in on Google Maps. At first, a mountain looks like a point. Zoom in, it's a triangle. Zoom more, you see the actual curves. Taylor series is mathematical "zooming in."

### 📐 The Mathematical Definition

The Taylor series of a function f(x) about a point x = a is:

```
f(x) = f(a) + f'(a)(x-a) + f''(a)(x-a)²/2! + f'''(a)(x-a)³/3! + ...
```

**General Formula:**
```
f(x) = Σ [f⁽ⁿ⁾(a)/n!] × (x-a)ⁿ  for n = 0 to ∞
```

**Special Case - Maclaurin Series (when a = 0):**
```
f(x) = f(0) + f'(0)x + f''(0)x²/2! + f'''(0)x³/3! + ...
```

### 📝 Step-by-Step Derivation

**Why does this formula work?**

1. We want to approximate f(x) as a polynomial: P(x) = c₀ + c₁(x-a) + c₂(x-a)² + ...
2. At x = a: P(a) = c₀ = f(a) ✓
3. Take first derivative: P'(x) = c₁ + 2c₂(x-a) + 3c₃(x-a)² + ...
   - At x = a: P'(a) = c₁ = f'(a) ✓
4. Take second derivative: P''(x) = 2c₂ + 6c₃(x-a) + ...
   - At x = a: P''(a) = 2c₂ = f''(a), so c₂ = f''(a)/2! ✓
5. Pattern continues: cₙ = f⁽ⁿ⁾(a)/n!

### 🔥 Important Taylor Series to MEMORIZE

| Function | Taylor Series (around 0) | Convergence |
|----------|-------------------------|-------------|
| eˣ | 1 + x + x²/2! + x³/3! + ... | All x |
| sin(x) | x - x³/3! + x⁵/5! - x⁷/7! + ... | All x |
| cos(x) | 1 - x²/2! + x⁴/4! - x⁶/6! + ... | All x |
| ln(1+x) | x - x²/2 + x³/3 - x⁴/4 + ... | -1 < x ≤ 1 |
| 1/(1-x) | 1 + x + x² + x³ + ... | |x| < 1 |
| (1+x)ⁿ | 1 + nx + n(n-1)x²/2! + ... | |x| < 1 |

### ⚠️ Taylor's Theorem with Remainder

**CRITICAL FOR EXAMS!**

When we truncate the series at n terms, there's an ERROR. Taylor's theorem tells us:

```
f(x) = Pₙ(x) + Rₙ(x)
```

Where the remainder (Lagrange form):
```
Rₙ(x) = f⁽ⁿ⁺¹⁾(ξ) × (x-a)ⁿ⁺¹ / (n+1)!
```
where ξ is some point between a and x.

**Error Bound:**
```
|Rₙ(x)| ≤ M × |x-a|ⁿ⁺¹ / (n+1)!
```
where M = max|f⁽ⁿ⁺¹⁾(t)| for t between a and x.

### 💡 Memory Trick
**"FIND THE DERIVATIVE, PLUG THE POINT, DIVIDE BY FACTORIAL"**
- F: Find all derivatives
- P: Plug in the center point a
- D: Divide by n!

### 📝 Exam Tips
1. Always check if the series converges at your point!
2. The more terms you use, the better the approximation (usually)
3. Works best when x is close to a
4. Look for patterns in derivatives (sin, cos cycle every 4)

---

## 2. ROLLE'S THEOREM

### 🧠 Intuition First

Imagine throwing a ball in the air. It goes up, reaches a peak, comes down. At the peak, the ball is momentarily stationary - velocity is ZERO!

Rolle's Theorem says: If you start and end at the same height (on a smooth curve), somewhere in between, the curve must be flat (horizontal tangent).

### 📐 Formal Statement

**If:**
1. f(x) is continuous on closed interval [a, b]
2. f(x) is differentiable on open interval (a, b)
3. f(a) = f(b)

**Then:**
There exists at least one point c ∈ (a, b) such that f'(c) = 0

### 🎨 Visual Understanding

```
        f'(c) = 0 (horizontal tangent)
            ↓
           ∩∩∩
          /    \
         /      \
        /        \
    f(a)          f(b)  ← Same height!
    ●──────────────●
    a      c       b
```

### ⚠️ All Three Conditions Are NECESSARY!

**Counterexamples:**

1. **Not continuous**: f(x) = 1/x on [-1, 1] with f(0) undefined
2. **Not differentiable**: f(x) = |x| on [-1, 1] - sharp corner at 0
3. **f(a) ≠ f(b)**: f(x) = x on [0, 1] - no horizontal tangent

### 💡 Memory Trick
**"SAME START, SAME END, SOMEWHERE FLAT"** 
Or think of a roller coaster that starts and ends at the same height - must have a peak or valley!

---

## 3. MEAN VALUE THEOREM (MVT)

### 🧠 Intuition First

Imagine driving from City A to City B, 100 km apart, in 2 hours. Your average speed is 50 km/h. MVT says: At some point during your trip, your speedometer showed EXACTLY 50 km/h!

### 📐 Formal Statement

**If:**
1. f(x) is continuous on [a, b]
2. f(x) is differentiable on (a, b)

**Then:**
There exists at least one c ∈ (a, b) such that:
```
f'(c) = [f(b) - f(a)] / (b - a)
```

### 🎨 Visual Understanding

```
                    Tangent at c (same slope as secant)
                   /
                  /
        ●───────●───────●
       /       c         \
      /                   \
     ●─────────────────────●  ← Secant line (average slope)
     a                     b
```

The tangent line at some point c is PARALLEL to the secant line connecting endpoints!

### 🔗 Relationship to Rolle's Theorem

MVT is a GENERALIZATION of Rolle's Theorem!

If f(a) = f(b), then:
- Secant slope = [f(b) - f(a)]/(b-a) = 0
- So f'(c) = 0 → Rolle's Theorem!

### 📝 Important Applications

1. **Proving inequalities**: Show that |sin(x) - sin(y)| ≤ |x - y|
2. **Showing functions are constant**: If f'(x) = 0 everywhere, f is constant
3. **Error analysis in numerical methods**

### 💡 Memory Trick
**"Average = Instant Somewhere"**
The AVERAGE rate of change equals the INSTANTANEOUS rate somewhere!

---

# 📚 PART B: ERRORS AND COMPUTER ARITHMETIC

---

## 4. APPROXIMATIONS AND ERRORS

### 🧠 Intuition First

Every measurement has some error. When you say you're "5 feet 10 inches tall," you might actually be 5'10.3" - the 0.3" is an error. In numerical computing, we can't represent most numbers exactly, so errors creep in.

### 📐 Types of Errors

#### 1. Absolute Error
```
Absolute Error = |True Value - Approximate Value|
                = |x - x̃|
```

**Example**: True value = π = 3.14159..., Approximate = 3.14
- Absolute Error = |3.14159... - 3.14| ≈ 0.00159

#### 2. Relative Error
```
Relative Error = |True Value - Approximate Value| / |True Value|
               = |x - x̃| / |x|
```

**Example**: Same as above
- Relative Error = 0.00159 / 3.14159 ≈ 0.0005 = 0.05%

#### 3. Percentage Error
```
Percentage Error = Relative Error × 100%
```

### 🔥 Why Relative Error Matters More

**Scenario 1**: Error of 1 meter when measuring Earth-Sun distance (150 billion meters)
- Relative error ≈ 0.0000000001% → Negligible!

**Scenario 2**: Error of 1 meter when measuring your height (1.7 meters)
- Relative error ≈ 59% → Completely wrong!

Same absolute error, VASTLY different significance!

### 📐 Sources of Error

| Error Type | Description | Example |
|------------|-------------|---------|
| **Inherent Error** | Error in input data | Measuring with a faulty ruler |
| **Truncation Error** | Cutting off infinite series | Using only 3 terms of Taylor series |
| **Round-off Error** | Limited decimal places | 1/3 = 0.333... stored as 0.33 |
| **Human Error** | Mistakes in calculation | Pressing wrong button |

### ⚡ Significant Figures

**Definition**: Digits that carry meaningful information

**Rules:**
1. All non-zero digits are significant: 123 has 3 sig figs
2. Zeros between non-zeros are significant: 1003 has 4 sig figs
3. Leading zeros are NOT significant: 0.0052 has 2 sig figs
4. Trailing zeros after decimal ARE significant: 2.50 has 3 sig figs

**Relationship to Error:**
If x = 3.14159 is rounded to x̃ = 3.14 (3 sig figs), then:
```
|x - x̃| < 0.5 × 10⁻² = 0.005
```

### 📝 Exam Formula: Significant Figures and Error

A number x̃ approximates x to n significant figures if:
```
|x - x̃| / |x| < 0.5 × 10⁻⁽ⁿ⁻¹⁾
```

---

## 5. DATA REPRESENTATION AND COMPUTER ARITHMETIC

### 🧠 Intuition First

Computers are like people who can only count using a limited alphabet. Imagine if you could only use digits 0-9 and had exactly 10 boxes to write a number. You'd face problems with very large numbers (don't fit) or very precise decimals (not enough boxes).

### 📐 Floating-Point Representation (IEEE 754)

A number is stored as:
```
x = ± m × bᵉ
```

Where:
- **±** = Sign (1 bit: 0 for positive, 1 for negative)
- **m** = Mantissa/Significand (fractional part, normalized)
- **b** = Base (usually 2 for computers)
- **e** = Exponent

**IEEE 754 Single Precision (32-bit):**
```
| 1 bit | 8 bits   | 23 bits   |
| Sign  | Exponent | Mantissa  |
```

**IEEE 754 Double Precision (64-bit):**
```
| 1 bit | 11 bits  | 52 bits   |
| Sign  | Exponent | Mantissa  |
```

### 🔢 Example: Storing 6.75 in Binary

1. Convert to binary: 6.75₁₀ = 110.11₂
2. Normalize: 110.11 = 1.1011 × 2²
3. Store:
   - Sign: 0 (positive)
   - Exponent: 2 + 127 (bias) = 129 = 10000001₂
   - Mantissa: 1011 (the 1 before decimal is implicit!)

### ⚠️ Machine Epsilon (ε_mach)

**Definition**: The smallest number such that 1 + ε ≠ 1 in computer arithmetic

```
Single Precision: ε_mach ≈ 1.2 × 10⁻⁷
Double Precision: ε_mach ≈ 2.2 × 10⁻¹⁶
```

**Why it matters**: Any number smaller than ε_mach times your number will be "invisible" when added!

### 📐 Overflow and Underflow

**Overflow**: Number too large to represent
- Single precision max: ≈ 3.4 × 10³⁸
- Result: ∞ (infinity)

**Underflow**: Number too small (close to zero)
- Single precision min: ≈ 1.2 × 10⁻³⁸
- Result: 0 (dangerous - silent error!)

---

## 6. LOSS OF SIGNIFICANCE (CATASTROPHIC CANCELLATION)

### 🧠 Intuition First

Imagine measuring two buildings that are almost the same height:
- Building A: 100.523 meters (measured to 6 sig figs)
- Building B: 100.519 meters (measured to 6 sig figs)
- Difference: 0.004 meters (only 1 sig fig!)

You started with 6 significant figures but ended with only 1! Three is the "catastrophic cancellation."

### 📐 The Mathematical Problem

When subtracting nearly equal numbers:
```
x = 1.23456789
y = 1.23456788
x - y = 0.00000001
```

If we only have 8 decimal places of precision:
- x stored as: 1.2345679 (rounded)
- y stored as: 1.2345679 (rounded)  
- Computed x - y = 0 (COMPLETELY WRONG!)

The true answer (0.00000001) is lost!

### 🔥 Classic Example: Quadratic Formula

For ax² + bx + c = 0, the standard formula is:
```
x = (-b ± √(b² - 4ac)) / 2a
```

**Problem**: When b² >> 4ac, we have √(b² - 4ac) ≈ |b|

For the root with +√ when b > 0 (or -√ when b < 0):
```
x = (-b + √(b² - 4ac)) / 2a ≈ (-b + b) / 2a ≈ 0/2a
```
This is catastrophic cancellation!

**Solution - Rationalized Form**:
```
x₁ = (-b - sign(b)√(b² - 4ac)) / 2a    (Stable for this root)
x₂ = c / (a × x₁)                        (Use Vieta's formula)
```

### 📐 Other Examples of Loss of Significance

| Expression | Problem | Stable Alternative |
|------------|---------|-------------------|
| √(x+1) - √x for large x | Cancellation | 1/(√(x+1) + √x) |
| 1 - cos(x) for small x | cos(x) ≈ 1 | 2sin²(x/2) |
| eˣ - 1 for small x | eˣ ≈ 1 | Use Taylor: x + x²/2 + ... |
| ln(x+1) - ln(x) for large x | Nearly equal | ln(1 + 1/x) |

### 💡 Memory Trick
**"When numbers are close, subtraction hurts the most!"**

### 📝 Exam Tips
1. Watch for subtractions of nearly equal quantities
2. Look for algebraic or series alternatives
3. Check if the problem involves small or large parameters that would cause near-equality

---

# 📚 PART C: ROOT FINDING METHODS

---

## 7. BISECTION METHOD

### 🧠 Intuition First

Imagine you're playing a number guessing game. Someone thinks of a number between 1 and 100. Each time you guess, they say "higher" or "lower." The smartest strategy? Always guess the MIDDLE number! Each guess cuts your search range in half.

The Bisection method does exactly this for finding where a function crosses zero.

### 📐 The Algorithm

**Prerequisites:**
- Continuous function f(x) on [a, b]
- f(a) and f(b) have OPPOSITE signs (one positive, one negative)
- This guarantees at least one root in (a, b) by **Intermediate Value Theorem**

**Algorithm:**
```
Input: f, a, b, tolerance ε
Output: Approximate root r

1. While (b - a) > ε:
   a. Compute midpoint: c = (a + b) / 2
   b. If f(c) = 0, return c (exact root found!)
   c. If f(a) × f(c) < 0:  (root is in left half)
         b = c
   d. Else:                (root is in right half)
         a = c
2. Return (a + b) / 2
```

### 🎨 Visual Understanding

```
Initial: f(a) < 0        f(b) > 0
         ●───────────────●
         a       c       b
                 ↓
         Check f(c):
         
If f(c) > 0:     If f(c) < 0:
●───────●        ●───────●
a   c=new b      a=c     b
```

### 📊 Worked Example

Find the root of f(x) = x³ - x - 2 in [1, 2]

| Iteration | a | b | c = (a+b)/2 | f(a) | f(c) | f(b) | New interval |
|-----------|---|---|-------------|------|------|------|--------------|
| 1 | 1 | 2 | 1.5 | -2 | -0.125 | 4 | [1.5, 2] |
| 2 | 1.5 | 2 | 1.75 | -0.125 | 1.609 | 4 | [1.5, 1.75] |
| 3 | 1.5 | 1.75 | 1.625 | -0.125 | 0.666 | 1.609 | [1.5, 1.625] |
| 4 | 1.5 | 1.625 | 1.5625 | -0.125 | 0.252 | 0.666 | [1.5, 1.5625] |
| ... | ... | ... | ... | ... | ... | ... | ... |

Root ≈ 1.5214 (actual: 1.52138...)

### 📐 Convergence Analysis

**Error after n iterations:**
```
|eₙ| = |r - cₙ| ≤ (b - a) / 2ⁿ⁺¹
```

**Number of iterations needed for tolerance ε:**
```
n ≥ log₂((b - a) / ε) - 1
```

Or equivalently:
```
n ≥ [log(b-a) - log(ε)] / log(2)
```

**Convergence Rate**: LINEAR with rate 1/2
- Each iteration HALVES the error
- We gain approximately log₁₀(2) ≈ 0.301 decimal digits per iteration
- Need about 3.3 iterations per additional correct decimal digit

### ✅ Advantages
1. **Always converges** (if initial conditions met)
2. **Simple** to implement and understand
3. **Robust** - works for any continuous function
4. **Error bound known** in advance

### ❌ Disadvantages
1. **Slow** compared to other methods
2. **Requires bracketing interval** [a, b] with sign change
3. **Cannot find multiple roots** in one run
4. **Cannot find roots where f touches but doesn't cross zero**

### 💻 Implementation (Pseudocode)

```python
def bisection(f, a, b, tol=1e-6, max_iter=100):
    if f(a) * f(b) >= 0:
        raise ValueError("f(a) and f(b) must have opposite signs")
    
    for i in range(max_iter):
        c = (a + b) / 2
        
        if abs(f(c)) < tol or (b - a) / 2 < tol:
            return c
        
        if f(a) * f(c) < 0:
            b = c
        else:
            a = c
    
    return c
```

### 💡 Memory Trick
**"Bisection = Binary Search for Roots"**

### ⚠️ Edge Cases
1. **Multiple roots**: Only finds one root in the interval
2. **Even-multiplicity roots**: f(x) = x² touches zero but doesn't cross - Bisection FAILS!
3. **Discontinuities**: May converge to a discontinuity if it looks like a sign change

---

## 8. NEWTON-RAPHSON METHOD

### 🧠 Intuition First

Imagine you're on a hilly landscape trying to find sea level (y = 0). At your current position, you look at the slope of the ground and say "If I follow this slope as a straight line, where would I hit sea level?" You walk there, then repeat.

Newton's method uses the TANGENT LINE to predict where the root is.

### 📐 Derivation

At point (xₙ, f(xₙ)), the tangent line has:
- Slope: f'(xₙ)  
- Equation: y - f(xₙ) = f'(xₙ)(x - xₙ)

Setting y = 0 to find x-intercept:
```
0 - f(xₙ) = f'(xₙ)(x - xₙ)
-f(xₙ) = f'(xₙ)(x - xₙ)
x = xₙ - f(xₙ)/f'(xₙ)
```

**The Newton-Raphson Formula:**
```
xₙ₊₁ = xₙ - f(xₙ) / f'(xₙ)
```

### 🎨 Visual Understanding

```
      Tangent line at xₙ
          \
           \
    f(xₙ) ●─\
            \
             \
    ─────●────\●─────────── y = 0
        xₙ₊₁   xₙ
         ↑
    New approximation!
```

### 📊 Worked Example

Find √2 (i.e., solve x² - 2 = 0) starting with x₀ = 1

f(x) = x² - 2,  f'(x) = 2x

Formula: xₙ₊₁ = xₙ - (xₙ² - 2)/(2xₙ) = (xₙ + 2/xₙ)/2

| n | xₙ | f(xₙ) | f'(xₙ) | xₙ₊₁ |
|---|-----|-------|--------|------|
| 0 | 1 | -1 | 2 | 1.5 |
| 1 | 1.5 | 0.25 | 3 | 1.4167 |
| 2 | 1.4167 | 0.00694 | 2.833 | 1.41422 |
| 3 | 1.41422 | 0.00001 | 2.828 | 1.414213562 |

√2 = 1.41421356... - we got 9 correct digits in just 4 iterations!

### 📐 Convergence Analysis

**Taylor Series Analysis:**

Let r be the true root and eₙ = xₙ - r be the error.

Expanding f(r) = 0 around xₙ:
```
0 = f(xₙ) + f'(xₙ)(r - xₙ) + f''(ξ)(r - xₙ)²/2
0 = f(xₙ) - f'(xₙ)eₙ + f''(ξ)eₙ²/2
```

From Newton's formula: xₙ₊₁ = xₙ - f(xₙ)/f'(xₙ)
```
eₙ₊₁ = xₙ₊₁ - r = xₙ - f(xₙ)/f'(xₙ) - r = eₙ - f(xₙ)/f'(xₙ)
```

After algebraic manipulation:
```
eₙ₊₁ = [f''(ξ) / (2f'(xₙ))] × eₙ²
```

**This means:**
```
eₙ₊₁ ≈ C × eₙ²
```

where C = f''(r) / (2f'(r)) near the root.

**Convergence Rate**: QUADRATIC (order 2)
- The number of correct digits approximately DOUBLES each iteration!
- If you have 2 correct digits, next iteration gives ~4, then ~8, then ~16...

### ⚠️ Convergence Conditions

Newton's method converges if:
1. **f, f', f'' exist and are continuous** near the root
2. **f'(r) ≠ 0** (simple root)
3. **Starting point x₀ is "close enough"** to the root

**Sufficient Condition (Newton-Kantorovich):**
```
|f(x₀) × f''(x)| / |f'(x₀)|² < 1
```
for all x in the neighborhood of x₀.

### ❌ Failure Cases

1. **f'(xₙ) = 0**: Division by zero! Method crashes.
   - Example: f(x) = x³ at x = 0 has f'(0) = 0

2. **Cycles/Divergence**: Can oscillate or diverge
   - Example: f(x) = x³ - 2x + 2 with x₀ = 0 → x₁ = 1 → x₂ = 0 → ...

3. **Multiple roots**: Converges LINEARLY, not quadratically
   - For root of multiplicity m: xₙ₊₁ = xₙ - m·f(xₙ)/f'(xₙ) (modified Newton)

4. **Wrong basin**: May converge to a different root than intended

### 💻 Implementation

```python
def newton(f, df, x0, tol=1e-10, max_iter=100):
    x = x0
    for i in range(max_iter):
        fx = f(x)
        dfx = df(x)
        
        if abs(dfx) < 1e-14:  # Avoid division by zero
            raise ValueError("Derivative too small")
        
        x_new = x - fx / dfx
        
        if abs(x_new - x) < tol:
            return x_new
        
        x = x_new
    
    raise ValueError("Did not converge")
```

### 📊 Computational Cost

Per iteration:
- 1 function evaluation: f(xₙ)
- 1 derivative evaluation: f'(xₙ)
- 1 division

**Total per iteration: 2 function evaluations** (counting derivative separately)

### 💡 Memory Tricks

1. **"Newton = Tangent Chasing"**
2. **Formula**: "x_new = x - f/f' " (fraction of function over slope)
3. **Convergence**: "Quadratic = Digits DOUBLE"

### 📝 Exam Tips

1. Always state the formula clearly
2. Show the derivative calculation
3. Track iterations in a table
4. Mention quadratic convergence
5. Note when it might fail (f' = 0, bad starting point)

---

## 9. SECANT METHOD

### 🧠 Intuition First

Newton's method is great, but it needs the derivative f'(x). What if computing the derivative is hard or expensive?

The Secant method says: "Approximate the derivative using two points!" Instead of the tangent (exact slope), use a secant line (approximate slope through two points).

### 📐 Derivation

Replace f'(xₙ) in Newton's formula with a finite difference approximation:
```
f'(xₙ) ≈ [f(xₙ) - f(xₙ₋₁)] / [xₙ - xₙ₋₁]
```

Substituting into Newton's formula:
```
xₙ₊₁ = xₙ - f(xₙ) / {[f(xₙ) - f(xₙ₋₁)] / [xₙ - xₙ₋₁]}
     = xₙ - f(xₙ)(xₙ - xₙ₋₁) / [f(xₙ) - f(xₙ₋₁)]
```

**The Secant Formula:**
```
xₙ₊₁ = xₙ - f(xₙ) × (xₙ - xₙ₋₁) / [f(xₙ) - f(xₙ₋₁)]
```

Or equivalently:
```
xₙ₊₁ = [xₙ₋₁f(xₙ) - xₙf(xₙ₋₁)] / [f(xₙ) - f(xₙ₋₁)]
```

### 🎨 Visual Understanding

```
                Secant line through (xₙ₋₁, f(xₙ₋₁)) and (xₙ, f(xₙ))
                            \
    f(xₙ₋₁) ●────────────────\
                              \
    f(xₙ)           ●──────────\
                                \
    ─────────●────────●─────────\●───── y = 0
            xₙ₊₁     xₙ        xₙ₋₁
```

### 📊 Worked Example

Find root of f(x) = x² - 2 with x₀ = 1, x₁ = 2

| n | xₙ₋₁ | xₙ | f(xₙ₋₁) | f(xₙ) | xₙ₊₁ |
|---|------|-----|---------|-------|------|
| 1 | 1 | 2 | -1 | 2 | 1.333 |
| 2 | 2 | 1.333 | 2 | -0.222 | 1.400 |
| 3 | 1.333 | 1.400 | -0.222 | -0.040 | 1.4146 |
| 4 | 1.400 | 1.4146 | -0.040 | 0.00119 | 1.41421 |

Root ≈ 1.41421 (√2 = 1.41421356...)

### 📐 Convergence Analysis

**Convergence Order**: φ = (1 + √5)/2 ≈ 1.618 (the Golden Ratio!)

```
eₙ₊₁ ≈ C × eₙ × eₙ₋₁
```

Which leads to:
```
eₙ₊₁ ≈ C' × |eₙ|^φ
```

**This means:**
- Faster than linear (Bisection)
- Slower than quadratic (Newton)
- "Superlinear" convergence

### 📊 Comparison Table

| Method | Order | Function Evals per Iter | Derivatives Needed |
|--------|-------|------------------------|-------------------|
| Bisection | 1 (linear) | 1 | No |
| Secant | 1.618 | 1 | No |
| Newton | 2 (quadratic) | 2 | Yes (f') |

**Efficiency Index** = p^(1/cost) where p is convergence order:
- Bisection: 1^1 = 1
- Secant: 1.618^1 = 1.618
- Newton: 2^(1/2) = 1.414

Secant actually has the HIGHEST efficiency per function evaluation!

### ⚠️ Conditions and Limitations

**Requires:**
1. Two initial guesses x₀ and x₁
2. f continuous
3. x₀ and x₁ "close enough" to the root

**Can fail when:**
1. f(xₙ) = f(xₙ₋₁) (horizontal secant - division by zero)
2. Poor initial guesses
3. Multiple or complex roots nearby

### 💻 Implementation

```python
def secant(f, x0, x1, tol=1e-10, max_iter=100):
    for i in range(max_iter):
        f0, f1 = f(x0), f(x1)
        
        if abs(f1 - f0) < 1e-14:
            raise ValueError("Division by near-zero")
        
        x2 = x1 - f1 * (x1 - x0) / (f1 - f0)
        
        if abs(x2 - x1) < tol:
            return x2
        
        x0, x1 = x1, x2
    
    raise ValueError("Did not converge")
```

### 💡 Memory Tricks

1. **"Secant = Newton without derivative"**
2. **"Two points make a secant; one point needs a tangent"**
3. **"Golden ratio convergence for Gold-standard efficiency"**

### 📝 Exam Tips

1. Remember you need TWO initial points
2. The formula looks complicated - derive it from Newton by approximating f'
3. Mention the golden ratio convergence order
4. Know that it's more efficient per function evaluation than Newton

---

# 📚 PART D: OPTIMIZATION METHODS

---

## 10. FIBONACCI SEARCH (1-D Minimization)

### 🧠 Intuition First

You're searching for the lowest point in a valley, but you can only "sample" the height at certain points. How do you narrow down the search efficiently?

Fibonacci search is like a sophisticated version of bisection for finding the MINIMUM of a function. It uses the Fibonacci sequence to decide where to place evaluation points optimally.

### 📐 The Setup

**Given:**
- Unimodal function f(x) on interval [a, b]
- **Unimodal** means: exactly ONE minimum, function decreases then increases

**Goal:**
- Find x* where f(x*) is minimum
- Reduce interval to size ≤ ε

### 📐 Fibonacci Sequence Review

```
F₁ = 1, F₂ = 1, F₃ = 2, F₄ = 3, F₅ = 5, F₆ = 8, F₇ = 13, ...
Fₙ = Fₙ₋₁ + Fₙ₋₂
```

### 📐 The Algorithm

**Step 1**: Determine n (number of iterations) such that:
```
Fₙ ≥ (b - a) / ε
```

**Step 2**: Initialize
```
x₁ = a + (Fₙ₋₂/Fₙ)(b - a)
x₂ = a + (Fₙ₋₁/Fₙ)(b - a)
```

**Step 3**: For k = 1, 2, ..., n-1:
```
If f(x₁) > f(x₂):
    a = x₁
    x₁ = x₂
    x₂ = a + (Fₙ₋ₖ₋₁/Fₙ₋ₖ)(b - a)
Else:
    b = x₂
    x₂ = x₁
    x₁ = a + (Fₙ₋ₖ₋₂/Fₙ₋ₖ)(b - a)
```

### 🎨 Visual Understanding

```
f(x)
  │    
  │    ∩
  │   / \
  │  /   \___
  │ /        \
  │/          \
  └───●───●────●── x
      a  x₁ x₂  b
      
Compare f(x₁) and f(x₂):
- If f(x₁) > f(x₂): minimum is in [x₁, b]
- If f(x₁) < f(x₂): minimum is in [a, x₂]
```

### 📐 Why Fibonacci Numbers?

The ratio Fₙ₋₁/Fₙ approaches 1/φ ≈ 0.618 (golden ratio inverse).

Fibonacci placement ensures:
1. After each iteration, ONE point can be reused
2. Interval reduction is optimal for a fixed number of evaluations
3. After n iterations: final interval = (b-a)/Fₙ

### 📊 Reduction Ratio

After n function evaluations (n-1 iterations):
```
Final interval size = (b - a) / Fₙ
```

For large n, this approaches:
```
(b - a) × φ⁻ⁿ ≈ (b - a) × (0.618)ⁿ
```

### ✅ Advantages
1. Optimal for fixed number of function evaluations
2. No derivatives needed
3. Well-defined termination

### ❌ Disadvantages
1. Must know n (number of iterations) in advance
2. Only works for unimodal functions
3. More complex than Golden Section

---

## 11. GOLDEN SECTION SEARCH

### 🧠 Intuition First

Golden Section is like Fibonacci search but simpler. Instead of using Fibonacci ratios, it uses the golden ratio τ = (√5 - 1)/2 ≈ 0.618 consistently.

Think of it as the "limiting case" of Fibonacci search as n → ∞.

### 📐 The Golden Ratio

```
φ = (1 + √5)/2 ≈ 1.618 (Golden Ratio)
τ = 1/φ = (√5 - 1)/2 ≈ 0.618 (Inverse Golden Ratio)
```

**Key Property**: τ² = 1 - τ, or equivalently, τ² + τ = 1

### 📐 The Algorithm

**Initialize**:
```
x₁ = a + (1-τ)(b-a) = a + 0.382(b-a)
x₂ = a + τ(b-a) = a + 0.618(b-a)
Evaluate f(x₁) and f(x₂)
```

**Iterate** until |b - a| < ε:
```
If f(x₁) > f(x₂):
    a = x₁
    x₁ = x₂
    f(x₁) = f(x₂)  [reuse!]
    x₂ = a + τ(b-a)
    Evaluate f(x₂)
Else:
    b = x₂
    x₂ = x₁
    f(x₂) = f(x₁)  [reuse!]
    x₁ = a + (1-τ)(b-a)
    Evaluate f(x₁)
```

### 📊 Worked Example

Minimize f(x) = (x-2)² on [0, 5], τ = 0.618

| Iter | a | b | x₁ | x₂ | f(x₁) | f(x₂) | New interval |
|------|---|---|-----|-----|-------|-------|--------------|
| 0 | 0 | 5 | 1.91 | 3.09 | 0.008 | 1.188 | [0, 3.09] |
| 1 | 0 | 3.09 | 1.18 | 1.91 | 0.672 | 0.008 | [1.18, 3.09] |
| 2 | 1.18 | 3.09 | 1.91 | 2.36 | 0.008 | 0.130 | [1.18, 2.36] |
| ... | ... | ... | ... | ... | ... | ... | ... |

Converges to x* ≈ 2 ✓

### 📐 Convergence Rate

Each iteration reduces interval by factor τ ≈ 0.618:
```
Interval after n iterations = (b-a) × τⁿ
```

Number of iterations for tolerance ε:
```
n = log((b-a)/ε) / log(1/τ) ≈ 2.078 × log₁₀((b-a)/ε)
```

### 💻 Implementation

```python
def golden_section(f, a, b, tol=1e-6):
    tau = (math.sqrt(5) - 1) / 2  # ≈ 0.618
    
    x1 = a + (1 - tau) * (b - a)
    x2 = a + tau * (b - a)
    f1, f2 = f(x1), f(x2)
    
    while (b - a) > tol:
        if f1 > f2:
            a = x1
            x1, f1 = x2, f2
            x2 = a + tau * (b - a)
            f2 = f(x2)
        else:
            b = x2
            x2, f2 = x1, f1
            x1 = a + (1 - tau) * (b - a)
            f1 = f(x1)
    
    return (a + b) / 2
```

### 📊 Comparison: Fibonacci vs Golden Section

| Aspect | Fibonacci | Golden Section |
|--------|-----------|----------------|
| Ratio used | Fₙ₋₁/Fₙ (varies) | τ ≈ 0.618 (constant) |
| Optimality | Optimal for fixed n | Slightly suboptimal |
| Implementation | Harder (need Fₙ) | Simpler |
| Number of evals | Fixed (n) | Variable (until tolerance) |

---

## 12. NEWTON'S METHOD FOR OPTIMIZATION

### 🧠 Intuition First

At a minimum, the derivative (slope) is zero: f'(x*) = 0. So finding a minimum is like finding the ROOT of f'(x)!

Newton's method for optimization applies Newton's root-finding method to f'(x).

### 📐 Derivation

Apply Newton's root-finding to g(x) = f'(x):
```
xₙ₊₁ = xₙ - g(xₙ)/g'(xₙ) = xₙ - f'(xₙ)/f''(xₙ)
```

**Newton's Optimization Formula:**
```
xₙ₊₁ = xₙ - f'(xₙ) / f''(xₙ)
```

### 📐 Geometric Interpretation

We're approximating f(x) by a quadratic (Taylor expansion to 2nd order):
```
f(x) ≈ f(xₙ) + f'(xₙ)(x-xₙ) + ½f''(xₙ)(x-xₙ)²
```

The minimum of this quadratic is found by setting derivative to zero:
```
f'(xₙ) + f''(xₙ)(x-xₙ) = 0
x = xₙ - f'(xₙ)/f''(xₙ)
```

### 📊 Worked Example

Minimize f(x) = x⁴ - 4x³ + 2, starting at x₀ = 4

f'(x) = 4x³ - 12x², f''(x) = 12x² - 24x

| n | xₙ | f'(xₙ) | f''(xₙ) | xₙ₊₁ |
|---|-----|--------|---------|------|
| 0 | 4 | 64 | 96 | 3.333 |
| 1 | 3.333 | 16.3 | 53.3 | 3.027 |
| 2 | 3.027 | 1.98 | 37.2 | 2.974 |
| 3 | 2.974 | 0.047 | 34.5 | 2.9986 |

Minimum at x* ≈ 3

### ⚠️ Important Conditions

1. **f''(xₙ) ≠ 0** (avoid division by zero)
2. **f''(x*) > 0** (ensures minimum, not maximum or saddle)
3. Starting point close enough to minimum

### 📐 Convergence

**Quadratic convergence** (like Newton root-finding):
```
eₙ₊₁ ≈ C × eₙ²
```

Much faster than Golden Section, but needs derivatives!

### 📊 Comparison of 1D Optimization Methods

| Method | Derivatives | Convergence | Robust |
|--------|-------------|-------------|--------|
| Fibonacci | None | Linear (≈0.618) | Yes |
| Golden Section | None | Linear (≈0.618) | Yes |
| Newton's | f', f'' | Quadratic | No (may diverge) |

---

## 13. STEEPEST DESCENT (Gradient Descent)

### 🧠 Intuition First

Imagine you're on a mountain in thick fog and want to reach the valley. You can't see far, but you CAN feel which way is downhill at your feet. The steepest descent strategy: always walk in the direction that goes downhill the fastest!

### 📐 Multivariate Setup

**Function**: f(x) where x = (x₁, x₂, ..., xₙ) is a vector

**Gradient**: ∇f = (∂f/∂x₁, ∂f/∂x₂, ..., ∂f/∂xₙ)

**Key Fact**: The gradient ∇f points in the direction of STEEPEST INCREASE. So -∇f points toward STEEPEST DECREASE.

### 📐 The Algorithm

```
Input: Starting point x₀, tolerance ε

Repeat:
    1. Compute gradient: g = ∇f(xₖ)
    2. If ||g|| < ε, stop (at minimum)
    3. Direction: d = -g (negative gradient)
    4. Line search: Find α that minimizes f(xₖ + αd)
    5. Update: xₖ₊₁ = xₖ + αd
```

### 📐 Line Search Methods

**Option 1: Exact Line Search**
Solve: α* = argmin f(xₖ - α∇f(xₖ))

**Option 2: Backtracking (Armijo) Line Search**
Start with α = 1, and while f(xₖ + αd) > f(xₖ) + c·α·∇f(xₖ)ᵀd:
    α = ρ·α (shrink by factor ρ ≈ 0.5)

### 📊 Worked Example

Minimize f(x,y) = x² + 4y² starting from (4, 4)

∇f = (2x, 8y)

| k | (x,y) | ∇f | Direction | α* | New (x,y) |
|---|-------|-----|-----------|-----|-----------|
| 0 | (4,4) | (8,32) | (-8,-32) | 0.147 | (2.82,-0.70) |
| 1 | (2.82,-0.70) | (5.65,-5.65) | (-5.65,5.65) | 0.147 | (2.0,0.12) |
| ... | ... | ... | ... | ... | ... |

Converges to (0, 0) ✓

### 📐 Convergence Analysis

For a quadratic function f(x) = ½xᵀAx - bᵀx where A is positive definite:

**Convergence rate**:
```
||xₖ₊₁ - x*|| ≤ [(κ-1)/(κ+1)]² × ||xₖ - x*||
```

Where κ = λₘₐₓ/λₘᵢₙ is the **condition number** of A.

**Problem**: When κ is large (ill-conditioned), convergence is SLOW!

### 🎨 Zigzag Behavior

```
     ╲    ╲    ╲
      ╲    ╲    ╲
   ↘   ╲    ╲    ●x₀
        ↘   ╲
         ↘  ●x₁
      x₃●    ↘
             ●x₂
          ●x*
```

The path "zig-zags" especially when contours are elongated ellipses!

### ✅ Advantages
1. Simple to implement
2. Works in any dimension
3. Always moves toward lower function values

### ❌ Disadvantages
1. Slow for ill-conditioned problems
2. Zig-zag behavior
3. Only finds local minima
4. Sensitive to step size

---

## 14. NELDER-MEAD (SIMPLEX) ALGORITHM

### 🧠 Intuition First

Imagine a bouncy, stretchy triangle (in 2D) or a shape with n+1 vertices in n dimensions. This shape "rolls downhill" toward the minimum, stretching, shrinking, and flipping as needed to fit into valleys.

It's a "derivative-free" method - you only need function values!

### 📐 Key Concepts

**Simplex**: A shape with n+1 vertices in n dimensions
- 1D: Line segment (2 points)
- 2D: Triangle (3 points)
- 3D: Tetrahedron (4 points)

### 📐 The Operations

Starting with vertices x₁, x₂, ..., xₙ₊₁ sorted by function value:
- **Best**: xᵦ = x₁ (lowest f value)
- **Worst**: xᵥ = xₙ₊₁ (highest f value)
- **Second worst**: xₛ = xₙ
- **Centroid** (excluding worst): x̄ = (x₁ + x₂ + ... + xₙ)/n

**Four Operations:**

1. **Reflection**: xᵣ = x̄ + α(x̄ - xᵥ), typically α = 1
   - "Flip" the worst point through the centroid

2. **Expansion**: xₑ = x̄ + γ(xᵣ - x̄), typically γ = 2
   - If reflection is good, try going further!

3. **Contraction**: xc = x̄ + ρ(xᵥ - x̄), typically ρ = 0.5
   - If reflection is bad, shrink toward centroid

4. **Shrink**: xᵢ = xᵦ + σ(xᵢ - xᵦ), typically σ = 0.5
   - Shrink entire simplex toward best point

### 📐 The Algorithm

```
1. Initialize simplex with n+1 points
2. Sort vertices: f(x₁) ≤ f(x₂) ≤ ... ≤ f(xₙ₊₁)
3. Compute centroid x̄ (excluding xₙ₊₁)
4. REFLECT: xᵣ = x̄ + α(x̄ - xₙ₊₁)
   
   If f(x₁) ≤ f(xᵣ) < f(xₙ):
       Replace xₙ₊₁ with xᵣ
   
   Else if f(xᵣ) < f(x₁):  [Reflection is best so far!]
       EXPAND: xₑ = x̄ + γ(xᵣ - x̄)
       If f(xₑ) < f(xᵣ): Replace xₙ₊₁ with xₑ
       Else: Replace xₙ₊₁ with xᵣ
   
   Else:  [f(xᵣ) ≥ f(xₙ)]
       CONTRACT: xc = x̄ + ρ(xₙ₊₁ - x̄)
       If f(xc) < f(xₙ₊₁): Replace xₙ₊₁ with xc
       Else: SHRINK all points toward x₁

5. Repeat until convergence
```

### 🎨 Visual (2D Case)

```
   Operations on triangle:
   
   REFLECT         EXPAND          CONTRACT        SHRINK
      ●xᵣ            ●xₑ
     /               /                               ●x₁
    /               /                  ●xᵥ          / \
   ●───●           ●───●           ●───●──●xc      ●───●
   │   │           │   │           │   │           (smaller)
   ●xᵥ             ●xᵥ             ●               
```

### 📊 Convergence Properties

- **No formal convergence guarantee** for general functions
- Works well in practice for smooth functions
- Can stall on non-smooth or noisy functions
- Typically converges to local minimum

### ✅ Advantages
1. **No derivatives needed** - derivative-free!
2. Simple to implement
3. Works well for n ≤ 10 dimensions
4. Robust to noise

### ❌ Disadvantages
1. Slow for high dimensions (n > 10)
2. No convergence guarantee
3. May converge to non-stationary points
4. Sensitive to initial simplex

### 💻 Simple Implementation Sketch

```python
def nelder_mead(f, x0, tol=1e-6, max_iter=1000):
    n = len(x0)
    # Initialize simplex
    simplex = [x0]
    for i in range(n):
        point = x0.copy()
        point[i] += 0.5  # Perturbation
        simplex.append(point)
    
    alpha, gamma, rho, sigma = 1, 2, 0.5, 0.5
    
    for _ in range(max_iter):
        # Sort by function values
        simplex.sort(key=f)
        
        # Check convergence
        if max([f(x) - f(simplex[0]) for x in simplex]) < tol:
            return simplex[0]
        
        # Centroid (excluding worst)
        centroid = sum(simplex[:-1]) / n
        
        # Reflection
        xr = centroid + alpha * (centroid - simplex[-1])
        
        # Apply appropriate operation based on f(xr)
        # ... (expansion, contraction, shrink logic)
    
    return simplex[0]
```

### 💡 Memory Trick

**"RECS" - Reflect, Expand, Contract, Shrink**

Think of a jellyfish pulsing through water, changing shape to navigate!

---

# 📊 UNIT 1 SUMMARY TABLE

| Method | Type | Order | Derivatives | Robust | Best For |
|--------|------|-------|-------------|--------|----------|
| Bisection | Root | 1 | None | Yes | Safe, guaranteed |
| Newton | Root | 2 | f' | No | Fast, smooth functions |
| Secant | Root | 1.618 | None | Moderate | Fast, no derivative |
| Fibonacci | Min | Linear | None | Yes | Fixed evaluation budget |
| Golden | Min | Linear | None | Yes | Simple 1D minimization |
| Newton-Opt | Min | 2 | f', f'' | No | Smooth 1D optimization |
| Steepest | Min | Linear | ∇f | Yes | Simple multi-D |
| Nelder-Mead | Min | N/A | None | Moderate | Black-box optimization |

---

# 🎯 EXAM PREPARATION CHECKLIST - UNIT 1

□ Can derive Taylor series for eˣ, sin(x), cos(x)?
□ Know Taylor remainder formula?
□ Can state Rolle's and MVT with conditions?
□ Understand absolute vs relative error?
□ Know IEEE floating-point structure?
□ Can identify catastrophic cancellation?
□ Can implement Bisection step-by-step?
□ Know Newton formula and derive it?
□ Understand when Newton fails?
□ Know Secant formula and golden ratio convergence?
□ Can explain Golden Section search?
□ Understand gradient descent direction?
□ Know Nelder-Mead operations?
