

# Pattern 1: Two Pointers

## 1. What This Pattern Solves

Use **Two Pointers** when:

* You are working with **arrays / strings**
* You need to find **pairs**, **subarrays**, or **compare elements**
* The data is **sorted** (often, but not always)

Typical problems:

* Two Sum (sorted version)
* Reverse a string
* Check palindrome
* Remove duplicates
* Pair with target sum

---

## 2. Core Idea

Instead of using nested loops (O(n²)),
you use **two indices** that move toward each other.

```
left  →        ←  right
```

This reduces time complexity to **O(n)**.

---

## 3. Mental Model

You place:

* One pointer at the **start**
* One pointer at the **end**

Then:

* Compare values
* Move pointers based on the condition

---

## 4. Example: Check Palindrome

Problem:
Check if `"racecar"` is a palindrome.

### Step-by-step logic:

1. `left = 0`, `right = len(s)-1`
2. Compare `s[left]` and `s[right]`
3. If equal → move inward
4. If not → return False

### Code:

```python
def isPalindrome(s):
    left, right = 0, len(s) - 1
    
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    
    return True
```

---

## 5. Example: Two Sum (Sorted Array)

Input:

```
nums = [1, 2, 4, 6, 10]
target = 8
```

Steps:

| Left | Right | Sum | Action                |
| ---- | ----- | --- | --------------------- |
| 1    | 10    | 11  | Too big → move right  |
| 1    | 6     | 7   | Too small → move left |
| 2    | 6     | 8   | Found                 |

### Code:

```python
def twoSumSorted(nums, target):
    left, right = 0, len(nums) - 1
    
    while left < right:
        s = nums[left] + nums[right]
        
        if s == target:
            return [left, right]
        elif s < target:
            left += 1
        else:
            right -= 1
```

---

## 6. When NOT to Use Two Pointers

* When the data is **unsorted** and order matters
* When you need **all combinations**
* When the problem is about **subarrays with variable length**
  (That’s Sliding Window)

---

# Pattern 2: Sliding Window

## 1. What This Pattern Solves

Use **Sliding Window** when:

* You need a **subarray / substring**
* The window is **contiguous**
* You are optimizing over a range

Typical problems:

* Max sum subarray of size K
* Longest substring without repeating characters
* Minimum window substring
* Fixed-size or variable-size window problems

---

## 2. Core Idea

Instead of recalculating for every subarray,
you **slide** a window across the array.

```
[ 2  1  5 ]  1  3
  [ 1  5  1 ]
     [ 5  1  3 ]
```

---

## 3. Two Types of Sliding Window

### A. Fixed Size Window

Example: Max sum of subarray of size `k`

### B. Variable Size Window

Example: Longest substring without repeating characters

---

## 4. Fixed Window Example

Problem:
Find max sum of subarray of size `k = 3`

Input:

```
nums = [2, 1, 5, 1, 3, 2]
```

### Step-by-step:

1. Start with first 3 elements
2. Slide window right
3. Add next element
4. Remove left element

### Code:

```python
def maxSumSubarray(nums, k):
    window_sum = sum(nums[:k])
    max_sum = window_sum
    
    for i in range(k, len(nums)):
        window_sum += nums[i]
        window_sum -= nums[i - k]
        max_sum = max(max_sum, window_sum)
    
    return max_sum
```

Time: **O(n)**
Space: **O(1)**

---

Certainly. Below is a **clean, structured, and intuitive explanation** of the **Variable Size Sliding Window** pattern, designed to eliminate confusion and build a correct mental model.

---

# Variable Size Sliding Window — Simplified Explanation

## 1. What “Variable Window” Actually Means

A **window** is a contiguous range in an array or string:

```
[left ... right]
```

In a **variable size window**, the window:

* **Expands** when the condition is valid
* **Shrinks** when the condition becomes invalid

Unlike fixed windows, the size is **not constant**.

---

## 2. When This Pattern Is Used

Use variable sliding window when:

* You need the **longest / shortest** subarray or substring
* There is a **condition to maintain**
* The window size is **not fixed**

Common problems:

* Longest substring without repeating characters
* Smallest subarray with sum ≥ K
* Longest subarray with at most K distinct elements

---

## 3. Core Logic (Mental Model)

You always maintain:

```
[left ... right]
```

### Step 1: Expand the window

Move `right` forward to include more elements.

### Step 2: Check the condition

If the condition is violated → shrink from the left.

### Step 3: Shrink the window

Move `left` forward until the condition is valid again.

### Step 4: Record the answer

Update the best window size.

---

## 4. Visual Example (Characters)

Problem:
**Longest substring without repeating characters**

Input:

```
"abcabcbb"
```

We want the **longest window** with **unique characters**.

---

### Step-by-Step Walkthrough

| Window | Characters | Valid? | Action |
| ------ | ---------- | ------ | ------ |
| "a"    | {a}        | Yes    | Expand |
| "ab"   | {a,b}      | Yes    | Expand |
| "abc"  | {a,b,c}    | Yes    | Expand |
| "abca" | {a,b,c,a}  | No     | Shrink |
| "bca"  | {b,c,a}    | Yes    | Expand |
| "bcab" | {b,c,a,b}  | No     | Shrink |

The **window grows and shrinks dynamically**.

---

## 5. Code Explained Line by Line

```python
def longestUniqueSubstring(s):
    char_set = set()
    left = 0
    max_len = 0
    
    for right in range(len(s)):
```

* `right` expands the window
* `left` will shrink it
* `char_set` tracks characters inside the window

---

```python
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
```

This is the **shrink phase**.

If a duplicate appears:

* Remove from the left
* Move `left` forward
* Repeat until valid

---

```python
        char_set.add(s[right])
        max_len = max(max_len, right - left + 1)
```

This is the **expand + record phase**.

---

## 6. Key Insight (Most Important)

You are **not searching for all subarrays**.
You are **maintaining a valid window**.

The window:

* Expands when valid
* Shrinks when invalid

This guarantees **O(n)** time.

---

## 7. Another Simple Example (Numbers)

Problem:
Smallest subarray with sum ≥ 7

Input:

```
nums = [2, 1, 5, 2, 3, 2]
```

---

### Window Movement

| Window  | Sum | Valid? | Action |
| ------- | --- | ------ | ------ |
| [2]     | 2   | No     | Expand |
| [2,1]   | 3   | No     | Expand |
| [2,1,5] | 8   | Yes    | Shrink |
| [1,5]   | 6   | No     | Expand |
| [1,5,2] | 8   | Yes    | Shrink |

Window size **changes dynamically**.

---

## 8. General Template

```python
left = 0

for right in range(len(data)):
    add(data[right])
    
    while condition_is_invalid:
        remove(data[left])
        left += 1
    
    update_answer()
```

This template applies to **all variable window problems**.

---

## 9. Fixed vs Variable (Final Clarity)

| Feature   | Fixed Window      | Variable Window           |
| --------- | ----------------- | ------------------------- |
| Size      | Constant          | Changes                   |
| Condition | Always valid      | Must be maintained        |
| Use case  | Max sum of size K | Longest/shortest subarray |
| Movement  | Slide             | Expand + Shrink           |

---

## 10. Why This Pattern Is Powerful

* Eliminates O(n²) brute force
* Maintains constraints efficiently
* Works for many real interview problems

---

## 6. Key Differences: Two Pointers vs Sliding Window

| Feature  | Two Pointers               | Sliding Window            |
| -------- | -------------------------- | ------------------------- |
| Purpose  | Pair / compare             | Subarray / substring      |
| Window   | Not fixed                  | Contiguous window         |
| Movement | Inward                     | Left & Right slide        |
| Use case | Sorted arrays, palindromes | Max/min/longest subarrays |

---

## 7. What You Should Do Now

Before moving forward, make sure you can:

* Explain **why** Two Pointers is O(n)
* Identify **Sliding Window** problems
* Dry-run both patterns
* Write the logic without memorizing code
* 


Excellent. We will now proceed with **Pattern 3** and **Pattern 4**, maintaining the same structured, interview-oriented approach.

At the end, I will also give you a **concise overview** of how many major patterns exist.

---

# Pattern 3: Fast & Slow Pointers (Tortoise–Hare)

## 1. What This Pattern Solves

Use **Fast & Slow Pointers** when:

* You are working with **linked lists**
* You need to detect **cycles**
* You need to find the **middle**
* You need to identify **loop entry points**

Typical problems:

* Detect cycle in linked list
* Find middle of linked list
* Happy Number
* Find duplicate number

---

## 2. Core Idea

Use **two pointers**:

* `slow` → moves 1 step
* `fast` → moves 2 steps

If there is a **cycle**, they will eventually meet.

---

## 3. Visual Intuition

```
slow →  o   o   o   o
fast →      o       o
```

Fast pointer "laps" slow pointer in a cycle.

---

## 4. Example: Detect Cycle in Linked List

### Code:

```python
def hasCycle(head):
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            return True
    
    return False
```

---

## 5. Why It Works

* Fast pointer closes the gap
* If no cycle → fast hits `None`
* If cycle → pointers meet

Time: **O(n)**
Space: **O(1)**

---

## 6. Find Middle of Linked List

```python
def findMiddle(head):
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow
```

Slow pointer ends at the middle.

---

## 7. When NOT to Use It

* Arrays or strings
* When you need frequency counts
* When order-based windows are required

---

# Pattern 4: Hashing / Frequency Map

## 1. What This Pattern Solves

Use **Hashing** when:

* You need **fast lookups**
* You need **counts**
* You need to detect **duplicates**
* You need to map values to indices

Typical problems:

* Two Sum
* Anagrams
* Subarray Sum
* Top K Frequent
* Group Anagrams

---

## 2. Core Idea

Store information in:

```
key → value
```

So lookups are **O(1)**.

---

## 3. Example: Two Sum

```python
def twoSum(nums, target):
    seen = {}
    
    for i, num in enumerate(nums):
        if target - num in seen:
            return [seen[target - num], i]
        seen[num] = i
```

---

## 4. Example: Frequency Count

```python
from collections import Counter

words = ["a", "b", "a", "c"]
freq = Counter(words)
```

Result:

```
{'a': 2, 'b': 1, 'c': 1}
```

---

## 5. Example: Anagram Check

```python
def isAnagram(s, t):
    return Counter(s) == Counter(t)
```

---

## 6. Why Hashing Is Powerful

* Eliminates nested loops
* Converts problems to **lookup-based**
* Improves time from O(n²) → O(n)

---

## 7. Pattern 3 vs Pattern 4

| Feature  | Fast–Slow             | Hashing        |
| -------- | --------------------- | -------------- |
| Data     | Linked List           | Any            |
| Goal     | Detect cycle / middle | Lookup / count |
| Space    | O(1)                  | O(n)           |
| Use case | Loops                 | Frequencies    |

---

# How Many DSA Patterns Are There?

Most interview problems can be classified into **15 core patterns**.

Here is the **standard list**:

1. Two Pointers
2. Sliding Window
3. Fast & Slow Pointers
4. Hashing / Frequency Map
5. Prefix Sum
6. Binary Search
7. Merge Intervals
8. Heap / Priority Queue
9. Monotonic Stack
10. Backtracking
11. Greedy
12. Recursion
13. Dynamic Programming
14. Graph (BFS/DFS)
15. Bit Manipulation

Mastering these covers **90%+** of interview questions.

---



