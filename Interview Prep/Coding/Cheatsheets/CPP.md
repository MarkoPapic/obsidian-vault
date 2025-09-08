
## Useful Headers
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <unordered_map>
#include <unordered_set>
#include <map>
#include <set>
#include <queue>
#include <stack>
#include <algorithm>
#include <climits>
#include <cmath>
```

## Minimal example
```cpp
#include <iostream>
#include <stack>

using namespace std;

class Solution {
private:
	bool isClosing(char c) {
		return c == ')' || c == '}' || c == ']';
	}
public:
    bool doSomething(string s) {
        bool closing = isClosing(s[0]);
		return true;
    }
};

int main() {
	Solution s;
	bool result = s.doSomething("hello");
	cout << "Result: " << result << endl;

	return 0;
}
```

## Numerical types

| Type               | Size     |
| ------------------ | -------- |
| short int          | 2B       |
| int                | 4B       |
| unsigned int       | 4B       |
| long               | 4B or 8B |
| unsigned long      | 4B or 8B |
| long long          | 8B       |
| unsigned long long | 8B       |
| float              | 4B       |
| double             | 8B       |
| long double        | 16B      |

## Data Structures
### Static Arrays
```cpp
int arr[100];              // default-initialized
int arr2[3] = {1, 2, 3};   // fixed size

memset(arr, 0, sizeof(arr));   // set all to 0
```
### Strings
```cpp
string s = "meta";
s.size(); s.empty(); s.clear();
s[0]; s.substr(1, 2); s.find("e");

s += "verse";             // concatenate
to_string(123);          // int to string
stoi("123");             // string to int
```
### Vector
```cpp
vector<int> v = {1, 2, 3};
v.push_back(4);
int x = v[1];          // access element
v.size(); v.empty(); v.clear();
v.front(); v.back();
sort(v.begin(), v.end());
reverse(v.begin(), v.end());

// 2D vector
vector<vector<int>> dp(n, vector<int>(m, -1));
```
### Stack
```cpp
stack<int> s;
s.push(1); s.top(); s.pop(); s.empty();
```
### Queue
```cpp
queue<int> q;
q.push(1); q.front(); q.pop(); q.empty();
```
### Dequeue
```cpp
deque<int> dq;
dq.push_back(1); dq.push_front(2);
dq.pop_back(); dq.pop_front();
dq.front(); dq.back();
```
### Priority Queue
```cpp
priority_queue<int> maxHeap;
priority_queue<int, vector<int>, greater<int>> minHeap;
maxHeap.push(3); maxHeap.top(); maxHeap.pop(); maxHeap.empty();
```
### Set / Multiset
```cpp
set<int> s;
s.insert(5); s.contains(5); s.erase(5);
s.find(5); s.size(); s.empty();

multiset<int> ms;
ms.insert(5); ms.erase(ms.find(5));
```
### Map / Unordered Map
```cpp
map<string, int> m;
unordered_map<string, int> um;

m["key"] = 42;
int x = m["key"];
m.contains("key"); m.erase("key"); m.size(); m.clear();
```

## Loops
```cpp
for (int i = 0; i < n; ++i) { }
for (auto x : v) { }

for (auto it = m.begin(); it != m.end(); ++it) {
    cout << it->first << " " << it->second << endl;
}
```

## Algorithms and Utils

### Sorting
```cpp
sort(v.begin(), v.end());
sort(v.begin(), v.end(), greater<int>());

sort(v.begin(), v.end(), [](const auto& a, const auto& b) {
    return a.second < b.second;
});
```
### Binary Search
```cpp
int idx = lower_bound(v.begin(), v.end(), target) - v.begin();  // first >= target
```
### Misc
```cpp
INT_MAX, INT_MIN
abs(x), pow(a, b), sqrt(x)
```
### Custom Comparator Piriority Queue
```cpp
// Min heap of pairs based on second element
priority_queue<pair<int, int>, vector<pair<int, int>>, 
               function<bool(pair<int,int>, pair<int,int>)>> 
    pq([](auto& a, auto& b) {
        return a.second > b.second;
    });
```
