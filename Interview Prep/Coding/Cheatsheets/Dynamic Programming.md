## 1. Linear 1D DP (Climbing / Robber / Decode)
**Core clue:** _You’re moving along a line (array/string/steps), and each state depends only on the last 1-2 steps._
**Classic phrasing**: _At each step, you can do X or Y; compute min/max/ways to reach the end._

- **Climbing Stairs (min cost)**  
  - State: `dp[i] = min_cost to reach step i`  
  - Transition: `min(dp[i-1], dp[i-2]) + cost[i]`
  - Complexity: **O(n) time, O(1) space**

- **House Robber**  
  - State: `dp[i] = max loot up to house i`  
  - Transition: `dp[i] = max(dp[i-1], dp[i-2] + val[i])`  
  - Complexity: **O(n) time, O(1) space**

- **Decode Ways**  
  - State: `dp[i] = #ways to decode prefix of length i`  
  - Transitions:  
    - If `s[i-1] != '0'`: `dp[i] += dp[i-1]`  
    - If `s[i-2..i-1]` is 10–26: `dp[i] += dp[i-2]`  
  - Complexity: **O(n) time, O(1) space**

```cpp
int minCostClimbingStairs(vector<int>& cost) {
    int n = (int)cost.size();
    int prev2 = 0, prev1 = 0;                 // dp[0], dp[1]
    for (int i = 2; i <= n; ++i) {            // build up to "top" = n
        int take1 = prev1 + cost[i - 1];      // come from i-1, pay cost[i-1]
        int take2 = prev2 + cost[i - 2];      // come from i-2, pay cost[i-2]
        int cur = min(take1, take2);
        prev2 = prev1; prev1 = cur;
    }
    return prev1;
}

int houseRobber(vector<int>& nums) {
    int n = nums.size();
    if (n == 0) return 0;
    if (n == 1) return nums[0];

    int prev2 = nums[0];                  // dp[i-2]
    int prev1 = max(nums[0], nums[1]);    // dp[i-1]
    for (int i = 2; i < n; ++i) {
        int cur = max(prev1, prev2 + nums[i]);
        prev2 = prev1;
        prev1 = cur;
    }
    return prev1;
}

int numDecodings(string s) {
    // ...

    for (int i = 2; i <= n; i++) {
        int cur = 0;

        // Single digit: s[i-1]
        if (s[i - 1] != '0') {
            cur = prev1;
        }

        // Two digits: s[i-2..i-1]
        int tens = (s[i - 2] - '0') * 10 + (s[i - 1] - '0');
        if (tens >= 10 && tens <= 26) {
            cur += prev2;
        }

        prev2 = prev1;
        prev1 = cur;
    }
    return prev1;
}
```

---

## 2. Knapsack-style DP
**Core clue:** _You’re making choices under a “capacity” or “sum” constraint._
**Signature recurrence**: $dp[w]=max/min/count(dp[w], dp[w−weight]+value)$
**Implementation**:
* Memo: array of length `goal + 1`
	* Outcomes for `[0..goal]`
	* Starting point \[0\] is usually set ahead of time
- Loop:
	- Outer: items we are considering
	- Inner: sub-goal capacity we are solving for
	- Direction:
		- 0/1: descending
		- unbounded (repetitions allowed): ascending

- **0/1 Knapsack**  
  - State: `dp[w] = max value with capacity w considering items [0..i]`
  - Transition: `dp[w] = max(dp[w], dp[w-weight[i]] + val[i])` (loop `w` descending) 
	  - If we skip item `i`, the value for capacity `w` stays the same
	  - If we use item `i`, the capacity drops by `weight[i]` and value increases by `val[i]`
  - Complexity: **O(n·W) time, O(W) space**

- **Partition Equal Subset Sum**  
  - State: `dp[s] = true if subset sum s possible with items [0..i]`  
  - Transition: `dp[s] |= dp[s-num]` (loop `s` descending) 
	  - If we could form sum `s` without `num`, then we can still form it without `num`
	  - If we could form sum `s-num`  without `num`, then we can form `s` with `num`
	  - Else: we cannot form `s` with the current subset
  - Complexity: **O(n·S) time, O(S) space** where `S = target sum`

- **Coin Change (min coins)**  
  - State: `dp[amt] = min number of coins to make amt considering coins [0..i]`  
  - Transition: `dp[amt] = min(dp[amt], dp[amt-coin] + 1)`
	  - If we skip coin `i`, the  number of coins stays the same
	  - If we use coin `i`,  new number of coins := num_coins_without_i + 1
  - Complexity: **O(n·amount) time, O(amount) space**

- **Coin Change 2 (count ways)**  
  - State: `dp[amt] = #ways to make amt` 
  - Transition: `dp[amt] += dp[amt-coin]` (loop `coin` outer, `amt` inner ascending)  
  - Complexity: **O(n·amount) time, O(amount) space**

```cpp
bool canPartition(vector<int>& nums) {
	int total = accumulate(nums.begin(), nums.end(), 0);
	if (total % 2 != 0)
	    return false; // odd total can't be split evenly
	int target = total / 2;
	
	vector<bool> dp(target + 1, false);
	dp[0] = true; // sum 0 always possible
	
	for (int num : nums) {
	    for (int s = target; s >= num; --s) {
	        dp[s] = dp[s] || dp[s - num];
	    }
	}
	return dp[target];
}

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
**Core clue:** _You’re comparing or transforming two sequences (strings/arrays)._

- **LCS (Longest Common Subsequence)**  
  - State: `dp[i][j] = LCS length of a[0..i-1], b[0..j-1]`  
  - Transition:  
    - If `a[i-1] == b[j-1]`: `dp[i][j] = dp[i-1][j-1] + 1`  
    - Else: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`  
  - Complexity: **O(n·m) time, O(mn) space**

- **Min Edit Distance**  
  - State: `dp[i][j] = min edits to convert a[0..i-1] → b[0..j-1]`  
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
int longestCommonSubsequence(string text1, string text2) {
    int m = text1.size(), n = text2.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1, 0));

    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (text1[i-1] == text2[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }
    return dp[m][n];
}

int minDistance(string word1, string word2) {
    int m = word1.size(), n = word2.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1, 0));

    // Base cases
    for (int i = 0; i <= m; ++i) dp[i][0] = i;
    for (int j = 0; j <= n; ++j) dp[0][j] = j;

    // Fill DP table
    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (word1[i-1] == word2[j-1]) {
                dp[i][j] = dp[i-1][j-1];
            } else {
                dp[i][j] = 1 + min({dp[i-1][j],    // delete
                                    dp[i][j-1],    // insert
                                    dp[i-1][j-1]});// replace
            }
        }
    }
    return dp[m][n];
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

