### Binomial Coefficient
$$
\binom{n}{k} = \frac{n!}{k!(n-k)!}
$$

- Properties:  
	- Symmetry: `C(n, k) = C(n, n-k)`  
	- Pascal’s rule: `C(n, k) = C(n-1, k-1) + C(n-1, k)`

```cpp
// O(k)
long long binomialCoefficient(int n, int k) {
    if (k > n) return 0;
    if (k > n - k) k = n - k; // use symmetry C(n,k)=C(n,n-k) for less iterations
    long long res = 1;
    for (int i = 1; i <= k; ++i) {
        res = res * (n - k + i) / i;
    }
    return res;
}

```

---
### Permutations
$$
  P(n, k) = \frac{n!}{(n-k)!}
$$
- (Ordered) In how many ways we can pick `k` items from a pool `n`: 
- `{A, B, C}` is different than `{A, C, B}`
- Example: Locker combination
- Special case `n == k`: $n!$

---
### Combinations
$$
C(n, k) = \binom{n}{k} = \frac{n!}{k!(n-k)!}
$$
- (Unordered) In how many ways we can pick `k` items from a pool `n`: 
- `{A, B, C}` is the same as `{A, C, B}`
- Examples:
	- Choose `k` pizza toppings out of `n` → `C(n, k)`
	- Ways to pick 4 cards from a deck of 52 → `C(52, 4)`
---
### Combinations with repetitions
$$
C_r(n, k) = \binom{n+k-1}{k}
$$
### Catalan Numbers
  $$
  C_n = \frac{1}{n+1} \binom{2n}{n}
  $$
- Hints: “balanced”, “non-crossing”, “valid parentheses”
- Examples:
	- Given `n` pairs of parentheses, how many valid expressions can you form? → `Catalan(n)`
	- How many different binary trees can be formed with `n` nodes?  → `Catalan(n)`
	- In how many ways can `n` people shake hands such that no two handshakes cross? → `Catalan(n/2)`

---
