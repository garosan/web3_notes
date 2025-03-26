# Section 02. Swap

Great question! Let's break this explanation down **step by step** and rephrase it in a more beginner-friendly way.

---

### 🧠 The Goal:

We want to figure out **how many tokens you get out** of a swap when you **put in some tokens**.

For example:

- You add some token **X** into Uniswap.
- You want to know how many tokens **Y** you'll receive in return.

---

### 🧪 The Core Equation: `X * Y = L²`

This is the **invariant** of the AMM (Automated Market Maker). It means that the product of token X and token Y **must always stay the same** (as long as no one adds or removes liquidity).

Let’s define the variables:

- `X0` = amount of token X in the pool **before** the swap
- `Y0` = amount of token Y in the pool **before** the swap
- `dx` = amount of token X that Alice **adds**
- `dy` = amount of token Y that Alice **receives**
- After the swap:
  - New X amount = `X0 + dx`
  - New Y amount = `Y0 - dy`

We **lose** `dy` tokens from the pool (because they’re sent to Alice), and **gain** `dx` tokens (because Alice adds them).

---

### ✍️ Step-by-step Derivation:

Before the swap:

```
X0 * Y0 = L²
```

After the swap:

```
(X0 + dx) * (Y0 - dy) = L²
```

Since both expressions are equal to `L²`, we can set them equal to each other:

```
X0 * Y0 = (X0 + dx)(Y0 - dy)
```

Now, solve this for `dy`.

---

### 🔍 Solving for dy:

Start with:

```
X0 * Y0 = (X0 + dx)(Y0 - dy)
```

Expand the right side:

```
X0 * Y0 = (X0 + dx) * Y0 - (X0 + dx) * dy
```

Move terms around:

```
(X0 + dx) * dy = (X0 + dx) * Y0 - X0 * Y0
```

Now divide both sides by `(X0 + dx)`:

```
dy = [(X0 + dx) * Y0 - X0 * Y0] / (X0 + dx)
```

Distribute the top:

```
dy = [X0 * Y0 + dx * Y0 - X0 * Y0] / (X0 + dx)
```

Now cancel out `X0 * Y0`:

```
dy = dx * Y0 / (X0 + dx)
```

---

### ✅ Final Result:

> **dy = (Y0 \* dx) / (X0 + dx)**

This tells us:  
Given:

- X0 = current token X in the pool
- Y0 = current token Y in the pool
- dx = amount of token X you're swapping in

You can calculate:

- dy = amount of token Y you'll get **out**

---

### ⚠️ One Last Note:

This formula assumes **no fees**. In reality, Uniswap charges a small fee (like 0.3%), so you'll get **slightly less** than this amount. They’ll cover that in the next part of the course.

## 🛠️ Links and Resources
