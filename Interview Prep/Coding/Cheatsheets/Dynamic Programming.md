## 1. Linear 1D DP (Climbing / Robber / Decode)

- **Climbing Stairs (min cost)**  
  - State: `dp[i] = min_cost to reach step i`  
  - Transition: `min(dp[i-1], dp[i-2]) + cost[i]` (cost)  
  - Complexity: **O(n) time, O(1) space**

- **House Robber**  
  - State: `dp[i] = max loot up to house i`  
  - Transition: `dp[i] = max(dp[i-1], dp[i-2] + val[i])`  
  - Complexity: **O(n) time, O(1) space**

- **Decode Ways**  
  - State: `dp[i] = #ways to decode prefix of length i`  
  - Transition:  
    - If `s[i-1] != '0'`: `dp[i] += dp[i-1]`  
    - If `s[i-2..i-1]` is 10–26: `dp[i] += dp[i-2]`  
  - Complexity: **O(n) time, O(1) space**

```cpp
int houseRobber(vector<int>& a) {
    long long prev2 = 0, prev1 = 0; // dp[i-2], dp[i-1]
    for (int x : a) {
        long long cur = max(prev1, prev2 + x);
        prev2 = prev1; prev1 = cur;
    }
    return (int)prev1;
}
```

---

## 2. Knapsack-style DP

- **0/1 Knapsack**  
  - State: `dp[w] = max value with capacity w`  
  - Transition: `dp[w] = max(dp[w], dp[w-weight[i]] + val[i])` (loop `w` descending)  
  - Complexity: **O(n·W) time, O(W) space**

- **Partition Equal Subset Sum**  
  - State: `dp[s] = true if subset sum s possible`  
  - Transition: `dp[s] |= dp[s-num]` (loop `s` descending)  
  - Complexity: **O(n·S) time, O(S) space** where `S = target sum`

- **Coin Change (min coins)**  
  - State: `dp[amt] = min coins to make amt`  
  - Transition: `dp[amt] = min(dp[amt], dp[amt-coin] + 1)`  
  - Complexity: **O(n·amount) time, O(amount) space**

- **Coin Change 2 (count ways)**  
  - State: `dp[amt] = #ways to make amt`  
  - Transition: `dp[amt] += dp[amt-coin]` (loop `coin` outer, `amt` inner ascending)  
  - Complexity: **O(n·amount) time, O(amount) space**

```cpp
// min coins
int coinChange(vector<int>& coins, int amount) {
    const int INF = 1e9;
    vector<int> dp(amount+1, INF); dp[0] = 0;
    for (int c : coins)
        for (int s = c; s <= amount; ++s)
            dp[s] = min(dp[s], dp[s-c] + 1);
    return dp[amount] >= INF ? -1 : dp[amount];
}

// count ways
long long change(int amount, vector<int>& coins) {
    vector<long long> dp(amount+1, 0); dp[0] = 1;
    for (int c : coins)
        for (int s = c; s <= amount; ++s)
            dp[s] += dp[s - c];
    return dp[amount];
}
```

---

## 3. Sequence Alignment

- **LCS (Longest Common Subsequence)**  
  - State: `dp[i][j] = LCS length of a[0..i-1], b[0..j-1]`  
  - Transition:  
    - If `a[i-1] == b[j-1]`: `dp[i][j] = dp[i-1][j-1] + 1`  
    - Else: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`  
  - Complexity: **O(n·m) time, O(min(n,m)) space**

- **Edit Distance**  
  - State: `dp[i][j] = min ops to convert a[0..i-1] → b[0..j-1]`  
  - Transition:  
    - If `a[i-1] == b[j-1]`: `dp[i][j] = dp[i-1][j-1]`  
    - Else: `1 + min(insert=dp[i][j-1], delete=dp[i-1][j], replace=dp[i-1][j-1])`  
  - Complexity: **O(n·m) time, O(min(n,m)) space**

- **Longest Palindromic Subsequence**  
  - State: `dp[l][r] = LPS length in s[l..r]`  
  - Transition:  
    - If `s[l] == s[r]`: `dp[l][r] = 2 + dp[l+1][r-1]`  
    - Else: `dp[l][r] = max(dp[l+1][r], dp[l][r-1])`  
  - Complexity: **O(n²) time, O(n²) space**

```cpp
int editDistance(string a, string b) {
    int n=a.size(), m=b.size();
    vector<int> prev(m+1), cur(m+1);
    iota(prev.begin(), prev.end(), 0);
    for (int i=1;i<=n;++i){
        cur[0]=i;
        for (int j=1;j<=m;++j){
            if(a[i-1]==b[j-1]) cur[j]=prev[j-1];
            else cur[j]=1+min({prev[j], cur[j-1], prev[j-1]});
        }
        swap(prev,cur);
    }
    return prev[m];
}
```

---

## \[Rare\] 4. Subarray / Subsequence Optimization

- **LIS (O(n²))**  
  - State: `dp[i] = LIS ending at i`  
  - Transition: `dp[i] = max(dp[j] + 1) for j < i, arr[j] < arr[i]`  
  - Complexity: **O(n²) time, O(n) space**

- **LIS (O(n log n))**  
  - Use patience sorting (`lower_bound`)  
  - Complexity: **O(n log n) time, O(n) space**

- **Maximum Subarray (Kadane’s)**  
  - State: `dp[i] = max subarray ending at i`  
  - Transition: `dp[i] = max(arr[i], dp[i-1] + arr[i])`  
  - Complexity: **O(n) time, O(1) space**

---

## \[Rare\] 5. Interval DP

- **Matrix Chain / Burst Balloons**  
  - State: `dp[l][r] = best value in interval [l..r]`  
  - Transition: `dp[l][r] = max/min over k in [l..r] of (dp[l][k-1] + cost(l,k,r) + dp[k+1][r])`  
  - Complexity: **O(n³) time, O(n²) space**

---

## \[Unlikely\] 6. Bitmask / State Compression

- **TSP**  
  - State: `dp[mask][u] = min cost to visit set=mask ending at u`  
  - Transition: `dp[mask|1<<v][v] = min(dp[mask|1<<v][v], dp[mask][u] + dist[u][v])`  
  - Complexity: **O(n²·2ⁿ) time, O(n·2ⁿ) space**

---

## \[Unlikely\] 7. Tree DP

- **Maximum Independent Set**  
  - State: `(take[u], skip[u])` = best if we take node u / skip u  
  - Transition:  
    - If take u → add skip[v] for children  
    - If skip u → add max(take[v], skip[v]) for children  
  - Complexity: **O(n) time, O(h) space** (DFS stack, h=tree height)

---

## \[Unlikely\] 8. Digit DP (stretch)

- State: `dp[pos][tight][sum/other property]`  
- Transition: iterate over digit choices (0–limit), update property.  
- Complexity: **O(digits · states_per_property) time**

---

