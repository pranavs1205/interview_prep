# Binary Search — Interview Prep Book
### Python & C++ | Striver A2Z Step 4 | 2026 Edition

> **Sources:** Striver A2Z, NeetCode, AlgoMaster, TechInterviewHandbook, interviewing.io, HelloInterview, LeetCode discussions, GFG, AlgoMonster  
> **Depth:** Interview (Deep) | **Languages:** Python + C++  
> **Split:** ~25% Theory | ~75% Problems + Solutions  
> **Generated:** June 2026

---

## Table of Contents

1. [Quick Reference Card](#1-quick-reference-card)
2. [Theory — Why Binary Search Works](#2-theory--why-binary-search-works)
3. [The 3 Templates You Must Know](#3-the-3-templates-you-must-know)
4. [The 4 Core Patterns](#4-the-4-core-patterns)
   - Pattern 1: Classic Binary Search (exact target)
   - Pattern 2: Lower Bound / Upper Bound / First & Last Occurrence
   - Pattern 3: Binary Search on Rotated / Modified Sorted Arrays
   - Pattern 4: Binary Search on Answer (Search Space)
5. [Problems — 1D Arrays (Easy → Hard)](#5-problems--1d-arrays-easy--hard)
6. [Problems — Binary Search on 2D Matrices](#6-problems--binary-search-on-2d-matrices)
7. [Problems — Binary Search on Answer Space](#7-problems--binary-search-on-answer-space)
8. [Built-in Binary Search Functions](#8-built-in-binary-search-functions)
9. [Common Mistakes & Gotchas](#9-common-mistakes--gotchas)
10. [Interview Q&A Bank](#10-interview-qa-bank)
11. [Problem Master List](#11-problem-master-list)
12. [Source Notes](#12-source-notes)
13. [Glossary](#13-glossary)

---

## 1. Quick Reference Card

**The single most important decision in binary search:** which half do you discard?

**Complexity:** Time O(log n) | Space O(1) iterative, O(log n) recursive

**The 4 patterns and when to use them:**

| Pattern | Trigger Signal | Example Problems |
|---|---|---|
| Classic BS | Sorted array, find exact target | Binary Search, Search Insert Position |
| Lower/Upper Bound | "First occurrence", "Last occurrence", "How many ≤ x?" | Find First/Last Position, Count occurrences |
| Rotated / Modified | "Array was sorted then rotated", "Peak element" | Search in Rotated Array, Find Minimum in Rotated |
| BS on Answer Space | "Minimum X such that feasible(X)", monotonic Yes/No | Koko Bananas, Capacity to Ship, Median of Arrays |

**The one rule that prevents infinite loops:**

> Every iteration, **at least one pointer must move**. If `mid` could equal `lo` or `hi` and you set a boundary to `mid` without moving, you will loop forever.

**Overflow-safe midpoint (use this always):**
```python
mid = lo + (hi - lo) // 2   # Python
```
```cpp
int mid = lo + (hi - lo) / 2;  // C++
```

---

## 2. Theory — Why Binary Search Works

Binary search exploits a sorted or monotone structure to eliminate half the search space with each comparison. Instead of checking n elements linearly, we check log₂(n) elements.

**The key prerequisite isn't just "sorted" — it's "monotone."** A sequence is monotone if once a condition becomes true it stays true (or vice versa). Sorted arrays are monotone by definition. But many other problems have a hidden monotone structure: "Can we do task X in time T?" — if yes for T, then yes for any T' > T too. This is the insight that unlocks binary search on the answer space.

**Why log₂(n)?** Each comparison halves the search space. Starting from n elements:
- After 1 comparison: n/2 remain
- After 2 comparisons: n/4 remain
- After k comparisons: n/2ᵏ remain

We stop when n/2ᵏ = 1, so k = log₂(n). For n = 1,000,000: only ~20 comparisons needed.

**Analogy:** Imagine guessing a number between 1 and 1,000,000. With linear search: up to 1,000,000 guesses. With binary search: at most 20 guesses. Every time your opponent says "too high" or "too low," you eliminate exactly half the remaining candidates.

**When is binary search applicable?**
1. The array (or answer space) is **sorted or monotone**.
2. You can **test a mid-point** quickly — in O(1) for arrays, or O(n) for answer-space problems.
3. You can determine **which half to discard** based on the test result.

---

## 3. The 3 Templates You Must Know

Binary search has several valid implementations that differ in loop condition and boundary updates. Using inconsistent templates is the #1 cause of off-by-one bugs. Pick **one** per problem type and apply it mechanically.

### Template 1 — Exact Target Search (`lo <= hi`)

Use when: you're searching for a specific value and will return immediately on finding it.

```python
def binary_search(nums, target):
    lo, hi = 0, len(nums) - 1

    while lo <= hi:
        mid = lo + (hi - lo) // 2

        if nums[mid] == target:
            return mid           # Found
        elif nums[mid] < target:
            lo = mid + 1         # Search right half
        else:
            hi = mid - 1         # Search left half

    return -1                    # Not found
```

```cpp
int binarySearch(vector<int>& nums, int target) {
    int lo = 0, hi = nums.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}
```

**When loop ends:** `lo > hi`. All elements have been considered. Return -1 for "not found".

---

### Template 2 — Find Leftmost / Lower Bound (`lo < hi`, right-biased)

Use when: you need the **first position** where a condition is true. The answer is always in `[lo, hi]`.

```python
def lower_bound(nums, target):
    """First index where nums[i] >= target. Returns len(nums) if all < target."""
    lo, hi = 0, len(nums)       # hi = len(nums) (one past end) as sentinel

    while lo < hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] < target:
            lo = mid + 1        # mid is too small, move lo right
        else:
            hi = mid            # mid could be the answer, shrink hi to mid

    return lo                   # lo == hi = answer index
```

```cpp
int lowerBound(vector<int>& nums, int target) {
    int lo = 0, hi = nums.size();  // hi is exclusive
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] < target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}
// C++ built-in: lower_bound(v.begin(), v.end(), target) - v.begin()
```

**When loop ends:** `lo == hi`. This is the insertion point / first occurrence index.

---

### Template 3 — Find Rightmost / Upper Bound (`lo < hi`, left-biased)

Use when: you need the **last position** where a condition is true.

```python
def upper_bound(nums, target):
    """First index where nums[i] > target. Subtract 1 for last occurrence."""
    lo, hi = 0, len(nums)

    while lo < hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] <= target:
            lo = mid + 1        # mid is <= target, answer is to the right
        else:
            hi = mid

    return lo   # First index where value > target
                # Last occurrence of target = lo - 1

# C++ built-in: upper_bound(v.begin(), v.end(), target) - v.begin()
```

**Relationship:**
- `lower_bound(target)` = first index where value ≥ target
- `upper_bound(target)` = first index where value > target
- Count of target in array = `upper_bound(target) - lower_bound(target)`
- Last occurrence of target = `upper_bound(target) - 1`

---

## 4. The 4 Core Patterns

### Pattern 1 — Classic Binary Search

**Trigger:** Sorted array + find exact target. The most fundamental form.

**Key rule:** If `nums[mid] == target` → return. If `nums[mid] < target` → search right. If `nums[mid] > target` → search left.

See Template 1 above. Time O(log n), Space O(1).

---

### Pattern 2 — Lower Bound / Upper Bound

**Trigger:** "Find first occurrence", "Find last occurrence", "Count elements equal to X", "Search insert position."

The trick is to **not stop when you find the target** — continue searching the relevant half to push toward the boundary.

**Finding first occurrence (manual, no built-ins):**
```python
def first_occurrence(nums, target):
    lo, hi = 0, len(nums) - 1
    result = -1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] == target:
            result = mid         # Record answer
            hi = mid - 1        # Keep searching LEFT for earlier occurrence
        elif nums[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return result
```

**Finding last occurrence:**
```python
def last_occurrence(nums, target):
    lo, hi = 0, len(nums) - 1
    result = -1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] == target:
            result = mid         # Record answer
            lo = mid + 1        # Keep searching RIGHT for later occurrence
        elif nums[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return result
```

---

### Pattern 3 — Rotated / Modified Sorted Arrays

**Trigger:** "Array was sorted then rotated", "Find peak element."

**The rotated array insight:** When you compute `mid` in a rotated sorted array, **at least one of the two halves is always sorted**. Determine which half is sorted by comparing `nums[lo]` with `nums[mid]`. Then check if the target falls within the sorted half — if yes, search there; if no, search the other half.

```
Original: [1, 2, 3, 4, 5, 6, 7]
Rotated:  [4, 5, 6, 7, 1, 2, 3]
              ↑ pivot
```

If `nums[lo] <= nums[mid]`: left half is sorted.
If `nums[lo] > nums[mid]`: right half is sorted.

---

### Pattern 4 — Binary Search on Answer Space

**The most powerful and frequently asked pattern.** Instead of searching in an array, you binary search over all *possible answers*.

**How to recognize it:**
- The problem asks for a "minimum X" or "maximum X" subject to a constraint.
- You can write a function `feasible(x)` that returns True/False.
- If `feasible(x)` is True, then `feasible(x+1)` is also True (or False — it's monotone).

**Template:**
```python
def solve():
    lo, hi = MINIMUM_POSSIBLE_ANSWER, MAXIMUM_POSSIBLE_ANSWER

    def feasible(mid):
        # Can we achieve the answer with value = mid?
        # Return True or False
        pass

    result = hi  # or lo, depending on whether minimizing or maximizing
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if feasible(mid):
            result = mid     # This works, try to do better (smaller/larger)
            hi = mid - 1     # Minimize: try smaller
            # lo = mid + 1   # Maximize: try larger
        else:
            lo = mid + 1     # Minimize: mid is too small
            # hi = mid - 1   # Maximize: mid is too large
    return result
```

The key question is always: **"What am I binary searching over?"** and **"What does feasible(mid) check?"**

---

## 5. Problems — 1D Arrays (Easy → Hard)

### E1 — Binary Search (LeetCode 704) ⭐

Classic. Find target in sorted array, return index or -1.

```python
def search(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] == target:   return mid
        elif nums[mid] < target:  lo = mid + 1
        else:                     hi = mid - 1
    return -1
```

```cpp
int search(vector<int>& nums, int target) {
    int lo = 0, hi = nums.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}
```

**Time:** O(log n) | **Space:** O(1)

---

### E2 — Search Insert Position (LeetCode 35)

Find the index where target would be inserted to maintain sorted order. Equivalent to `lower_bound`.

```python
def search_insert(nums, target):
    lo, hi = 0, len(nums)       # hi = len because target may go past the end
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] < target:
            lo = mid + 1
        else:
            hi = mid
    return lo

# [1,3,5,6], target=5 → 2
# [1,3,5,6], target=2 → 1
# [1,3,5,6], target=7 → 4
```

**Time:** O(log n) | **Space:** O(1)

---

### E3 — First Bad Version (LeetCode 278)

Given n versions, find the first bad one. `isBadVersion(version)` API returns true/false.

**Key insight:** First bad version is the "lower bound" of the condition `isBadVersion(v) == True`. Once a version is bad, all subsequent versions are also bad.

```python
def first_bad_version(n):
    lo, hi = 1, n
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if isBadVersion(mid):
            hi = mid         # mid could be first bad, keep it
        else:
            lo = mid + 1     # mid is good, first bad is to the right
    return lo
```

**Time:** O(log n) | **Space:** O(1) | LeetCode 278

---

### E4 — Square Root of X (LeetCode 69)

Find floor(sqrt(x)) without using sqrt().

```python
def my_sqrt(x):
    if x < 2:
        return x
    lo, hi = 1, x // 2      # sqrt(x) <= x//2 for x >= 4
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if mid * mid == x:
            return mid
        elif mid * mid < x:
            lo = mid + 1
        else:
            hi = mid - 1
    return hi                # hi is the floor (last mid where mid*mid <= x)
```

```cpp
int mySqrt(int x) {
    if (x < 2) return x;
    long long lo = 1, hi = x / 2;
    while (lo <= hi) {
        long long mid = lo + (hi - lo) / 2;
        if (mid * mid == x) return mid;
        else if (mid * mid < x) lo = mid + 1;
        else hi = mid - 1;
    }
    return hi;
}
```

> **Why `return hi`?** When the loop ends with `lo > hi`, `hi` is the largest integer whose square ≤ x — which is exactly floor(sqrt(x)).

**Time:** O(log x) | **Space:** O(1) | LeetCode 69

---

### M1 — Find First and Last Position of Element (LeetCode 34) ⭐⭐

Return `[first, last]` indices of target. O(log n) required.

```python
def search_range(nums, target):
    def first_occ(nums, target):
        lo, hi, res = 0, len(nums) - 1, -1
        while lo <= hi:
            mid = lo + (hi - lo) // 2
            if nums[mid] == target:
                res = mid
                hi = mid - 1    # Search LEFT for earlier
            elif nums[mid] < target:
                lo = mid + 1
            else:
                hi = mid - 1
        return res

    def last_occ(nums, target):
        lo, hi, res = 0, len(nums) - 1, -1
        while lo <= hi:
            mid = lo + (hi - lo) // 2
            if nums[mid] == target:
                res = mid
                lo = mid + 1    # Search RIGHT for later
            elif nums[mid] < target:
                lo = mid + 1
            else:
                hi = mid - 1
        return res

    return [first_occ(nums, target), last_occ(nums, target)]

print(search_range([5,7,7,8,8,10], 8))  # [3, 4]
print(search_range([5,7,7,8,8,10], 6))  # [-1, -1]
```

```cpp
vector<int> searchRange(vector<int>& nums, int target) {
    int first = -1, last = -1;
    int lo = 0, hi = nums.size() - 1;

    // First occurrence
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] == target) { first = mid; hi = mid - 1; }
        else if (nums[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }

    lo = 0; hi = nums.size() - 1;
    // Last occurrence
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] == target) { last = mid; lo = mid + 1; }
        else if (nums[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }

    return {first, last};
}
```

**Time:** O(log n) — two passes | **Space:** O(1) | LeetCode 34 | Amazon, Microsoft

---

### M2 — Search in Rotated Sorted Array (LeetCode 33) ⭐⭐⭐

Array was sorted, then rotated at some unknown pivot. Find target. O(log n).

**Key insight:** Split at `mid`. One of the two halves is **always fully sorted**. Check which one, then determine if target falls in that sorted half. If yes, search there; otherwise search the other half.

```python
def search_rotated(nums, target):
    lo, hi = 0, len(nums) - 1

    while lo <= hi:
        mid = lo + (hi - lo) // 2

        if nums[mid] == target:
            return mid

        # Left half is sorted
        if nums[lo] <= nums[mid]:
            if nums[lo] <= target < nums[mid]:  # Target in sorted left half
                hi = mid - 1
            else:                               # Target in right half
                lo = mid + 1
        # Right half is sorted
        else:
            if nums[mid] < target <= nums[hi]:  # Target in sorted right half
                lo = mid + 1
            else:                               # Target in left half
                hi = mid - 1

    return -1

print(search_rotated([4,5,6,7,0,1,2], 0))  # 4
print(search_rotated([4,5,6,7,0,1,2], 3))  # -1
print(search_rotated([1], 0))              # -1
```

```cpp
int search(vector<int>& nums, int target) {
    int lo = 0, hi = nums.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] == target) return mid;
        if (nums[lo] <= nums[mid]) {
            if (nums[lo] <= target && target < nums[mid]) hi = mid - 1;
            else lo = mid + 1;
        } else {
            if (nums[mid] < target && target <= nums[hi]) lo = mid + 1;
            else hi = mid - 1;
        }
    }
    return -1;
}
```

**Trace for [4,5,6,7,0,1,2], target=0:**
- lo=0, hi=6, mid=3: nums[3]=7 ≠ 0. nums[0]=4 ≤ nums[3]=7 → left sorted. 4 ≤ 0 < 7? No → lo = 4
- lo=4, hi=6, mid=5: nums[5]=1 ≠ 0. nums[4]=0 > nums[5]=1 → right sorted. 1 < 0 ≤ 2? No → hi = 4
- lo=4, hi=4, mid=4: nums[4]=0 == 0 → return 4 ✓

**Time:** O(log n) | **Space:** O(1) | LeetCode 33 | Google, Amazon, Microsoft top question

---

### M3 — Find Minimum in Rotated Sorted Array (LeetCode 153)

The minimum is at the pivot — the only element smaller than its left neighbour.

**Key insight:** The right half containing the pivot always has `nums[mid] < nums[hi]` (the right portion is still sorted). The minimum is always in the unsorted (rotated) half.

```python
def find_min(nums):
    lo, hi = 0, len(nums) - 1

    while lo < hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] > nums[hi]:
            lo = mid + 1    # Minimum is in the right half (pivot is there)
        else:
            hi = mid        # Minimum is at mid or to the left
    return nums[lo]

print(find_min([3,4,5,1,2]))    # 1
print(find_min([4,5,6,7,0,1,2])) # 0
print(find_min([11,13,15,17]))  # 11 (no rotation)
```

```cpp
int findMin(vector<int>& nums) {
    int lo = 0, hi = nums.size() - 1;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] > nums[hi]) lo = mid + 1;
        else hi = mid;
    }
    return nums[lo];
}
```

**Time:** O(log n) | **Space:** O(1) | LeetCode 153

---

### M4 — Find Peak Element (LeetCode 162)

A peak is an element greater than its neighbours. Find any peak index. O(log n).

**Key insight:** If `nums[mid] < nums[mid+1]`, the right side is ascending — a peak must exist to the right of mid. If `nums[mid] > nums[mid+1]`, the left side (including mid) must contain a peak.

```python
def find_peak(nums):
    lo, hi = 0, len(nums) - 1

    while lo < hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] < nums[mid + 1]:
            lo = mid + 1    # Peak is to the right (ascending slope → go up)
        else:
            hi = mid        # nums[mid] is a candidate peak (descending slope)

    return lo   # lo == hi = peak index

print(find_peak([1,2,3,1]))   # 2
print(find_peak([1,2,1,3,5,6,4]))  # 1 or 5 (any peak valid)
```

**Time:** O(log n) | **Space:** O(1) | LeetCode 162

---

### M5 — Single Element in a Sorted Array (LeetCode 540)

Every element appears exactly twice except one. Find the single element. O(log n), O(1) space.

**Key insight:** In a pair-perfect array, each pair occupies even-odd index positions (0-1, 2-3, 4-5...). If `nums[mid] == nums[mid^1]` (mid's pair), the single element is to the right; otherwise it's at mid or to the left. The XOR trick: `mid^1` flips the last bit — gives `mid-1` for odd mid, `mid+1` for even mid — always the pair index.

```python
def single_non_duplicate(nums):
    lo, hi = 0, len(nums) - 1

    while lo < hi:
        mid = lo + (hi - lo) // 2
        if mid % 2 == 1:
            mid -= 1          # Ensure mid is always even (start of a pair)

        if nums[mid] == nums[mid + 1]:
            lo = mid + 2      # Pair intact, single is to the right
        else:
            hi = mid          # Pair broken or mid is the single

    return nums[lo]

print(single_non_duplicate([1,1,2,3,3,4,4,8,8]))  # 2
print(single_non_duplicate([3,3,7,7,10,11,11]))    # 10
```

**Time:** O(log n) | **Space:** O(1) | LeetCode 540

---

### H1 — Search in Rotated Sorted Array II (LeetCode 81) — With Duplicates

Same as LeetCode 33 but now the array may have duplicates. The tricky case is `nums[lo] == nums[mid] == nums[hi]` — you can't determine which half is sorted.

```python
def search_rotated_duplicates(nums, target):
    lo, hi = 0, len(nums) - 1

    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] == target:
            return True

        # Can't determine sorted half when lo, mid, hi are equal
        if nums[lo] == nums[mid] == nums[hi]:
            lo += 1
            hi -= 1
        elif nums[lo] <= nums[mid]:  # Left half sorted
            if nums[lo] <= target < nums[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
        else:                        # Right half sorted
            if nums[mid] < target <= nums[hi]:
                lo = mid + 1
            else:
                hi = mid - 1

    return False
```

**Time:** O(log n) avg, O(n) worst case (all duplicates) | LeetCode 81

---

### H2 — Median of Two Sorted Arrays (LeetCode 4) ⭐⭐⭐

Find the median of two sorted arrays of sizes m and n. Required: O(log(min(m, n))).

**Key insight:** A median partitions the combined array into two equal halves. Binary search over partitions of the smaller array. For each partition of array A, there's a corresponding partition of array B. Check if the partition is valid: `A[i-1] <= B[j]` and `B[j-1] <= A[i]`. If not, adjust the partition.

```python
def find_median_sorted_arrays(nums1, nums2):
    # Ensure nums1 is the smaller array for O(log(min(m,n)))
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    m, n = len(nums1), len(nums2)
    lo, hi = 0, m

    while lo <= hi:
        i = lo + (hi - lo) // 2        # Partition in nums1
        j = (m + n + 1) // 2 - i       # Partition in nums2

        # Max of left half, min of right half for each array
        max_left1  = nums1[i-1] if i > 0 else float('-inf')
        min_right1 = nums1[i]   if i < m else float('inf')
        max_left2  = nums2[j-1] if j > 0 else float('-inf')
        min_right2 = nums2[j]   if j < n else float('inf')

        if max_left1 <= min_right2 and max_left2 <= min_right1:
            # Valid partition — compute median
            if (m + n) % 2 == 1:
                return float(max(max_left1, max_left2))
            else:
                return (max(max_left1, max_left2) + min(min_right1, min_right2)) / 2.0
        elif max_left1 > min_right2:
            hi = i - 1    # nums1's left partition is too large
        else:
            lo = i + 1    # nums1's left partition is too small

    raise ValueError("Input arrays are not sorted")

print(find_median_sorted_arrays([1,3], [2]))       # 2.0
print(find_median_sorted_arrays([1,2], [3,4]))     # 2.5
```

**Time:** O(log(min(m, n))) | **Space:** O(1) | LeetCode 4 | Google, Facebook, Amazon — hard flagship problem

---

## 6. Problems — Binary Search on 2D Matrices

### 2D-1 — Search a 2D Matrix (LeetCode 74) ⭐⭐

Matrix where each row is sorted AND the first element of each row > last element of previous row. Find target.

**Key insight:** The entire matrix is equivalent to a single sorted 1D array. Binary search on virtual index `[0, m*n-1]`. Convert 1D index to 2D: `row = mid // n`, `col = mid % n`.

```python
def search_matrix(matrix, target):
    m, n = len(matrix), len(matrix[0])
    lo, hi = 0, m * n - 1

    while lo <= hi:
        mid = lo + (hi - lo) // 2
        val = matrix[mid // n][mid % n]   # Convert 1D index to 2D

        if val == target:   return True
        elif val < target:  lo = mid + 1
        else:               hi = mid - 1

    return False

print(search_matrix([[1,3,5,7],[10,11,16,20],[23,30,34,60]], 3))   # True
print(search_matrix([[1,3,5,7],[10,11,16,20],[23,30,34,60]], 13))  # False
```

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size(), n = matrix[0].size();
    int lo = 0, hi = m * n - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        int val = matrix[mid / n][mid % n];
        if (val == target) return true;
        else if (val < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return false;
}
```

**Time:** O(log(m × n)) | **Space:** O(1) | LeetCode 74

---

### 2D-2 — Search a 2D Matrix II (LeetCode 240)

Rows sorted left-to-right, columns sorted top-to-bottom, BUT first element of each row is NOT necessarily greater than last element of the previous row (less restrictive than LC 74).

**Key insight:** Standard binary search doesn't work because the matrix isn't globally sorted. Instead, start from the **top-right corner**. If target < current: move left (smaller values). If target > current: move down (larger values). Each step eliminates a row or column.

```python
def search_matrix_ii(matrix, target):
    m, n = len(matrix), len(matrix[0])
    row, col = 0, n - 1    # Start at top-right corner

    while row < m and col >= 0:
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] > target:
            col -= 1    # Too large: move left
        else:
            row += 1    # Too small: move down

    return False
```

**Time:** O(m + n) | **Space:** O(1) | LeetCode 240

> **Note:** This is NOT binary search in the traditional sense — it's a two-pointer walk, but it's always taught alongside 2D binary search problems.

---

### 2D-3 — Row-wise Sorted Matrix Median (GFG Hard)

Find the median of a row-wise sorted m×n matrix (m×n is odd).

**Key insight:** Binary search on the answer space [min_element, max_element]. For a candidate median `x`, count how many elements in the matrix are ≤ x using binary search on each row (O(m log n)). The median is the smallest x where `count(x) >= (m*n+1)//2`.

```python
import bisect

def find_median(matrix):
    m, n = len(matrix), len(matrix[0])
    target_count = (m * n + 1) // 2

    # Search space: min element to max element in matrix
    lo = min(row[0] for row in matrix)
    hi = max(row[-1] for row in matrix)

    while lo < hi:
        mid = lo + (hi - lo) // 2

        # Count elements <= mid using binary search on each row
        count = sum(bisect.bisect_right(row, mid) for row in matrix)

        if count < target_count:
            lo = mid + 1    # mid is too small to be the median
        else:
            hi = mid        # mid could be the median or is too large

    return lo

print(find_median([[1,3,5],[2,6,9],[3,6,9]]))  # 5
```

```cpp
int findMedian(vector<vector<int>>& matrix) {
    int m = matrix.size(), n = matrix[0].size();
    int target = (m * n + 1) / 2;
    int lo = matrix[0][0], hi = matrix[0][0];
    for (auto& row : matrix) {
        lo = min(lo, row[0]);
        hi = max(hi, row[n-1]);
    }
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        int count = 0;
        for (auto& row : matrix)
            count += upper_bound(row.begin(), row.end(), mid) - row.begin();
        if (count < target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}
```

**Time:** O(m log n × log(max - min)) | **Space:** O(1) | GFG Hard | Amazon, Google

---

## 7. Problems — Binary Search on Answer Space

This is the section that separates candidates who "know binary search" from those who truly understand it. Every problem here reduces to: **binary search over the answer, check feasibility with a linear scan.**

### BS-Answer-1 — Koko Eating Bananas (LeetCode 875) ⭐⭐⭐

Koko can eat `k` bananas/hour. Must finish all piles in `h` hours. Find minimum integer `k`.

**What to binary search:** The eating speed `k`. Range: [1, max(piles)].

**feasible(k):** Can Koko finish all piles in ≤ h hours at speed k?
For each pile, hours needed = `ceil(pile / k)`. Sum all hours, check ≤ h.

```python
import math

def min_eating_speed(piles, h):
    lo, hi = 1, max(piles)

    def feasible(speed):
        return sum(math.ceil(pile / speed) for pile in piles) <= h

    while lo < hi:
        mid = lo + (hi - lo) // 2
        if feasible(mid):
            hi = mid        # This speed works, try slower (smaller)
        else:
            lo = mid + 1    # Too slow, try faster

    return lo

print(min_eating_speed([3,6,7,11], 8))    # 4
print(min_eating_speed([30,11,23,4,20], 5))  # 30
```

```cpp
int minEatingSpeed(vector<int>& piles, int h) {
    int lo = 1, hi = *max_element(piles.begin(), piles.end());
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        long long hours = 0;
        for (int p : piles) hours += (p + mid - 1) / mid;  // ceil(p/mid)
        if (hours <= h) hi = mid;
        else lo = mid + 1;
    }
    return lo;
}
```

**Time:** O(n log(max(piles))) | **Space:** O(1) | LeetCode 875 | Amazon, Google

---

### BS-Answer-2 — Capacity to Ship Packages Within D Days (LeetCode 1011) ⭐⭐⭐

Ship all packages in exactly d days. Find minimum weight capacity of the ship.

**What to binary search:** The ship's weight capacity. Range: [max(weights), sum(weights)].
- Lower bound: must carry at least the heaviest package.
- Upper bound: can always finish in 1 day by carrying everything.

**feasible(capacity):** Can we ship all packages in ≤ d days with this capacity?

```python
def ship_within_days(weights, days):
    lo, hi = max(weights), sum(weights)

    def feasible(capacity):
        day_count, current_load = 1, 0
        for w in weights:
            if current_load + w > capacity:
                day_count += 1          # Start a new day
                current_load = 0
            current_load += w
        return day_count <= days

    while lo < hi:
        mid = lo + (hi - lo) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    return lo

print(ship_within_days([1,2,3,4,5,6,7,8,9,10], 5))   # 15
print(ship_within_days([3,2,2,4,1,4], 3))             # 6
```

**Time:** O(n log(sum - max)) | **Space:** O(1) | LeetCode 1011 | Amazon top question

---

### BS-Answer-3 — Minimum Days to Make M Bouquets (LeetCode 1482)

Given `bloomDay[i]` (day when flower i blooms), make m bouquets each requiring k adjacent flowers. Find minimum days.

**What to binary search:** The number of days. Range: [min(bloomDay), max(bloomDay)].

**feasible(day):** With `day` days, how many bouquets can we make?
On or before `day`, a flower blooms if `bloomDay[i] <= day`. Count consecutive bloomed flowers, group into bouquets of size k.

```python
def min_days(bloomDay, m, k):
    if m * k > len(bloomDay):
        return -1   # Impossible

    def feasible(day):
        bouquets = consecutive = 0
        for b in bloomDay:
            if b <= day:
                consecutive += 1
                if consecutive == k:
                    bouquets += 1
                    consecutive = 0
            else:
                consecutive = 0
        return bouquets >= m

    lo, hi = min(bloomDay), max(bloomDay)
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    return lo

print(min_days([1,10,3,10,2], 3, 1))  # 3
print(min_days([1,10,3,10,2], 3, 2))  # -1
```

**Time:** O(n log(max - min)) | **Space:** O(1) | LeetCode 1482

---

### BS-Answer-4 — Find the Smallest Divisor Given a Threshold (LeetCode 1283)

Find the smallest divisor d such that the sum of `ceil(num / d)` for all nums ≤ threshold.

```python
import math

def smallest_divisor(nums, threshold):
    lo, hi = 1, max(nums)

    def feasible(d):
        return sum(math.ceil(n / d) for n in nums) <= threshold

    while lo < hi:
        mid = lo + (hi - lo) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    return lo
```

**This is structurally identical to Koko Eating Bananas** — same template, same feasibility check pattern. Recognizing this family is the key to solving new problems quickly.

---

### BS-Answer-5 — Aggressive Cows (GFG / SPOJ Classic) ⭐⭐⭐

Place C cows in N stalls at given positions. Maximize the minimum distance between any two cows.

**What to binary search:** The minimum distance. Range: [1, max(stalls) - min(stalls)].

**feasible(min_dist):** Can we place all C cows such that any two are at least `min_dist` apart?
Greedy: place first cow at leftmost stall, then each next cow at the earliest stall that is ≥ `min_dist` away from the last placed cow.

```python
def aggressive_cows(stalls, cows):
    stalls.sort()
    lo, hi = 1, stalls[-1] - stalls[0]

    def feasible(min_dist):
        placed = 1
        last_pos = stalls[0]
        for s in stalls[1:]:
            if s - last_pos >= min_dist:
                placed += 1
                last_pos = s
        return placed >= cows

    result = 0
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if feasible(mid):
            result = mid    # This distance works, try larger (maximize)
            lo = mid + 1
        else:
            hi = mid - 1

    return result

print(aggressive_cows([1,2,4,8,9], 3))  # 3
```

> **Note:** This is a **maximization** problem, so when `feasible(mid)` is True, record the answer and try **larger** (move lo right), unlike Koko where we minimize and move hi left.

**Time:** O(n log(max - min)) | GFG, SPOJ, commonly asked at Amazon, Flipkart

---

### BS-Answer-6 — Split Array Largest Sum (LeetCode 410) ⭐⭐

Split array into k non-empty subarrays. Minimize the largest sum among subarrays.

**What to binary search:** The maximum sum allowed per subarray. Range: [max(nums), sum(nums)].

**feasible(max_sum):** Can we split into ≤ k subarrays where each sum ≤ max_sum?

```python
def split_array(nums, k):
    lo, hi = max(nums), sum(nums)

    def feasible(max_sum):
        splits = 1
        current = 0
        for n in nums:
            if current + n > max_sum:
                splits += 1
                current = 0
            current += n
        return splits <= k

    while lo < hi:
        mid = lo + (hi - lo) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    return lo

print(split_array([7,2,5,10,8], 2))   # 18
print(split_array([1,2,3,4,5], 2))    # 9
```

**Time:** O(n log(sum - max)) | **Space:** O(1) | LeetCode 410 | Google, Facebook

---

## 8. Built-in Binary Search Functions

### Python — `bisect` module

```python
import bisect

arr = [1, 3, 3, 5, 7, 9]

# bisect_left = lower_bound = first index where arr[i] >= target
bisect.bisect_left(arr, 3)    # → 1 (first 3 is at index 1)
bisect.bisect_left(arr, 4)    # → 3 (where 4 would insert to keep sorted)

# bisect_right = upper_bound = first index where arr[i] > target
bisect.bisect_right(arr, 3)   # → 3 (after last 3)
bisect.bisect_right(arr, 4)   # → 3 (same as bisect_left for missing value)

# insort: insert element maintaining sorted order
bisect.insort(arr, 4)         # arr becomes [1, 3, 3, 4, 5, 7, 9]

# Count occurrences of x
x = 3
count = bisect.bisect_right(arr, x) - bisect.bisect_left(arr, x)  # → 2

# First index of x (or -1 if not found)
idx = bisect.bisect_left(arr, x)
first = idx if idx < len(arr) and arr[idx] == x else -1
```

### C++ — `<algorithm>` STL

```cpp
#include <algorithm>
vector<int> arr = {1, 3, 3, 5, 7, 9};

// lower_bound: first element >= target (iterator)
auto it = lower_bound(arr.begin(), arr.end(), 3);
int idx = it - arr.begin();   // Convert to index: 1

// upper_bound: first element > target
auto it2 = upper_bound(arr.begin(), arr.end(), 3);
int idx2 = it2 - arr.begin(); // 3

// Count occurrences
int count = upper_bound(arr.begin(), arr.end(), 3)
          - lower_bound(arr.begin(), arr.end(), 3);  // 2

// binary_search: returns bool (found or not)
bool found = binary_search(arr.begin(), arr.end(), 5);  // true

// Sorted insert: insert at correct position
arr.insert(lower_bound(arr.begin(), arr.end(), 4), 4);

// On custom sorted range with comparator
sort(people.begin(), people.end(), [](auto& a, auto& b){ return a.age < b.age; });
auto pos = lower_bound(people.begin(), people.end(), target_age,
                       [](const Person& p, int age){ return p.age < age; });
```

---

## 9. Common Mistakes & Gotchas

**1. Integer overflow in midpoint calculation**
`(lo + hi) / 2` can overflow if lo and hi are large ints (e.g., near INT_MAX in C++). Always use `lo + (hi - lo) / 2`.

```cpp
// WRONG (overflows for large lo, hi):
int mid = (lo + hi) / 2;

// CORRECT:
int mid = lo + (hi - lo) / 2;
```

Python's integers are arbitrary-precision so overflow isn't an issue, but the safe form is a good habit.

**2. Infinite loop from incorrect boundary update**

If you set `hi = mid` (not `mid - 1`) when using `lo <= hi`, you can get an infinite loop when `lo == hi == mid`:

```python
# DANGER: when lo == hi, mid = lo, and hi = mid doesn't shrink
while lo <= hi:
    mid = (lo + hi) // 2
    if condition: hi = mid    # Infinite loop if lo == hi!

# FIX: use hi = mid - 1 with the lo <= hi template
# OR use lo < hi template with hi = mid
```

**3. Off-by-one in loop condition**
`while lo <= hi` — terminates when `lo > hi`. Used for exact-target search.
`while lo < hi` — terminates when `lo == hi`. Used for lower/upper bound. After loop, answer is `lo` (or `hi`, they're equal).

**4. Wrong return value for lower/upper bound**
For "search insert position" (lower bound): return `lo` after the `lo < hi` loop.
For "last occurrence of target": return `upper_bound(target) - 1` and verify the element equals target.

**5. Rotated array with duplicates**
LC 33 (no duplicates): always O(log n).
LC 81 (with duplicates): worst case O(n) when `nums[lo] == nums[mid] == nums[hi]`. Must handle that case separately by incrementing lo and decrementing hi.

**6. Binary search on answer — wrong bounds**
For Koko Eating Bananas: lo = 1 (minimum speed), hi = max(piles). A common mistake is setting hi = sum(piles) — technically correct but unnecessary and slower.
For Capacity to Ship: lo = max(weights) (must carry heaviest), not 1. Setting lo = 1 works but wastes iterations.

**7. ceil division in Python vs C++**
`ceil(a / b)` in Python: use `(a + b - 1) // b` or `math.ceil(a / b)` or `-(-a // b)`.
In C++: `(a + b - 1) / b` (be careful not to overflow).

**8. Forgetting to handle empty array or single element**
Always check `if not nums: return -1` at the top for empty arrays. Single element arrays: most templates handle them correctly but verify your boundary logic on n=1.

---

## 10. Interview Q&A Bank

**Q1: Explain binary search and when it is applicable.**
Binary search is an algorithm that finds a target in a sorted (or monotone) structure in O(log n) time by repeatedly halving the search space. At each step, compare the middle element with the target — if equal, done; if target is larger, eliminate the left half; if smaller, eliminate the right half. It's applicable whenever: the data is sorted or has a monotone property, you can compute a "mid" quickly, and you can determine which half to discard based on the comparison.

**Q2: What is the difference between lower_bound and upper_bound?**
`lower_bound(target)` returns the **first position** where the element is ≥ target — effectively the first occurrence of target if it exists. `upper_bound(target)` returns the **first position** where the element is > target — one past the last occurrence. The count of target in a sorted array is `upper_bound - lower_bound`. The last occurrence is at `upper_bound - 1`. In C++, both are built into `<algorithm>`. In Python, `bisect_left` = lower_bound, `bisect_right` = upper_bound.

**Q3: Why does binary search work on a rotated sorted array?**
When you compute mid in a rotated array, one of the two halves (left or right) is always fully sorted. You can determine which by comparing `nums[lo]` with `nums[mid]`. Once you know the sorted half, you can check in O(1) whether the target falls within it. If yes, search there; otherwise search the other half. This discards half the elements per step, keeping time O(log n).

**Q4: What is "binary search on the answer space" and when do you use it?**
Instead of searching for a target in an array, you binary search over all possible answer values. Use it when: the problem asks for a minimum or maximum value satisfying a constraint, and you can write a `feasible(mid)` function that checks if `mid` is achievable. The key property: if `feasible(x)` is True, then `feasible(x+1)` is also True (monotone). Classic examples: Koko Eating Bananas (minimize eating speed), Capacity to Ship (minimize weight capacity), Aggressive Cows (maximize minimum distance).

**Q5: What are the two main templates for binary search and when do you use each?**
Template 1 (`while lo <= hi`): Use for exact target search. Loop exits when `lo > hi` meaning all elements exhausted. Returns -1 if not found. Both `lo = mid + 1` and `hi = mid - 1` on updates.
Template 2 (`while lo < hi`): Use for boundary search (first/last occurrence, lower/upper bound). Loop exits when `lo == hi`, which is the answer. On updates: `lo = mid + 1` and `hi = mid` (not `mid - 1`).

**Q6: How do you find the number of occurrences of a target in a sorted array in O(log n)?**
Run two binary searches: one lower bound search (first occurrence) and one upper bound search (first index greater than target). Count = upper_bound_index - lower_bound_index. Each search is O(log n), total O(log n). In C++: `upper_bound(v.begin(), v.end(), x) - lower_bound(v.begin(), v.end(), x)`.

**Q7: What is the time and space complexity of iterative vs recursive binary search?**
Both are O(log n) time. Iterative binary search is O(1) space. Recursive binary search is O(log n) space due to the call stack (recursion depth = log n). In interviews, prefer iterative unless the problem structure naturally maps to recursion, to avoid potential stack overflow and unnecessary overhead.

**Q8: How would you search in a matrix where each row is sorted and each column is sorted (LC 240)?**
Binary search doesn't directly apply because the matrix isn't globally sorted. Use the "staircase" technique: start at the top-right corner. If current > target, move left (smaller column values). If current < target, move down (larger row values). Each step eliminates either a full row or a full column. Time O(m + n), Space O(1).

**Q9: How would you find the median of two sorted arrays in O(log(min(m, n)))?**
Binary search over partitions of the smaller array. For each partition i of array A, there's a corresponding partition j = (m+n+1)/2 - i of array B. Check if the partition is valid: max of left side ≤ min of right side for both arrays. If not valid, adjust i. This is the hardest binary search problem commonly asked in interviews (LeetCode 4).

**Q10: What is the minimum guaranteed worst-case number of comparisons to find a target in a sorted array of 1000 elements?**
10 comparisons. log₂(1000) ≈ 9.97, so ceiling is 10. Binary search needs at most ⌈log₂(n)⌉ comparisons in the worst case. For comparison: linear search needs up to 1000. For n = 1,000,000: binary needs 20 comparisons.

**Q11: Walk me through the feasible() function for Capacity to Ship Packages.**
Given a candidate ship capacity, simulate loading packages one by one onto the ship. If adding the next package would exceed the capacity, start a new day (increment day counter, reset current load). Count total days needed. If days ≤ required days, this capacity is feasible. Binary search over capacity from max(weights) to sum(weights), finding the minimum feasible capacity.

**Q12: How does binary search apply to finding a peak element in an unsorted array?**
A peak is guaranteed to exist because the array is bounded (we can treat elements outside as -infinity). If `nums[mid] < nums[mid+1]`, the right half has an ascending slope — a peak must exist somewhere to the right. If `nums[mid] > nums[mid+1]`, mid itself or the left half contains a peak. Move `lo` or `hi` accordingly. This converges to a peak in O(log n) even though the array isn't sorted.

**Q13: When does binary search NOT work in a rotated sorted array?**
When duplicates are present (LC 81). With duplicates, if `nums[lo] == nums[mid] == nums[hi]`, you can't determine which half is sorted. The only safe move is to shrink the search space by one from each end: `lo += 1`, `hi -= 1`. This degrades worst-case to O(n) for arrays with all identical elements (e.g., [3,3,3,3,1,3,3]).

**Q14: What is the relationship between binary search and dynamic programming?**
Binary search often optimizes a greedy or DP solution. Patience sorting with binary search solves Longest Increasing Subsequence in O(n log n) instead of O(n²) DP. Binary search on the answer "min-max" type problems converts them from O(n²) brute force to O(n log n). Many DP recurrences with convex hull optimization also embed binary search. Generally: if you have a sorted/monotone structure in your DP state space, binary search can reduce a dimension.

**Q15: How would you handle binary search when the search space is not integers but real numbers?**
Use a fixed number of iterations (e.g., 100) instead of a convergence condition, or compare with an epsilon tolerance `abs(hi - lo) > 1e-9`. The template becomes:
```python
lo, hi = 0.0, 1e9
for _ in range(100):
    mid = (lo + hi) / 2
    if feasible(mid):
        hi = mid
    else:
        lo = mid
return lo
```
Example: Find a number x such that x³ + x² = some_value (cubic root finding).

---

**Answer Framework — "Implement binary search on X":**
State the search space bounds (lo, hi) → Define feasible(mid) → State the monotone property → Write the loop → Handle boundary updates correctly → Verify with edge cases (empty, single element, target at ends)

**Depth Gauge:**
- **Junior (SDE-1):** Classic binary search, search insert position, first/last occurrence, search in rotated array, find minimum in rotated array. Must implement without bugs.
- **Mid-level (SDE-2):** Peak element, single element in sorted array, Koko bananas, capacity to ship, aggressive cows. Recognize the "binary search on answer" pattern independently.
- **Senior (SDE-3+):** Median of two sorted arrays, split array largest sum, row-wise matrix median, design problems using binary search. Explain the monotone property proof for novel problems.

---

## 11. Problem Master List

**1D Array — Classic & Boundaries:**

| # | Problem | Pattern | Key Insight |
|---|---|---|---|
| LC 704 | Binary Search | Classic | Template 1 |
| LC 35 | Search Insert Position | Lower Bound | Equivalent to `bisect_left` |
| LC 278 | First Bad Version | Lower Bound | First True in monotone |
| LC 69 | Sqrt(x) | Classic | Return `hi` for floor |
| LC 34 | Find First and Last Position | Lower+Upper Bound | Two separate binary searches |
| LC 162 | Find Peak Element | Modified BS | Follow ascending slope |
| LC 540 | Single Element in Sorted Array | Modified BS | Keep even-indexed mid |
| LC 33 | Search in Rotated Sorted Array | Rotated BS | One half always sorted |
| LC 153 | Find Minimum in Rotated Array | Rotated BS | Compare mid with hi |
| LC 81 | Search Rotated Array II (dups) | Rotated BS | Handle lo==mid==hi case |
| LC 4 | Median of Two Sorted Arrays | Binary on partition | Partition search |

**2D Matrix:**

| # | Problem | Pattern | Key Insight |
|---|---|---|---|
| LC 74 | Search a 2D Matrix | 1D index mapping | mid/n row, mid%n col |
| LC 240 | Search a 2D Matrix II | Staircase walk | Start top-right |
| GFG | Median in Row-Sorted Matrix | BS on answer | Count elements ≤ mid per row |

**Binary Search on Answer Space:**

| # | Problem | Binary Search Over | feasible() |
|---|---|---|---|
| LC 875 | Koko Eating Bananas | Speed [1, max] | Total hours ≤ h |
| LC 1011 | Capacity to Ship Packages | Capacity [max, sum] | Days needed ≤ d |
| LC 1482 | Min Days for M Bouquets | Days [min, max] | Count bouquets ≥ m |
| LC 1283 | Smallest Divisor | Divisor [1, max] | Sum of ceils ≤ threshold |
| LC 410 | Split Array Largest Sum | Max sum [max, sum] | Splits needed ≤ k |
| GFG | Aggressive Cows | Min dist [1, range] | Can place all cows |
| LC 774 | Minimize Max Distance to Gas | Distance | Count stations needed |
| LC 1552 | Magnetic Force Between Balls | Min dist | Can place all balls |

---

## 12. Source Notes

| Source | Contribution |
|---|---|
| Striver A2Z Playlist | Problem sequence: 1D BS → BS on Answers → 2D Matrix |
| NeetCode | Median of two sorted arrays walkthrough, rotated array visual |
| AlgoMaster | Lower bound / upper bound explanation, Find First/Last detail |
| LeetCode Discussions | Binary search pattern taxonomy (80% of problems), template comparison |
| interviewing.io | Edge case testing strategy (boundaries, duplicates, negatives) |
| HelloInterview | Rotated array visual — "one half always sorted" insight |
| AlgoMonster | Row-wise matrix median solution, answer-space template |
| GFG | Aggressive cows, median in matrix, classical problems |

---

## 13. Glossary

**Binary Search:** Algorithm that finds a target in a sorted sequence in O(log n) by halving the search space each step.

**Lower Bound:** First index where value ≥ target. Python: `bisect_left`. C++: `lower_bound`.

**Upper Bound:** First index where value > target. Python: `bisect_right`. C++: `upper_bound`.

**Monotone Property:** A sequence (of boolean values) where all False values come before all True values (or vice versa). Enables binary search even when the array itself isn't numeric.

**Search Space:** The range of candidate values being binary searched over. In "BS on Answer," this is the range of possible answer values, not array indices.

**feasible(x):** A function that checks whether answer value x satisfies the problem's constraint. Must be monotone: if feasible(x) then feasible(x+k) for all k > 0 (or k < 0, depending on the problem).

**Rotated Sorted Array:** A sorted array that has been cyclically shifted (rotated) at an unknown pivot point.

**Peak Element:** An element greater than both its neighbors. The boundary elements need only be greater than their one neighbour.

**Pivot (Rotation):** The index where the rotation happened — the minimum element in a rotated sorted array.

**Overflow-safe Midpoint:** `lo + (hi - lo) / 2` — avoids integer overflow when lo + hi would exceed INT_MAX.

**ceil(a/b) — integer division:** `(a + b - 1) // b` in Python and C++. Rounds up instead of down.

---

*Next recommended: **Strings** (Step 5) or **Linked Lists** (Step 6) — both build directly on the O(log n) thinking developed here.*
