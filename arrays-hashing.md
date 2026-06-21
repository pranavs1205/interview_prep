# Arrays + Hashing — Interview Prep Book
### Python & C++ | Striver A2Z Step 3 + Hashing | 2026 Edition

> **Sources:** Striver A2Z, TechInterviewHandbook, NeetCode, AlgoMaster, GFG, FutureJobs, interviewing.io, LeetCode discussions  
> **Depth:** Interview (Deep) | **Languages:** Python + C++  
> **Split:** ~20% Theory | ~80% Problems + Solutions across all 6 core patterns  
> **Generated:** June 2026

---

## Table of Contents

1. [Quick Reference Card](#1-quick-reference-card)
2. [Arrays Theory](#2-arrays-theory)
3. [Hashing Theory](#3-hashing-theory)
4. [The 6 Core Array Patterns](#4-the-6-core-array-patterns)
   - Pattern 1: Brute Force → Optimization Mindset
   - Pattern 2: Two Pointers
   - Pattern 3: Sliding Window
   - Pattern 4: Prefix Sum
   - Pattern 5: HashMap / HashSet
   - Pattern 6: Kadane's Algorithm
5. [Problems — Easy (Must Solve First)](#5-problems--easy-must-solve-first)
6. [Problems — Medium (Core Interview Level)](#6-problems--medium-core-interview-level)
7. [Problems — Hard (Differentiators)](#7-problems--hard-differentiators)
8. [Interview Q&A Bank](#8-interview-qa-bank)
9. [Problem Master List with LeetCode Links](#9-problem-master-list-with-leetcode-links)
10. [Source Notes](#10-source-notes)
11. [Glossary](#11-glossary)

---

## 1. Quick Reference Card

**The 6 Patterns and their trigger signals:**

| Pattern | When You See... | Time | Space |
|---|---|---|---|
| Two Pointers | Sorted array, pair/triplet sum, palindrome, remove duplicates | O(n) | O(1) |
| Sliding Window | Contiguous subarray/substring with a constraint | O(n) | O(1)–O(k) |
| Prefix Sum | Range sum queries, subarray sum = k, equilibrium | O(1) query | O(n) |
| HashMap | Frequency count, pair existence, duplicate detection | O(n) avg | O(n) |
| Kadane's | Max/min subarray sum | O(n) | O(1) |
| Sort + Greedy | Intervals, meetings, pair matching | O(n log n) | O(1)–O(n) |

**HashMap Python vs C++ at a glance:**
```python
# Python
d = {}
d[key] = val
d.get(key, default)
key in d
del d[key]
from collections import defaultdict, Counter
```
```cpp
// C++
unordered_map<int,int> mp;
mp[key] = val;        // or mp[key]++
mp.count(key)         // 0 or 1
mp.find(key) != mp.end()
mp.erase(key)
// map<int,int> for sorted order — O(log n)
// unordered_map for O(1) avg
```

**Must-solve 15 problems (cover all patterns):**
Two Sum · Best Time to Buy/Sell Stock · Contains Duplicate · Maximum Subarray · Product of Array Except Self · Subarray Sum Equals K · Longest Consecutive Sequence · 3Sum · Container With Most Water · Trapping Rain Water · Majority Element · Rotate Array · Find the Duplicate · Maximum Product Subarray · Merge Intervals

---

## 2. Arrays Theory

An array is a contiguous block of memory where elements are stored at consecutive addresses. This contiguity is what gives arrays O(1) random access — `arr[i]` computes address `base + i × element_size` in constant time, regardless of array size.

**Python list vs C++ vector:**

| Property | Python `list` | C++ `vector<T>` |
|---|---|---|
| Dynamic resizing | Yes (amortized O(1) append) | Yes (amortized O(1) push_back) |
| Type | Mixed types allowed | Single type |
| Memory | References (8 bytes each) | Direct values |
| Access | O(1) | O(1) |
| Insert/delete at end | O(1) amortized | O(1) amortized |
| Insert/delete at middle | O(n) (shift) | O(n) (shift) |
| Search (unsorted) | O(n) | O(n) |
| Search (sorted) | O(log n) binary | O(log n) binary |

**The key mental model:** An array lets you trade O(n) setup for O(1) access. When a problem has repeated range queries or lookups, think "can I precompute something into an array to make each query O(1)?" That's the prefix sum and hashmap mindset.

**Important operations in Python for interviews:**
```python
arr = [1, 2, 3, 4, 5]

# Slicing — O(k) time and space
arr[1:4]          # [2, 3, 4]
arr[::-1]         # Reverse: [5, 4, 3, 2, 1]
arr[::2]          # Every other: [1, 3, 5]

# List comprehension — clean and fast
squares = [x*x for x in arr]
evens = [x for x in arr if x % 2 == 0]

# zip for parallel iteration
for a, b in zip(arr1, arr2):
    ...

# enumerate for index + value
for i, val in enumerate(arr):
    ...

# In-place reversal
arr.reverse()       # O(n), in-place
arr[:] = arr[::-1]  # Also in-place via slice assignment

# Frequency count in one line
from collections import Counter
freq = Counter(arr)     # {value: count, ...}
```

---

## 3. Hashing Theory

A hash map (also called hash table, dictionary) maps keys to values in O(1) average time using a hash function. The hash function converts a key to an integer index into an underlying array (the "bucket array"). When two keys hash to the same index (a "collision"), the structure uses either chaining (linked list at that bucket) or open addressing (probe next slot).

**Why O(1) average?** The hash function distributes keys roughly uniformly across buckets. With load factor (items/buckets) kept below a threshold (Python: 2/3, Java: 0.75), each bucket holds a constant expected number of items, so operations are O(1) on average.

**When does hashing degrade to O(n)?** When many keys collide into the same bucket — either from a poor hash function or a deliberately adversarial input (hash DoS). In competitive programming, unordered_map in C++ can be hacked with crafted inputs. Use custom hash or `map` (O(log n), red-black tree) in such cases.

**Python `dict` vs C++ `map` vs C++ `unordered_map`:**

| | Python `dict` | C++ `unordered_map` | C++ `map` |
|---|---|---|---|
| Implementation | Hash table | Hash table | Red-black tree |
| Lookup | O(1) avg | O(1) avg | O(log n) |
| Ordered? | Yes (Python 3.7+, insertion order) | No | Yes (sorted by key) |
| Use when | General-purpose | Speed, no order needed | Need sorted keys or range queries |

**HashSet for membership testing:**
```python
# Python set
seen = set()
seen.add(x)
x in seen         # O(1) avg
seen.remove(x)
```
```cpp
// C++
unordered_set<int> seen;
seen.insert(x);
seen.count(x)     // 0 or 1, O(1) avg
seen.erase(x);
```

**When to use HashMap vs HashSet:**
- **HashMap** when you need to store a *value* associated with a key (frequency, index, count).
- **HashSet** when you only need to test *membership* (seen before? duplicate? in set?).

---

## 4. The 6 Core Array Patterns

### Pattern 1 — Brute Force → Optimization Mindset

The mental process every interviewer expects: start with the naive O(n²) or O(n³) solution, identify what's being recomputed, and eliminate that redundancy.

**Brute force → Two Pointers:** Nested loop checking all pairs → two pointers checking one pair per step.

**Brute force → Prefix Sum:** Computing subarray sum from scratch each time → precompute prefix sums for O(1) range queries.

**Brute force → HashMap:** Scanning the array for every existence check → put everything in a hash set first for O(1) lookup.

**Template thought process:**
1. What is the brute force? What's its complexity?
2. What is being repeatedly computed?
3. Can I precompute it (prefix, sort) or cache it (hashmap)?
4. Does a constraint (sorted, positive only, small range) enable a special algorithm?

---

### Pattern 2 — Two Pointers

Use two pointers — typically at both ends of a sorted array or both starting at the same end — to reduce nested loop O(n²) to O(n).

**Trigger:** "Find pair/triplet with target sum", "remove duplicates from sorted array", "move elements", "check palindrome".

**Template 1 — Opposite ends (sorted array):**
```python
def two_sum_sorted(arr, target):
    l, r = 0, len(arr) - 1
    while l < r:
        s = arr[l] + arr[r]
        if s == target:
            return [l, r]
        elif s < target:
            l += 1   # Need larger sum → move left pointer right
        else:
            r -= 1   # Need smaller sum → move right pointer left
    return [-1, -1]
```

**Template 2 — Same direction (fast/slow):**
```python
def remove_duplicates(nums):
    if not nums:
        return 0
    slow = 0              # Tracks position for next unique element
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]
    return slow + 1       # Length of unique portion
```

---

### Pattern 3 — Sliding Window

Maintain a "window" (contiguous subarray) whose size or content changes as we slide it across the array. Replace O(n²) brute force with O(n) by incrementally updating the window instead of recomputing from scratch.

**Fixed-size window:**
```python
def max_sum_subarray_k(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum
    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i - k]   # Slide: add new, remove old
        max_sum = max(max_sum, window_sum)
    return max_sum
```

**Variable-size window (expand/shrink):**
```python
def longest_subarray_sum_atmost_k(arr, k):
    l = window_sum = max_len = 0
    for r in range(len(arr)):
        window_sum += arr[r]
        while window_sum > k:   # Shrink from left
            window_sum -= arr[l]
            l += 1
        max_len = max(max_len, r - l + 1)
    return max_len
```

**Key insight:** The right pointer always moves forward. The left pointer only moves forward too (never backward). So each element is added once and removed once → total O(n).

---

### Pattern 4 — Prefix Sum

Precompute cumulative sums so that any subarray sum can be retrieved in O(1).

`prefix[i] = arr[0] + arr[1] + ... + arr[i-1]` (with prefix[0] = 0 as sentinel)

Sum of subarray `arr[l..r]` = `prefix[r+1] - prefix[l]`

```python
def build_prefix(arr):
    prefix = [0] * (len(arr) + 1)
    for i, x in enumerate(arr):
        prefix[i + 1] = prefix[i] + x
    return prefix

def range_sum(prefix, l, r):
    return prefix[r + 1] - prefix[l]

arr = [1, 2, 3, 4, 5]
p = build_prefix(arr)
print(range_sum(p, 1, 3))  # arr[1]+arr[2]+arr[3] = 2+3+4 = 9
```

**The prefix sum + hashmap combo** is the key to solving "number of subarrays with sum = k":
- If `prefix[j] - prefix[i] = k`, then the subarray from i to j-1 has sum k.
- Rearranged: `prefix[i] = prefix[j] - k`.
- As we scan right, for each new prefix sum, check how many times `current_prefix - k` has appeared before.

---

### Pattern 5 — HashMap / HashSet

The most versatile pattern. Reduces any "find pair/element with property X" from O(n) search per query to O(1).

**Frequency counting:**
```python
from collections import Counter, defaultdict

def most_frequent(arr):
    freq = Counter(arr)           # {value: count}
    return freq.most_common(1)[0] # (value, count) of most frequent

def group_anagrams(words):
    groups = defaultdict(list)
    for w in words:
        key = tuple(sorted(w))    # Canonical form: sorted letters
        groups[key].append(w)
    return list(groups.values())
```

**Existence check (HashSet):**
```python
def contains_duplicate(nums):
    return len(nums) != len(set(nums))  # One-liner

def contains_duplicate_verbose(nums):
    seen = set()
    for x in nums:
        if x in seen:
            return True
        seen.add(x)
    return False
```

**C++ unordered_map patterns:**
```cpp
// Frequency map
unordered_map<int,int> freq;
for (int x : arr) freq[x]++;

// Check existence before accessing
if (freq.count(x)) { /* x exists */ }
if (freq.find(x) != freq.end()) { /* same */ }

// defaultdict equivalent: unordered_map auto-initializes to 0
freq[x]++;  // If x not in map, inserts with value 0, then increments
```

---

### Pattern 6 — Kadane's Algorithm

Kadane's algorithm finds the maximum sum contiguous subarray in O(n) time.

**Core insight:** At each position, decide: should we extend the existing subarray or start a new one from here? We start fresh if the current running sum has become negative — a negative prefix only hurts future sums.

```python
def max_subarray_sum(nums):
    max_sum = nums[0]
    current_sum = nums[0]

    for num in nums[1:]:
        current_sum = max(num, current_sum + num)  # Extend or restart
        max_sum = max(max_sum, current_sum)

    return max_sum

# [−2, 1, −3, 4, −1, 2, 1, −5, 4] → 6 (subarray [4, −1, 2, 1])
```

```cpp
int maxSubarraySum(vector<int>& nums) {
    int maxSum = nums[0], curSum = nums[0];
    for (int i = 1; i < nums.size(); i++) {
        curSum = max(nums[i], curSum + nums[i]);
        maxSum = max(maxSum, curSum);
    }
    return maxSum;
}
```

**Variant — also return the subarray indices:**
```python
def max_subarray_with_indices(nums):
    max_sum = nums[0]
    cur_sum = nums[0]
    start = end = temp_start = 0

    for i in range(1, len(nums)):
        if cur_sum + nums[i] < nums[i]:
            cur_sum = nums[i]
            temp_start = i
        else:
            cur_sum += nums[i]

        if cur_sum > max_sum:
            max_sum = cur_sum
            start = temp_start
            end = i

    return max_sum, start, end
```

---

## 5. Problems — Easy (Must Solve First)

### E1 — Two Sum (LeetCode 1) ⭐⭐⭐

Find indices of two numbers that add up to target.

**Brute force:** O(n²) — check all pairs.
**Optimal (HashMap):** O(n) — for each number, check if `target - num` is in the hashmap.

```python
def two_sum(nums, target):
    seen = {}   # value → index
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []

print(two_sum([2, 7, 11, 15], 9))   # [0, 1]
print(two_sum([3, 2, 4], 6))        # [1, 2]
```

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int,int> seen;
    for (int i = 0; i < nums.size(); i++) {
        int comp = target - nums[i];
        if (seen.count(comp))
            return {seen[comp], i};
        seen[nums[i]] = i;
    }
    return {};
}
```

**Time:** O(n) | **Space:** O(n) | Asked at: Amazon, Google, Facebook, Microsoft, Flipkart, virtually everywhere

---

### E2 — Best Time to Buy and Sell Stock (LeetCode 121) ⭐⭐⭐

Find maximum profit from a single buy-sell. You must buy before you sell.

**Key insight:** Track the minimum price seen so far. At each price, compute profit if we sell today (price - min_so_far). Update running max profit.

```python
def max_profit(prices):
    min_price = float('inf')
    max_profit = 0
    for price in prices:
        min_price = min(min_price, price)
        max_profit = max(max_profit, price - min_price)
    return max_profit

print(max_profit([7, 1, 5, 3, 6, 4]))  # 5 (buy at 1, sell at 6)
print(max_profit([7, 6, 4, 3, 1]))     # 0 (no profit possible)
```

```cpp
int maxProfit(vector<int>& prices) {
    int minPrice = INT_MAX, maxP = 0;
    for (int p : prices) {
        minPrice = min(minPrice, p);
        maxP = max(maxP, p - minPrice);
    }
    return maxP;
}
```

**Time:** O(n) | **Space:** O(1) | Asked at: Amazon, Goldman Sachs, JPMorgan, Morgan Stanley

---

### E3 — Contains Duplicate (LeetCode 217)

```python
def contains_duplicate(nums):
    return len(nums) != len(set(nums))
# Or equivalently:
def contains_duplicate_v2(nums):
    seen = set()
    for x in nums:
        if x in seen: return True
        seen.add(x)
    return False
```

**Time:** O(n) | **Space:** O(n)

---

### E4 — Maximum Subarray / Kadane's (LeetCode 53) ⭐⭐⭐

```python
def max_subarray(nums):
    cur = res = nums[0]
    for n in nums[1:]:
        cur = max(n, cur + n)
        res = max(res, cur)
    return res

print(max_subarray([-2,1,-3,4,-1,2,1,-5,4]))  # 6
```

**Time:** O(n) | **Space:** O(1) | Must explain Kadane's logic verbally.

---

### E5 — Missing Number (LeetCode 268)

Given array of n integers in range [0, n] with one missing, find it.

**Math approach:**
```python
def missing_number(nums):
    n = len(nums)
    return n * (n + 1) // 2 - sum(nums)
```

**XOR approach (no overflow risk):**
```python
def missing_number_xor(nums):
    result = len(nums)
    for i, num in enumerate(nums):
        result ^= i ^ num   # XOR cancels duplicates
    return result
```

**Time:** O(n) | **Space:** O(1)

---

### E6 — Move Zeroes (LeetCode 283)

Move all 0s to the end while maintaining order of non-zero elements.

```python
def move_zeroes(nums):
    insert_pos = 0
    for num in nums:
        if num != 0:
            nums[insert_pos] = num
            insert_pos += 1
    while insert_pos < len(nums):
        nums[insert_pos] = 0
        insert_pos += 1
```

```cpp
void moveZeroes(vector<int>& nums) {
    int insertPos = 0;
    for (int num : nums)
        if (num != 0) nums[insertPos++] = num;
    while (insertPos < nums.size())
        nums[insertPos++] = 0;
}
```

**Time:** O(n) | **Space:** O(1)

---

### E7 — Majority Element (LeetCode 169) — Boyer-Moore Voting

Find element appearing more than n/2 times.

**Boyer-Moore Voting Algorithm:** Maintain a candidate and a count. When count reaches 0, switch candidate. The majority element (if it exists) always survives.

```python
def majority_element(nums):
    candidate, count = None, 0
    for num in nums:
        if count == 0:
            candidate = num
        count += 1 if num == candidate else -1
    return candidate
```

```cpp
int majorityElement(vector<int>& nums) {
    int candidate = 0, count = 0;
    for (int num : nums) {
        if (count == 0) candidate = num;
        count += (num == candidate) ? 1 : -1;
    }
    return candidate;
}
```

**Time:** O(n) | **Space:** O(1) | If majority not guaranteed, verify with a second pass.

---

### E8 — Rotate Array (LeetCode 189)

Rotate array right by k steps.

**Reverse trick:** Reverse full → reverse first k → reverse last n-k.

```python
def rotate(nums, k):
    n = len(nums)
    k = k % n

    def reverse(l, r):
        while l < r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1; r -= 1

    reverse(0, n - 1)
    reverse(0, k - 1)
    reverse(k, n - 1)
```

**Time:** O(n) | **Space:** O(1)

---

## 6. Problems — Medium (Core Interview Level)

### M1 — Product of Array Except Self (LeetCode 238) ⭐⭐⭐

Return array where `output[i]` = product of all elements except `nums[i]`. No division. O(n) time and O(1) extra space.

**Key insight:** For each index i, output[i] = (product of everything to the left of i) × (product of everything to the right of i). Compute these two prefix/suffix products without division.

```python
def product_except_self(nums):
    n = len(nums)
    result = [1] * n

    # Forward pass: result[i] = product of nums[0..i-1]
    prefix = 1
    for i in range(n):
        result[i] = prefix
        prefix *= nums[i]

    # Backward pass: multiply result[i] by product of nums[i+1..n-1]
    suffix = 1
    for i in range(n - 1, -1, -1):
        result[i] *= suffix
        suffix *= nums[i]

    return result

print(product_except_self([1,2,3,4]))  # [24,12,8,6]
```

```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> result(n, 1);
    int prefix = 1;
    for (int i = 0; i < n; i++) {
        result[i] = prefix;
        prefix *= nums[i];
    }
    int suffix = 1;
    for (int i = n-1; i >= 0; i--) {
        result[i] *= suffix;
        suffix *= nums[i];
    }
    return result;
}
```

**Time:** O(n) | **Space:** O(1) extra (output array doesn't count) | LeetCode 238 | Asked at: Amazon, Facebook, Apple, Microsoft

---

### M2 — Subarray Sum Equals K (LeetCode 560) ⭐⭐⭐

Count the number of subarrays whose sum equals k.

**Key insight:** Use prefix sum + hashmap. If `prefix[j] - prefix[i] = k`, subarray [i..j-1] has sum k. Rearranged: `prefix[i] = prefix[j] - k`. As we compute prefix sums left to right, for each prefix[j] we look up how many times `prefix[j] - k` appeared previously.

```python
from collections import defaultdict

def subarray_sum(nums, k):
    count = 0
    prefix_sum = 0
    freq = defaultdict(int)
    freq[0] = 1   # Empty prefix has sum 0 — critical base case!

    for num in nums:
        prefix_sum += num
        count += freq[prefix_sum - k]   # How many valid i's for this j?
        freq[prefix_sum] += 1

    return count

print(subarray_sum([1, 1, 1], 2))       # 2
print(subarray_sum([1, 2, 3], 3))       # 2 ([1,2] and [3])
print(subarray_sum([-1, -1, 1], 0))     # 1 (works with negatives too!)
```

```cpp
int subarraySum(vector<int>& nums, int k) {
    int count = 0, prefixSum = 0;
    unordered_map<int,int> freq;
    freq[0] = 1;
    for (int num : nums) {
        prefixSum += num;
        count += freq[prefixSum - k];
        freq[prefixSum]++;
    }
    return count;
}
```

> **Why `freq[0] = 1`?** If a prefix sum from index 0 to j equals k, then `prefixSum - k = 0` must be found in the map. Without this initialization, we'd miss subarrays that start from index 0.

**Time:** O(n) | **Space:** O(n) | LeetCode 560 | Amazon, Google top interview question

---

### M3 — 3Sum (LeetCode 15) ⭐⭐⭐

Find all unique triplets that sum to zero.

**Key insight:** Sort the array. Fix one element (i), then use two pointers on the remaining sorted portion to find pairs summing to `-nums[i]`. Skip duplicates carefully.

```python
def three_sum(nums):
    nums.sort()
    result = []

    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:  # Skip duplicate i
            continue
        if nums[i] > 0:
            break  # All elements from here are positive, can't sum to 0

        l, r = i + 1, len(nums) - 1
        while l < r:
            s = nums[i] + nums[l] + nums[r]
            if s == 0:
                result.append([nums[i], nums[l], nums[r]])
                while l < r and nums[l] == nums[l + 1]: l += 1  # Skip dup l
                while l < r and nums[r] == nums[r - 1]: r -= 1  # Skip dup r
                l += 1; r -= 1
            elif s < 0:
                l += 1
            else:
                r -= 1

    return result

print(three_sum([-1, 0, 1, 2, -1, -4]))  # [[-1,-1,2],[-1,0,1]]
```

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> result;
    for (int i = 0; i < nums.size() - 2; i++) {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        if (nums[i] > 0) break;
        int l = i+1, r = nums.size()-1;
        while (l < r) {
            int s = nums[i] + nums[l] + nums[r];
            if (s == 0) {
                result.push_back({nums[i], nums[l], nums[r]});
                while (l < r && nums[l] == nums[l+1]) l++;
                while (l < r && nums[r] == nums[r-1]) r--;
                l++; r--;
            } else if (s < 0) l++;
            else r--;
        }
    }
    return result;
}
```

**Time:** O(n²) | **Space:** O(1) extra | LeetCode 15 | Google, Facebook, Amazon

---

### M4 — Container With Most Water (LeetCode 11)

Find two lines that form a container holding the most water.

**Key insight:** Two pointers at ends. Area = min(height[l], height[r]) × (r - l). Always move the pointer with the smaller height — moving the larger one can only decrease or maintain the minimum height, while also decreasing the width.

```python
def max_area(height):
    l, r = 0, len(height) - 1
    max_water = 0
    while l < r:
        water = min(height[l], height[r]) * (r - l)
        max_water = max(max_water, water)
        if height[l] < height[r]:
            l += 1
        else:
            r -= 1
    return max_water

print(max_area([1,8,6,2,5,4,8,3,7]))  # 49
```

**Time:** O(n) | **Space:** O(1) | LeetCode 11

---

### M5 — Longest Consecutive Sequence (LeetCode 128) ⭐⭐

Find length of longest sequence of consecutive integers. O(n) time required.

**Key insight:** Put all numbers in a HashSet. For each number x, only start counting if x-1 is NOT in the set (i.e., x is the start of a sequence). Then count upward. This ensures each element is visited at most twice.

```python
def longest_consecutive(nums):
    num_set = set(nums)
    max_length = 0

    for x in num_set:
        if x - 1 not in num_set:   # x is the start of a sequence
            current = x
            length = 1
            while current + 1 in num_set:
                current += 1
                length += 1
            max_length = max(max_length, length)

    return max_length

print(longest_consecutive([100,4,200,1,3,2]))  # 4 (sequence 1,2,3,4)
```

```cpp
int longestConsecutive(vector<int>& nums) {
    unordered_set<int> s(nums.begin(), nums.end());
    int maxLen = 0;
    for (int x : s) {
        if (!s.count(x - 1)) {   // Start of sequence
            int cur = x, len = 1;
            while (s.count(cur + 1)) { cur++; len++; }
            maxLen = max(maxLen, len);
        }
    }
    return maxLen;
}
```

**Time:** O(n) | **Space:** O(n) | LeetCode 128 | Google top interview question

---

### M6 — Maximum Product Subarray (LeetCode 152)

Find the contiguous subarray with the maximum product.

**Key insight:** Unlike sum, negatives flip signs. Track both the maximum AND minimum product ending at each position. A negative × negative = positive, so today's minimum might become tomorrow's maximum.

```python
def max_product(nums):
    max_prod = min_prod = result = nums[0]

    for num in nums[1:]:
        if num < 0:
            max_prod, min_prod = min_prod, max_prod  # Flip on negative

        max_prod = max(num, max_prod * num)
        min_prod = min(num, min_prod * num)
        result = max(result, max_prod)

    return result

print(max_product([2,3,-2,4]))   # 6
print(max_product([-2,0,-1]))    # 0
```

**Time:** O(n) | **Space:** O(1) | LeetCode 152

---

### M7 — Find Duplicate Number (LeetCode 287) — Floyd's Cycle Detection

Array contains n+1 integers in range [1, n]. Find the duplicate without modifying array and using O(1) space.

**Key insight:** Treat the array as a linked list where `arr[i]` is the "next pointer" from node i. Since there's a duplicate, there's a cycle. Use Floyd's tortoise-and-hare algorithm to find the cycle start.

```python
def find_duplicate(nums):
    # Phase 1: Find intersection point inside the cycle
    slow = fast = nums[0]
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break

    # Phase 2: Find the cycle entry (= the duplicate)
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]

    return slow

print(find_duplicate([1,3,4,2,2]))  # 2
print(find_duplicate([3,1,3,4,2]))  # 3
```

**Time:** O(n) | **Space:** O(1) | LeetCode 287

---

### M8 — Rotate Image / Matrix (LeetCode 48)

Rotate n×n matrix 90 degrees clockwise in-place.

**Key insight:** Transpose (flip along main diagonal) + reverse each row = 90° clockwise.

```python
def rotate(matrix):
    n = len(matrix)
    # Step 1: Transpose
    for i in range(n):
        for j in range(i + 1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    # Step 2: Reverse each row
    for row in matrix:
        row.reverse()
```

**Time:** O(n²) | **Space:** O(1)

---

### M9 — Group Anagrams (LeetCode 49) ⭐⭐

Group strings that are anagrams of each other.

```python
from collections import defaultdict

def group_anagrams(strs):
    groups = defaultdict(list)
    for s in strs:
        key = tuple(sorted(s))   # Canonical form: sorted letters
        groups[key].append(s)
    return list(groups.values())

print(group_anagrams(["eat","tea","tan","ate","nat","bat"]))
# [["eat","tea","ate"],["tan","nat"],["bat"]]
```

```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> groups;
    for (string& s : strs) {
        string key = s;
        sort(key.begin(), key.end());
        groups[key].push_back(s);
    }
    vector<vector<string>> result;
    for (auto& [k, v] : groups) result.push_back(v);
    return result;
}
```

**Time:** O(n × m log m) where m = max string length | LeetCode 49

---

### M10 — Top K Frequent Elements (LeetCode 347)

```python
from collections import Counter
import heapq

def top_k_frequent(nums, k):
    freq = Counter(nums)
    # Approach 1: heapq.nlargest — simple and fast
    return heapq.nlargest(k, freq.keys(), key=freq.get)

# Approach 2: Bucket sort — O(n) instead of O(n log k)
def top_k_frequent_bucket(nums, k):
    freq = Counter(nums)
    buckets = [[] for _ in range(len(nums) + 1)]
    for num, count in freq.items():
        buckets[count].append(num)
    result = []
    for i in range(len(buckets) - 1, 0, -1):
        result.extend(buckets[i])
        if len(result) >= k:
            return result[:k]
    return result
```

**Time:** O(n log k) heap | O(n) bucket | LeetCode 347

---

### M11 — Next Permutation (LeetCode 31) ⭐⭐

Find the next lexicographically greater permutation in-place.

**Algorithm:**
1. Find the rightmost index `i` where `nums[i] < nums[i+1]` (longest descending suffix).
2. Find the rightmost `j` where `nums[j] > nums[i]`.
3. Swap `nums[i]` and `nums[j]`.
4. Reverse the suffix starting at `i+1`.

```python
def next_permutation(nums):
    n = len(nums)
    i = n - 2

    # Step 1: Find rightmost ascent
    while i >= 0 and nums[i] >= nums[i + 1]:
        i -= 1

    if i >= 0:
        # Step 2: Find rightmost element greater than nums[i]
        j = n - 1
        while nums[j] <= nums[i]:
            j -= 1
        nums[i], nums[j] = nums[j], nums[i]  # Step 3: Swap

    # Step 4: Reverse suffix
    nums[i + 1:] = nums[i + 1:][::-1]

# [1,2,3] → [1,3,2]
# [3,2,1] → [1,2,3] (last permutation → first)
# [1,1,5] → [1,5,1]
```

**Time:** O(n) | **Space:** O(1) | LeetCode 31

---

### M12 — Set Matrix Zeroes (LeetCode 73)

If any cell in matrix is 0, set its entire row and column to 0. In-place.

```python
def set_zeroes(matrix):
    m, n = len(matrix), len(matrix[0])
    first_row_zero = any(matrix[0][j] == 0 for j in range(n))
    first_col_zero = any(matrix[i][0] == 0 for i in range(m))

    # Use first row and column as markers
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][j] == 0:
                matrix[i][0] = 0   # Mark row
                matrix[0][j] = 0   # Mark column

    # Zero out cells based on markers
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][0] == 0 or matrix[0][j] == 0:
                matrix[i][j] = 0

    if first_row_zero:
        for j in range(n): matrix[0][j] = 0
    if first_col_zero:
        for i in range(m): matrix[i][0] = 0
```

**Time:** O(m×n) | **Space:** O(1)

---

## 7. Problems — Hard (Differentiators)

### H1 — Trapping Rain Water (LeetCode 42) ⭐⭐⭐

Given heights, compute total water trapped.

**Key insight — Two Pointers:** Water at position i = `min(max_left, max_right) - height[i]`. Use two pointers and maintain running max_left, max_right. Move the pointer with the smaller max height — because the other side's contribution is already bounded by the smaller height.

```python
def trap(height):
    l, r = 0, len(height) - 1
    max_left = max_right = 0
    water = 0

    while l < r:
        if max_left <= max_right:
            max_left = max(max_left, height[l])
            water += max_left - height[l]
            l += 1
        else:
            max_right = max(max_right, height[r])
            water += max_right - height[r]
            r -= 1

    return water

print(trap([0,1,0,2,1,0,1,3,2,1,2,1]))  # 6
```

```cpp
int trap(vector<int>& height) {
    int l = 0, r = height.size()-1;
    int maxL = 0, maxR = 0, water = 0;
    while (l < r) {
        if (maxL <= maxR) {
            maxL = max(maxL, height[l]);
            water += maxL - height[l++];
        } else {
            maxR = max(maxR, height[r]);
            water += maxR - height[r--];
        }
    }
    return water;
}
```

**Time:** O(n) | **Space:** O(1) | LeetCode 42 | Google, Amazon, Microsoft top question

---

### H2 — Longest Subarray with Sum K (positive + negative numbers)

```python
def longest_subarray_sum_k(arr, k):
    prefix_sum = 0
    max_len = 0
    first_occurrence = {}   # prefix_sum → earliest index where seen

    for i, num in enumerate(arr):
        prefix_sum += num
        if prefix_sum == k:
            max_len = i + 1   # Entire arr[0..i] sums to k
        if prefix_sum - k in first_occurrence:
            max_len = max(max_len, i - first_occurrence[prefix_sum - k])
        if prefix_sum not in first_occurrence:
            first_occurrence[prefix_sum] = i   # Store FIRST occurrence only

    return max_len

print(longest_subarray_sum_k([1, -1, 5, -2, 3], 3))  # 4
```

**Time:** O(n) | **Space:** O(n)

---

### H3 — Count Subarrays with XOR = K

```python
from collections import defaultdict

def count_xor_subarrays(arr, k):
    prefix_xor = 0
    count = 0
    freq = defaultdict(int)
    freq[0] = 1

    for x in arr:
        prefix_xor ^= x
        count += freq[prefix_xor ^ k]   # Same as prefix sum trick but with XOR
        freq[prefix_xor] += 1

    return count
```

**The XOR trick is identical to the prefix sum + hashmap trick.** `arr[l..r] XOR = k` iff `prefix_xor[r] XOR prefix_xor[l-1] = k` iff `prefix_xor[l-1] = prefix_xor[r] XOR k`.

---

### H4 — 4Sum (LeetCode 18)

Generalization of 3Sum. Fix two elements, use two pointers for the remaining pair.

```python
def four_sum(nums, target):
    nums.sort()
    result = []
    n = len(nums)

    for i in range(n - 3):
        if i > 0 and nums[i] == nums[i-1]: continue
        if nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target: break   # Pruning
        if nums[i] + nums[n-3] + nums[n-2] + nums[n-1] < target: continue # Pruning

        for j in range(i + 1, n - 2):
            if j > i + 1 and nums[j] == nums[j-1]: continue
            l, r = j + 1, n - 1
            while l < r:
                s = nums[i] + nums[j] + nums[l] + nums[r]
                if s == target:
                    result.append([nums[i], nums[j], nums[l], nums[r]])
                    while l < r and nums[l] == nums[l+1]: l += 1
                    while l < r and nums[r] == nums[r-1]: r -= 1
                    l += 1; r -= 1
                elif s < target:
                    l += 1
                else:
                    r -= 1
    return result
```

**Time:** O(n³) | LeetCode 18

---

### H5 — Merge K Sorted Arrays

```python
import heapq

def merge_k_sorted(arrays):
    result = []
    # Min-heap: (value, array_index, element_index)
    heap = []
    for i, arr in enumerate(arrays):
        if arr:
            heapq.heappush(heap, (arr[0], i, 0))

    while heap:
        val, arr_idx, elem_idx = heapq.heappop(heap)
        result.append(val)
        if elem_idx + 1 < len(arrays[arr_idx]):
            heapq.heappush(heap, (arrays[arr_idx][elem_idx + 1], arr_idx, elem_idx + 1))

    return result

print(merge_k_sorted([[1,4,5],[1,3,4],[2,6]]))  # [1,1,2,3,4,4,5,6]
```

**Time:** O(N log k) where N = total elements, k = number of arrays | LeetCode 23

---

## 8. Interview Q&A Bank

**Q1: What is the time complexity of accessing, inserting, and deleting from an array?**
Access is O(1) — direct index computation via `base + i × size`. Insertion/deletion at the end is O(1) amortized for dynamic arrays (occasional resize is O(n) but amortized O(1)). Insertion/deletion at an arbitrary position is O(n) because all subsequent elements must be shifted.

**Q2: How does a hashmap achieve O(1) average time? When does it degrade?**
A hashmap applies a hash function to convert a key into an array index (bucket). With a good hash function and low load factor, each bucket holds O(1) expected elements, giving O(1) lookup. It degrades to O(n) when many keys collide into the same bucket — either from poor hash function design or adversarial input. Python's dict and C++'s unordered_map use techniques like prime-based table sizes and polynomial rolling hashes to minimize this.

**Q3: What is the difference between `map` and `unordered_map` in C++?**
`unordered_map` is a hash table — O(1) average for all operations, but no ordering. `map` is a red-black tree (balanced BST) — O(log n) for all operations, but keys are always sorted and support range queries. Use `unordered_map` by default for speed; use `map` when you need sorted iteration or range queries like "find all keys between 5 and 10."

**Q4: Explain Kadane's algorithm and its core intuition.**
Kadane's algorithm finds the maximum sum contiguous subarray in O(n) time. At each position, we decide: is it better to extend the current subarray, or start fresh from this element? We extend if `current_sum + nums[i] > nums[i]`, i.e., if `current_sum > 0`. If the running sum has gone negative, it only hurts future sums, so we restart. Track the global maximum throughout.

**Q5: When should you use prefix sum vs sliding window vs hashmap for subarray problems?**
Use **prefix sum** when you need range sum queries repeatedly after O(n) preprocessing, or for "number of subarrays with sum = k" (prefix sum + hashmap). Use **sliding window** when the constraint is on contiguous elements and you need max/min length of a valid window — works best when adding/removing elements from the window can be done incrementally. Use **hashmap** when you need O(1) lookup of whether a value has been seen, or to store the first/last index of a value for length calculations.

**Q6: How would you find two numbers in an unsorted array that sum to a target?**
Use a hashmap. For each number x, check if `target - x` is in the hashmap. If yes, return the pair. If no, add x to the map. This is O(n) time and O(n) space — the classic Two Sum pattern (LeetCode 1).

**Q7: What is the Boyer-Moore Voting Algorithm?**
An O(n) time, O(1) space algorithm to find the majority element (appears more than n/2 times). Maintain a candidate and a count. For each element: if count is 0, make it the new candidate and set count to 1. If current element equals candidate, increment count; otherwise decrement count. The majority element always survives because it has more votes than all others combined. If majority is not guaranteed, verify the candidate in a second pass.

**Q8: How does Floyd's cycle detection algorithm apply to finding duplicates in an array?**
Treat the array as a linked list where index i points to `arr[i]`. A duplicate value means two indices point to the same next node — creating a cycle. Floyd's algorithm finds the cycle using slow (1 step) and fast (2 steps) pointers. Once they meet inside the cycle, reset one pointer to the start and move both one step at a time — they meet at the cycle entry, which is the duplicate.

**Q9: What is the sliding window technique and when should you use it?**
The sliding window uses two pointers (left and right) to maintain a contiguous subarray or substring. The right pointer expands the window by including a new element; the left pointer shrinks it when a constraint is violated. Since each element is added and removed at most once, the total operations are O(n). Use it for problems asking for maximum/minimum length subarray satisfying a condition, or fixed-size window statistics.

**Q10: What is the "prefix sum + hashmap" pattern and what problems does it solve?**
It solves "count subarrays with sum (or XOR) = k" in O(n). Build a running prefix sum as you scan left to right. For each new prefix[j], look up how many times `prefix[j] - k` appeared before (using a hashmap). If found, those earlier positions i give subarrays [i..j-1] with sum exactly k. Initialize the hashmap with `{0: 1}` to handle subarrays starting from index 0. The same pattern works for XOR (replace + with ^).

**Q11: How do you find the longest consecutive sequence in O(n)?**
Put all elements in a HashSet. For each element x, only start counting if x-1 is NOT in the set (ensuring x is a sequence start). Count upward as long as consecutive elements exist. Since each element is the start of at most one sequence, and each element is visited at most twice (once when checked as start, once when extending), total time is O(n).

**Q12: Explain the two-pointer approach for 3Sum.**
Sort the array — O(n log n). Iterate with index i fixing the first element. For the remaining subarray, apply the two-pointer technique: left starts at i+1, right at the end. If sum < 0, move left pointer right; if sum > 0, move right pointer left; if exactly 0, record the triplet. Skip duplicates at all three positions to avoid repeated triplets. Total time is O(n²).

**Q13: What is Trapping Rain Water and what's the optimal approach?**
Water trapped at index i = min(max height to the left, max height to the right) minus height[i]. The two-pointer approach avoids precomputing prefix/suffix max arrays. Maintain running max_left and max_right. Always process the side with the smaller running max, because the trapped water at that position is bounded by the smaller side. O(n) time, O(1) space.

**Q14: What is the Product of Array Except Self and why can't you use division?**
The problem explicitly disallows division. The solution uses two passes: first pass computes prefix products (product of all elements to the left of i), second pass multiplies by suffix products (product of all elements to the right of i). This gives each `result[i]` = product of all elements except `nums[i]` in O(n) time and O(1) extra space. Division would fail for arrays containing zeros.

**Q15: When should you use a HashSet vs HashMap?**
Use a **HashSet** when you only need to know if an element exists (membership testing): detecting duplicates, checking if a value has been seen before. Use a **HashMap** when you need to associate a value with a key: frequency counting, storing first/last index, grouping elements, or any "lookup and retrieve" operation. HashSet is a HashMap with dummy values — it has the same O(1) average complexity.

---

**Answer Framework — "Solve this array problem":**
1. Clarify: Input type? Sorted? Duplicates? Return type?
2. State brute force and its complexity.
3. Identify the pattern: two pointers / sliding window / prefix sum / hashmap?
4. Explain the key insight in one sentence.
5. Write the code cleanly with edge case handling.
6. State final time and space complexity.

**Depth Gauge:**
- **Junior (SDE-1):** Two Sum, Max Subarray (Kadane's), Best Time to Buy/Sell, Contains Duplicate, Product Except Self. Explain brute force and optimal clearly.
- **Mid-level (SDE-2):** 3Sum, Subarray Sum=K, Longest Consecutive, Trapping Rain Water, Max Product Subarray. Know all 6 patterns and when to use each.
- **Senior (SDE-3+):** Next Permutation, Merge K Sorted, Set Matrix Zeroes, sliding window hard variants. Discuss space-optimal solutions, generalize patterns to new problem types.

---

## 9. Problem Master List with LeetCode Links

**Easy (Solve all before moving to Medium):**

| # | Problem | Pattern | Key Insight |
|---|---|---|---|
| LC 1 | Two Sum | HashMap | Store complement |
| LC 121 | Best Time to Buy Stock | Greedy | Track min price |
| LC 217 | Contains Duplicate | HashSet | Set vs length |
| LC 53 | Maximum Subarray | Kadane's | Restart when negative |
| LC 268 | Missing Number | Math/XOR | Sum formula |
| LC 283 | Move Zeroes | Two Ptr | Slow/fast pointer |
| LC 169 | Majority Element | Boyer-Moore | Voting algorithm |
| LC 189 | Rotate Array | Reverse trick | 3 reverses |
| LC 905 | Sort Array by Parity | Two Ptr | Left/right ends |
| LC 485 | Max Consecutive Ones | Sliding | Count/reset |

**Medium (Core interview level):**

| # | Problem | Pattern | Key Insight |
|---|---|---|---|
| LC 238 | Product Except Self | Prefix+Suffix | Two-pass no division |
| LC 560 | Subarray Sum = K | Prefix+HashMap | freq[prefix - k] |
| LC 15 | 3Sum | Sort+Two Ptr | Fix i, two ptr rest |
| LC 11 | Container With Most Water | Two Ptr | Move smaller height |
| LC 128 | Longest Consecutive Seq | HashSet | Start only if x-1 absent |
| LC 152 | Maximum Product Subarray | DP variant | Track max & min |
| LC 287 | Find the Duplicate | Floyd's Cycle | Linked list cycle |
| LC 48 | Rotate Image | Matrix | Transpose + reverse rows |
| LC 49 | Group Anagrams | HashMap | Sorted string as key |
| LC 347 | Top K Frequent | Heap/Bucket | Counter + nlargest |
| LC 31 | Next Permutation | Array manip | Find ascent + reverse |
| LC 73 | Set Matrix Zeroes | In-place | First row/col as marker |
| LC 56 | Merge Intervals | Sort+Greedy | Sort by start, compare ends |
| LC 75 | Sort Colors | Dutch National Flag | 3-pointer |
| LC 229 | Majority Element II | Boyer-Moore | 2 candidates |

**Hard:**

| # | Problem | Pattern | Key Insight |
|---|---|---|---|
| LC 42 | Trapping Rain Water | Two Ptr | Move smaller max side |
| LC 84 | Largest Rectangle Histogram | Monotonic Stack | Stack of indices |
| LC 239 | Sliding Window Maximum | Deque | Monotonic deque |
| LC 41 | First Missing Positive | Index as hash | Negate visited indices |
| LC 23 | Merge K Sorted Lists | Min-Heap | Heap of (val, list, idx) |
| GFG | Count Inversions | Merge Sort mod | Count during merge step |
| GFG | Longest Subarray Sum K | Prefix+HashMap | First occurrence only |

---

## 10. Source Notes

| Source | Contribution |
|---|---|
| Striver A2Z (takeuforward.org) | Problem sequence: Easy → Medium → Hard, core problems list |
| TechInterviewHandbook (arrays) | Pattern taxonomy, sliding window and two pointer templates |
| NeetCode.io | Kadane's variant with subarray tracking, sliding window template |
| AlgoMaster blog (15 LeetCode patterns) | Pattern trigger signals, prefix sum template |
| FutureJobs DSA blog | Indian interview context — Amazon SDE rounds, 6 core patterns, pattern coverage over raw volume |
| interviewing.io (hash tables) | HashMap vs unordered_map vs map deep dive, collision handling |
| GFG Top 50 Hashing | Problem list for hashing, Boyer-Moore, Floyd's cycle |
| LeetCode discussions | Two pointers 100-day list, sliding window summary, XOR subarray pattern |

---

## 11. Glossary

**Prefix Sum:** Cumulative sum array where `prefix[i]` = sum of all elements from index 0 to i-1. Enables O(1) range sum queries.

**Two Pointers:** Technique using two indices that scan the array from opposite ends or at different speeds, reducing O(n²) pair-finding to O(n).

**Sliding Window:** A two-pointer technique where both pointers move in the same direction to maintain a contiguous window satisfying a constraint.

**Kadane's Algorithm:** O(n) algorithm for maximum sum subarray. At each position, either extend the current subarray or restart from the current element.

**HashMap / Hash Table:** Data structure mapping keys to values in O(1) average time using a hash function. Python: `dict`. C++: `unordered_map`.

**HashSet:** Set-only version of HashMap — stores unique elements, supports O(1) membership testing. Python: `set`. C++: `unordered_set`.

**Boyer-Moore Voting:** O(1) space algorithm to find majority element (>n/2 occurrences) in a single pass using candidate + vote counter.

**Floyd's Cycle Detection:** Tortoise-and-hare algorithm detecting cycles in a sequence using slow (1 step) and fast (2 steps) pointers. Used to find duplicates by treating arrays as linked lists.

**Dutch National Flag:** Dijkstra's 3-way partition algorithm sorting 0s, 1s, 2s in O(n) with O(1) space using three pointers.

**Inversion:** Array pair (i, j) where i < j but arr[i] > arr[j]. Number of inversions measures how unsorted an array is.

**Load Factor:** Ratio of stored items to available buckets in a hash table. Controls performance — higher load factor increases collision probability.

**Amortized O(1):** An operation that occasionally takes O(n) (e.g., array resize) but averages out to O(1) per operation over a sequence of operations.

---

*You've now covered the highest-ROI topics for product company interviews: Basics → OOP → Recursion → Sorting → Arrays + Hashing.*  
*Next recommended topic: **Binary Search** — unlocks O(log n) thinking, builds directly on sorted arrays.*
