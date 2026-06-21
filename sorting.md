# Sorting Algorithms — Interview Prep Book
### Python & C++ | Striver A2Z Step 2 | 2026 Edition

> **Sources:** Striver A2Z Playlist, GeeksforGeeks, interviewing.io, AlgoCademy, InterviewCake, SourceCodeSolutions, Devinterview.io  
> **Depth:** Interview (Deep) | **Languages:** Python + C++  
> **Split:** ~25% Theory | ~75% Problems + Solutions  
> **Generated:** June 2026

---

## Table of Contents

1. [Quick Reference Card](#1-quick-reference-card)
2. [Why Sorting Matters for Interviews](#2-why-sorting-matters-for-interviews)
3. [The Algorithms — Theory + Code](#3-the-algorithms--theory--code)
   - 3.1 Bubble Sort
   - 3.2 Selection Sort
   - 3.3 Insertion Sort
   - 3.4 Merge Sort ⭐ (Must Master)
   - 3.5 Quick Sort ⭐ (Must Master)
   - 3.6 Counting Sort / Radix Sort (Bonus)
4. [Built-in Sort: When to Just Use It](#4-built-in-sort-when-to-just-use-it)
5. [Problems with Full Solutions](#5-problems-with-full-solutions)
6. [Common Mistakes & Gotchas](#6-common-mistakes--gotchas)
7. [Interview Q&A Bank](#7-interview-qa-bank)
8. [Source Notes](#8-source-notes)
9. [Glossary](#9-glossary)

---

## 1. Quick Reference Card

| Algorithm | Best | Average | Worst | Space | Stable | In-Place |
|---|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ | ✅ |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ | ✅ |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ | ✅ |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ | ❌ |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ | ✅ |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | ❌ | ✅ |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) | ✅ | ❌ |
| Tim Sort (Python) | O(n) | O(n log n) | O(n log n) | O(n) | ✅ | ❌ |

**When to use which (interview decision framework):**
- **General purpose, performance critical** → Quick Sort (avg O(n log n), cache-friendly, in-place)
- **Stability required** → Merge Sort (guaranteed O(n log n), always stable)
- **Linked list** → Merge Sort (no random access needed)
- **Nearly sorted data** → Insertion Sort (approaches O(n))
- **O(1) extra space + O(n log n) worst case guaranteed** → Heap Sort
- **Small integer range** → Counting Sort (O(n+k))
- **Just sort it, interview language** → `sorted()` / `sort()` in Python, `std::sort` in C++

**The only two you MUST implement from scratch in interviews:** Merge Sort and Quick Sort.

---

## 2. Why Sorting Matters for Interviews

Sorting is the prerequisite for a huge fraction of interview problems. Binary search requires a sorted array. Two-pointer problems often sort first. Many greedy algorithms depend on sorted order. Sorting interview questions test three things at once: recursion, divide-and-conquer thinking, and time-space trade-off reasoning.

The practical reality is that you'll rarely implement a sort from scratch on the job. But interviewers use it as a lens — "implement merge sort" is really asking: can you write a clean recursive function, reason about base cases, think about index boundaries, and analyze complexity? As one source puts it, merge sort and quicksort are "foundational pillars when delving into advanced data structures and computational strategies, like trees and heaps."

Understanding sorting deeply also unlocks the ability to analyze *any* algorithm. Once you can reason about why merge sort is O(n log n) via the recursion tree method, you can apply that same thinking to tree traversals, divide-and-conquer DP, and segment trees later.

---

## 3. The Algorithms — Theory + Code

### 3.1 Bubble Sort

**Idea:** Repeatedly compare adjacent elements and swap them if they are in the wrong order. The largest unsorted element "bubbles up" to its correct position after each pass. After k passes, the k largest elements are in their final positions.

**Analogy:** Imagine sorting a row of people by height. You walk down the row repeatedly, and whenever two adjacent people are in the wrong order, you swap them. After each full walk, the tallest remaining unsorted person has moved to the right end.

**When used:** Almost never in production. Included in every course because it is the easiest to understand and forms the conceptual baseline.

**Optimization:** After any pass where no swaps happen, the array is already sorted. Use a flag to detect this and break early — this gives O(n) best case for already-sorted arrays.

**Python:**
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n - 1):
        swapped = False
        for j in range(n - 1 - i):   # Last i elements are already sorted
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:               # No swap → already sorted
            break
    return arr

print(bubble_sort([5, 1, 4, 2, 8]))  # [1, 2, 4, 5, 8]
```

**C++:**
```cpp
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if (!swapped) break;
    }
}
```

**Trace for [5, 1, 4, 2, 8]:**
- Pass 1: [1, 4, 2, 5, 8] — 8 bubbles to end
- Pass 2: [1, 2, 4, 5, 8] — 5 in place
- Pass 3: No swaps → done

**Time:** O(n²) worst/avg | O(n) best (with flag) | **Space:** O(1) | **Stable:** Yes

---

### 3.2 Selection Sort

**Idea:** Divide the array into a sorted (left) and unsorted (right) portion. Find the minimum element in the unsorted portion and place it at the beginning of the unsorted portion. Repeat until sorted.

**Analogy:** You have a hand of cards. You scan all cards to find the smallest, pull it out and place it at position 0. Then scan the remaining cards for the next smallest and place at position 1, and so on.

**Key difference from Bubble Sort:** Selection sort makes at most n-1 swaps (one per pass), whereas bubble sort can make O(n²) swaps. If swaps are expensive (e.g., writing to flash memory), selection sort can be better despite the same time complexity.

**Python:**
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n - 1):
        min_idx = i
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        if min_idx != i:
            arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr
```

**C++:**
```cpp
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx])
                minIdx = j;
        }
        if (minIdx != i)
            swap(arr[i], arr[minIdx]);
    }
}
```

**Time:** O(n²) all cases | **Space:** O(1) | **Stable:** ❌ (swapping can disturb relative order of equal elements)

---

### 3.3 Insertion Sort

**Idea:** Build the sorted array one element at a time. Take the next element from the unsorted region and insert it into its correct position in the sorted region by shifting larger elements to the right.

**Analogy:** Sorting a hand of playing cards. You pick up cards one by one. For each new card, you find its correct position among the already-held cards and slide it in.

**Key insight:** Insertion sort is **adaptive** — it approaches O(n) on nearly-sorted arrays. This is why it's used as a component in Tim Sort (Python's and Java's built-in sort) for small sub-arrays.

**Python:**
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        # Shift elements greater than key one position to the right
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key   # Insert key in correct position
    return arr
```

**C++:**
```cpp
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

**Trace for [4, 3, 2, 1]:**
- i=1: key=3 → [3, 4, 2, 1]
- i=2: key=2 → [2, 3, 4, 1]
- i=3: key=1 → [1, 2, 3, 4]

**Time:** O(n²) worst/avg | O(n) best | **Space:** O(1) | **Stable:** Yes

> **Interview Insight:** Insertion sort is the only O(n²) algorithm you'd actually use in practice — for small arrays (n < 32) or nearly-sorted data. Python's `timsort` uses insertion sort for subarrays of size ≤ 64.

---

### 3.4 Merge Sort ⭐

This is mandatory. Know it cold. Merge sort is a textbook divide-and-conquer algorithm.

**Idea:**
1. **Divide:** Split the array into two halves at the midpoint.
2. **Conquer:** Recursively sort each half.
3. **Combine:** Merge the two sorted halves into one sorted array.

**Base case:** An array of 0 or 1 elements is already sorted.

**Analogy:** You have a stack of unsorted exam papers. Split the stack into two halves, give each half to a friend to sort. Once both friends return sorted stacks, merge the two sorted stacks into one by repeatedly picking the smaller top paper.

**Why is it always O(n log n)?**
- The recursion tree has **log₂(n) levels** (each level halves the problem size).
- At each level, the merge step does **O(n) total work** across all subproblems.
- Total: O(n) × O(log n) = O(n log n) — in **all** cases (best, avg, worst).

**Python (clean, readable version):**
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:   # <= preserves stability
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result

print(merge_sort([5, 3, 8, 1, 9, 2]))  # [1, 2, 3, 5, 8, 9]
```

**C++ (in-place index version — preferred in interviews):**
```cpp
void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp;
    int i = left, j = mid + 1;

    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j])
            temp.push_back(arr[i++]);
        else
            temp.push_back(arr[j++]);
    }
    while (i <= mid)  temp.push_back(arr[i++]);
    while (j <= right) temp.push_back(arr[j++]);

    for (int k = left; k <= right; k++)
        arr[k] = temp[k - left];
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) return;          // Base case
    int mid = left + (right - left) / 2; // Avoids overflow vs (left+right)/2
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}

// Call: mergeSort(arr, 0, arr.size() - 1);
```

> **Why `left + (right - left) / 2` and not `(left + right) / 2`?**
> If left and right are both large ints (e.g. ~2 billion), `left + right` overflows a 32-bit int. This is a classic interview gotcha that separates prepared candidates.

**Time:** O(n log n) all cases | **Space:** O(n) for the temp array | **Stable:** Yes

**Key Properties:**
- Best sorting algorithm for **linked lists** — no random access needed.
- The **only comparison sort that is stable + O(n log n) worst case**.
- Good for **external sorting** (data too large for RAM) because it has sequential access patterns.

---

### 3.5 Quick Sort ⭐

The most important sorting algorithm to understand deeply. It is the basis of most real-world sorting library implementations (C++ `std::sort` uses introsort, which is quicksort + heapsort).

**Idea:**
1. **Choose a pivot** element.
2. **Partition:** Rearrange the array so elements less than the pivot are on its left, elements greater are on its right, and the pivot is in its final sorted position.
3. **Recurse** on the left and right sub-arrays (not including the pivot, since it's already placed).

**Base case:** Array of 0 or 1 elements.

**Analogy:** You're organizing books on a shelf. Pick one book (the pivot). Move all shorter books to its left, all taller books to its right. Now the pivot book is perfectly placed. Recursively organize the left and right groups.

**The Partition (Lomuto scheme — cleaner for interviews):**

```python
def partition(arr, low, high):
    pivot = arr[high]   # Last element as pivot
    i = low - 1         # i tracks the boundary of "less than pivot" region

    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]

    arr[i + 1], arr[high] = arr[high], arr[i + 1]  # Place pivot
    return i + 1   # Pivot's final index

def quick_sort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)

# Usage
arr = [10, 7, 8, 9, 1, 5]
quick_sort(arr, 0, len(arr) - 1)
print(arr)  # [1, 5, 7, 8, 9, 10]
```

**C++:**
```cpp
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

**Trace of partition on [3, 6, 8, 10, 1, 2, 1], pivot=1 (last):**
- pivot = 1, i = -1
- j=0: arr[0]=3 > 1, no swap
- j=1: arr[1]=6 > 1, no swap
- ... j=5: arr[5]=2 > 1, no swap
- Place pivot: arr[0] ↔ arr[6] → [1, 6, 8, 10, 1, 2, 3], pi=0

**Why can Quick Sort be O(n²)?**
If the pivot is always the largest or smallest element (e.g., already-sorted array with last-element pivot), every partition produces one empty sub-array and one of size n-1. The recursion depth becomes n, giving O(n²) total work.

**Fix: Randomized pivot.**

```python
import random

def partition_random(arr, low, high):
    rand_idx = random.randint(low, high)
    arr[rand_idx], arr[high] = arr[high], arr[rand_idx]  # Swap random to end
    return partition(arr, low, high)

def quick_sort_random(arr, low, high):
    if low < high:
        pi = partition_random(arr, low, high)
        quick_sort_random(arr, low, pi - 1)
        quick_sort_random(arr, pi + 1, high)
```

With random pivot, the expected time is O(n log n) and the worst case becomes astronomically unlikely.

**Time:** O(n log n) avg | O(n²) worst | **Space:** O(log n) recursion stack avg | **Stable:** No

---

### 3.6 Counting Sort / Radix Sort (Bonus — Know Conceptually)

**Counting Sort** — works in O(n+k) where k is the range of values. Not comparison-based; counts occurrences of each value, then reconstructs the sorted array from the counts.

```python
def counting_sort(arr, max_val):
    count = [0] * (max_val + 1)
    for x in arr:
        count[x] += 1
    result = []
    for val, cnt in enumerate(count):
        result.extend([val] * cnt)
    return result

print(counting_sort([4, 2, 2, 8, 3, 3, 1], 8))
# [1, 2, 2, 3, 3, 4, 8]
```

**When it beats comparison sorts:** When k (range) is O(n). Sorting 1 million integers in range [0, 1000] — counting sort is O(n), comparison sort is O(n log n). If range k >> n (e.g., sorting 10 numbers from range [0, 10⁹]), counting sort is wasteful.

**Radix Sort** sorts integers digit-by-digit (LSD to MSD) using a stable counting sort at each digit. Time O(nk) where k is the number of digits. Useful for fixed-width integers or strings; rarely asked in interviews but good to explain.

---

## 4. Built-in Sort: When to Just Use It

In real coding interviews, unless asked to implement a sort, just use the built-in. Know it well.

**Python:**
```python
# Sort in-place
arr = [3, 1, 4, 1, 5]
arr.sort()                              # Ascending in-place
arr.sort(reverse=True)                 # Descending in-place

# Return new sorted list
sorted_arr = sorted(arr)

# Custom key
words = ["banana", "apple", "cherry"]
words.sort(key=len)                    # Sort by length
words.sort(key=lambda w: w[-1])        # Sort by last character

# Sort list of tuples/objects
people = [("Alice", 30), ("Bob", 25), ("Carol", 35)]
people.sort(key=lambda x: x[1])       # Sort by age
people.sort(key=lambda x: (-x[1], x[0]))  # Desc age, then asc name

# Python's sort is Tim Sort — O(n log n) worst, O(n) best, stable
```

**C++:**
```cpp
#include <algorithm>
vector<int> arr = {3, 1, 4, 1, 5};

sort(arr.begin(), arr.end());                         // Ascending
sort(arr.begin(), arr.end(), greater<int>());         // Descending

// Custom comparator
vector<pair<int,string>> people = {{30,"Alice"},{25,"Bob"}};
sort(people.begin(), people.end(), [](auto& a, auto& b){
    return a.first < b.first;  // Sort by age ascending
});

// Partial sort — only sort first k elements
partial_sort(arr.begin(), arr.begin() + k, arr.end());

// nth_element — puts the nth smallest element at position n in O(n)
nth_element(arr.begin(), arr.begin() + k, arr.end());
// arr[k] is now the (k+1)th smallest; left of k is ≤ arr[k]

// stable_sort — like sort but preserves order of equal elements
stable_sort(arr.begin(), arr.end());

// C++ std::sort uses introsort: quicksort + heapsort fallback
// Guaranteed O(n log n) worst case
```

> **Interview Tip:** When you say `sort()` in a Python interview, mention "Python's built-in sort is Timsort, O(n log n) worst case, stable." When you say `std::sort` in C++, mention "it's introsort — quicksort with heapsort fallback, O(n log n) worst case, unstable."

---

## 5. Problems with Full Solutions

### Problem 5.1 — Sort Colors / Dutch National Flag (LeetCode 75)

Given array with only 0s, 1s, and 2s, sort in-place in O(n) time and O(1) space.

**Key insight:** Three-pointer approach (Dutch National Flag algorithm by Dijkstra). Maintain three regions: `[0..low-1]` = all 0s, `[low..mid-1]` = all 1s, `[mid..high]` = unknown, `[high+1..n-1]` = all 2s.

```python
def sort_colors(nums):
    low, mid, high = 0, 0, len(nums) - 1

    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:  # nums[mid] == 2
            nums[mid], nums[high] = nums[high], nums[mid]
            high -= 1
            # Don't increment mid — newly swapped element not yet checked

print(sort_colors_test := [2, 0, 2, 1, 1, 0])
sort_colors(sort_colors_test)
print(sort_colors_test)  # [0, 0, 1, 1, 2, 2]
```

```cpp
void sortColors(vector<int>& nums) {
    int low = 0, mid = 0, high = nums.size() - 1;
    while (mid <= high) {
        if (nums[mid] == 0)
            swap(nums[low++], nums[mid++]);
        else if (nums[mid] == 1)
            mid++;
        else
            swap(nums[mid], nums[high--]);
    }
}
```

**Time:** O(n) single pass | **Space:** O(1) | LeetCode 75 | Asked at: Google, Amazon, Adobe

---

### Problem 5.2 — Merge Intervals (LeetCode 56)

Given a list of intervals, merge all overlapping ones.

**Key insight:** Sort by start time. Then linearly scan — if current interval overlaps with the last merged one, extend the last merged one's end. Otherwise add a new merged interval.

```python
def merge_intervals(intervals):
    if not intervals:
        return []
    intervals.sort(key=lambda x: x[0])  # Sort by start
    merged = [intervals[0]]

    for start, end in intervals[1:]:
        last_end = merged[-1][1]
        if start <= last_end:            # Overlaps
            merged[-1][1] = max(last_end, end)  # Extend
        else:
            merged.append([start, end])  # No overlap, new interval

    return merged

print(merge_intervals([[1,3],[2,6],[8,10],[15,18]]))
# [[1,6],[8,10],[15,18]]
```

```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end()); // Sorts by first element
    vector<vector<int>> merged;
    merged.push_back(intervals[0]);

    for (int i = 1; i < intervals.size(); i++) {
        if (intervals[i][0] <= merged.back()[1])
            merged.back()[1] = max(merged.back()[1], intervals[i][1]);
        else
            merged.push_back(intervals[i]);
    }
    return merged;
}
```

**Time:** O(n log n) for sort + O(n) scan | **Space:** O(n) | LeetCode 56 | Asked at: Facebook, Amazon, Google

---

### Problem 5.3 — Count Inversions in Array (Classic + Merge Sort Application)

Count pairs (i, j) where i < j but arr[i] > arr[j]. A classic interview problem that demonstrates why merge sort is important beyond just sorting.

**Key insight:** During the merge step of merge sort, whenever we pick an element from the right half (because it's smaller than the current left half element), all remaining elements in the left half form inversions with it. This lets us count inversions in O(n log n).

```python
def count_inversions(arr):
    if len(arr) <= 1:
        return arr, 0

    mid = len(arr) // 2
    left, left_inv = count_inversions(arr[:mid])
    right, right_inv = count_inversions(arr[mid:])

    merged = []
    inv = left_inv + right_inv
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            merged.append(left[i])
            i += 1
        else:
            merged.append(right[j])
            inv += len(left) - i  # All remaining in left > right[j]
            j += 1

    merged.extend(left[i:])
    merged.extend(right[j:])
    return merged, inv

arr = [2, 4, 1, 3, 5]
_, count = count_inversions(arr)
print(count)  # 3 — pairs (2,1), (4,1), (4,3)
```

**Time:** O(n log n) | **Space:** O(n) | Asked at: Google, Microsoft, Amazon

---

### Problem 5.4 — Kth Largest Element (LeetCode 215)

Find the kth largest element in an unsorted array.

**Approach 1 — Sort:** O(n log n), simplest.

**Approach 2 — QuickSelect:** O(n) average case. The key insight is that after partitioning, the pivot is in its final sorted position. If that position == n-k (0-indexed), we found it. Otherwise recurse on just one side.

```python
import random

def find_kth_largest(nums, k):
    target = len(nums) - k  # kth largest = (n-k)th smallest (0-indexed)

    def quick_select(low, high):
        # Random pivot to avoid O(n²) worst case
        rand = random.randint(low, high)
        nums[rand], nums[high] = nums[high], nums[rand]
        pivot = nums[high]
        i = low - 1

        for j in range(low, high):
            if nums[j] <= pivot:
                i += 1
                nums[i], nums[j] = nums[j], nums[i]

        nums[i + 1], nums[high] = nums[high], nums[i + 1]
        pi = i + 1

        if pi == target:
            return nums[pi]
        elif pi < target:
            return quick_select(pi + 1, high)
        else:
            return quick_select(low, pi - 1)

    return quick_select(0, len(nums) - 1)

print(find_kth_largest([3,2,1,5,6,4], 2))  # 5
```

**Approach 3 — Min-Heap size k:** O(n log k). Maintain a min-heap of size k. The answer is the heap's top.

```python
import heapq

def find_kth_largest_heap(nums, k):
    heap = nums[:k]
    heapq.heapify(heap)      # Min-heap of first k elements
    for num in nums[k:]:
        if num > heap[0]:    # Larger than current kth largest
            heapq.heapreplace(heap, num)
    return heap[0]           # Smallest in heap = kth largest overall
```

**Time Comparison:** QuickSelect O(n) avg | Heap O(n log k) | Full sort O(n log n)
For interviews: heap approach is cleaner to explain. QuickSelect is the "hero" answer if asked for optimal. | LeetCode 215 | Asked at: Amazon, Google, Facebook, Microsoft

---

### Problem 5.5 — Sort Array by Parity (LeetCode 905)

Move all even numbers to the front, odd numbers to the back.

**Two-pointer approach:**

```python
def sort_array_by_parity(nums):
    left, right = 0, len(nums) - 1
    while left < right:
        if nums[left] % 2 != 0 and nums[right] % 2 == 0:
            nums[left], nums[right] = nums[right], nums[left]
        if nums[left] % 2 == 0:
            left += 1
        if nums[right] % 2 != 0:
            right -= 1
    return nums
```

**Time:** O(n) | **Space:** O(1)

---

### Problem 5.6 — Largest Number (LeetCode 179)

Given a list of non-negative integers, arrange them so they form the largest number.

**Key insight:** Custom comparator. To decide if x should come before y: compare (str(x)+str(y)) vs (str(y)+str(x)).

```python
from functools import cmp_to_key

def largest_number(nums):
    def compare(a, b):
        if a + b > b + a:
            return -1   # a should come first
        elif a + b < b + a:
            return 1    # b should come first
        return 0

    nums = list(map(str, nums))
    nums.sort(key=cmp_to_key(compare))
    result = ''.join(nums)
    return '0' if result[0] == '0' else result  # Handle [0, 0] case

print(largest_number([3, 30, 34, 5, 9]))  # "9534330"
```

```cpp
string largestNumber(vector<int>& nums) {
    vector<string> strs(nums.begin(), nums.end());  // Won't compile directly
    // Convert:
    vector<string> s;
    for (int n : nums) s.push_back(to_string(n));
    sort(s.begin(), s.end(), [](const string& a, const string& b){
        return a + b > b + a;
    });
    if (s[0] == "0") return "0";
    string result;
    for (auto& str : s) result += str;
    return result;
}
```

**Time:** O(n log n × m) where m = avg digit length | LeetCode 179 | Asked at: Amazon, Facebook

---

### Problem 5.7 — Merge Two Sorted Arrays Without Extra Space

Given two sorted arrays arr1 (size m) and arr2 (size n), merge them so arr1 contains the first m+n smallest elements and arr2 the remaining n elements. No extra space allowed.

**Gap method (Shell Sort approach):**

```python
import math

def merge_no_extra(arr1, arr2):
    m, n = len(arr1), len(arr2)
    gap = math.ceil((m + n) / 2)

    while gap > 0:
        i = 0
        while i + gap < m + n:
            j = i + gap
            left = arr1[i] if i < m else arr2[i - m]
            right = arr1[j] if j < m else arr2[j - m]

            if left > right:
                if i < m and j < m:
                    arr1[i], arr1[j] = arr1[j], arr1[i]
                elif i < m and j >= m:
                    arr1[i], arr2[j - m] = arr2[j - m], arr1[i]
                else:
                    arr2[i - m], arr2[j - m] = arr2[j - m], arr2[i - m]
            i += 1

        gap = math.ceil(gap / 2) if gap > 1 else 0

arr1, arr2 = [1, 3, 5, 7], [0, 2, 6, 8, 9]
merge_no_extra(arr1, arr2)
print(arr1, arr2)  # [0, 1, 2, 3] [5, 6, 7, 8, 9]
```

**Time:** O((m+n) log(m+n)) | **Space:** O(1)

---

## 6. Common Mistakes & Gotchas

**Merge Sort:**
- Forgetting `<=` vs `<` in the merge step — using strict `<` instead of `<=` makes it unstable (breaks stability guarantee).
- Using `(left + right) / 2` for midpoint — can overflow in C++ with large indices. Use `left + (right - left) / 2`.
- In the C++ in-place version, forgetting to copy temp elements back to the original array after merging.

**Quick Sort:**
- Using last element as pivot on an already-sorted or reverse-sorted array → O(n²) worst case. Always randomize or use median-of-three in interviews.
- Off-by-one errors in the partition boundary: after `partition()` returns `pi`, recurse on `[low, pi-1]` and `[pi+1, high]` — not including `pi` itself (it's already placed).
- Forgetting to handle the case where `low >= high` (base case).
- In the partition, not incrementing `mid` after a swap with `high` (Dutch National Flag). The newly swapped element at `mid` hasn't been inspected yet.

**General:**
- Confusing stable and unstable sorts — selection sort is unstable even though it "looks" like it should be. A swap can move an equal element past another equal element.
- Thinking quick sort is always faster than merge sort — for linked lists, merge sort is better because quick sort needs random access for efficient partitioning.
- Forgetting that Python's `sort()` and `sorted()` are **stable** — this matters when sorting by multiple keys.
- In C++, `std::sort` is **unstable** — use `std::stable_sort` when stability matters.

---

## 7. Interview Q&A Bank

**Q1: What is a stable sorting algorithm? Why does it matter?**
A sorting algorithm is stable if elements with equal keys appear in the same relative order after sorting as they did before. It matters when you sort by multiple criteria sequentially. For example, if you first sort a list of students by grade, then by name, a stable sort ensures students with the same name are still ordered by grade. Merge sort, insertion sort, and bubble sort are stable. Quick sort, selection sort, and heap sort are not.

**Q2: What is the time complexity of merge sort and why is it always O(n log n)?**
Merge sort is always O(n log n) in all cases — best, average, and worst. The recursion tree has log₂(n) levels because we halve the problem at each step. At each level, the merge step does O(n) total work across all the subproblems at that level. Multiplying levels × work per level gives O(n log n). This deterministic behavior — no O(n²) worst case — is merge sort's key advantage over quick sort.

**Q3: Why is quick sort generally faster than merge sort in practice despite the same average time complexity?**
Quick sort is more cache-friendly because it operates in-place — it modifies the original array in memory, benefiting from spatial locality. Merge sort requires an O(n) auxiliary array and accesses two separate memory regions during merge. Additionally, quick sort has smaller constants in its time complexity. The O(n log n) in quick sort involves fewer actual operations per element than merge sort. C++'s `std::sort` uses introsort — a hybrid that starts with quick sort and falls back to heap sort if recursion depth becomes too large, giving O(n log n) guaranteed with quick sort's practical speed.

**Q4: What is the worst case for quick sort and how do you avoid it?**
The worst case is O(n²) and occurs when every partition produces maximally unbalanced splits — one sub-array of size n-1 and one empty. This happens when the pivot is always the min or max element, which occurs with sorted or reverse-sorted input if you always pick the last (or first) element as pivot. Fix: randomized pivot selection (pick a random element, swap with last, then partition). This makes the worst case astronomically unlikely. Another approach is "median-of-three" — choose the median of first, middle, and last elements as pivot.

**Q5: Compare merge sort and quick sort.**

| Property | Merge Sort | Quick Sort |
|---|---|---|
| Time complexity | O(n log n) all cases | O(n log n) avg, O(n²) worst |
| Space complexity | O(n) | O(log n) recursion stack |
| Stable | Yes | No |
| In-place | No | Yes |
| Cache performance | Poor (two separate arrays) | Good (in-place) |
| Best use case | Linked lists, stability needed | General arrays |
| Real-world usage | Python's Timsort, Java's Arrays.sort | C++ std::sort base |

**Q6: When would you choose insertion sort over merge sort or quick sort?**
For small arrays (typically n ≤ 32–64) or nearly-sorted arrays. Insertion sort has lower constant factors than merge sort and quick sort — the overhead of recursive calls and array allocations dominates for small n. Python's Timsort uses insertion sort for runs of length ≤ 64. In competitive programming, a common optimization is to switch from merge sort to insertion sort when the sub-array size drops below a threshold.

**Q7: What is Timsort? Why is it Python's built-in sort?**
Timsort is a hybrid sorting algorithm combining merge sort and insertion sort, designed by Tim Peters in 2002 for Python. It exploits the fact that real-world data often has already-sorted or nearly-sorted sub-sequences ("runs"). It identifies natural runs in the data, uses insertion sort to extend small runs to a minimum size (32–64), then uses a modified merge sort to merge the runs. This gives O(n) best case on sorted data and O(n log n) guaranteed worst case, while being stable. Java's `Arrays.sort` for objects also uses Timsort.

**Q8: Can you implement merge sort for a linked list?**
Yes — and for linked lists, merge sort is the preferred algorithm because it doesn't need random access. Find the midpoint (slow/fast pointer technique), split the list, recursively sort both halves, then merge by relinking nodes. Quick sort requires random access for efficient partitioning, which makes it less suitable for linked lists.

**Q9: What sorting algorithm would you use to sort 1 million integers in the range [0, 1000]?**
Counting sort. Since the range k = 1001 is much smaller than n = 1,000,000, counting sort runs in O(n + k) ≈ O(n) time. Create an array of size 1001, count occurrences of each value, then reconstruct the sorted array. This beats any comparison-based sort's O(n log n) for this special case.

**Q10: What is the recurrence relation for merge sort and how do you solve it?**
`T(n) = 2T(n/2) + O(n)`, where 2T(n/2) accounts for the two recursive halves and O(n) for the merge step. By the Master Theorem (case 2: a=2, b=2, f(n)=n, n^(log_b a) = n^1 = n), the solution is T(n) = O(n log n).

**Q11: What is the space complexity of quick sort?**
O(log n) average case for the recursion stack. In the best case (perfectly balanced pivots), recursion depth is O(log n). In the worst case (always unbalanced pivot), depth is O(n). An optimization called "tail call elimination" reduces the stack depth by recursing only on the smaller half and handling the larger half iteratively, guaranteeing O(log n) stack depth.

**Q12: How does the Dutch National Flag algorithm work and what problem does it solve?**
The Dutch National Flag problem (Dijkstra, 1976) partitions an array into three sections in O(n) time with O(1) space. It uses three pointers — low, mid, high. Elements at indices less than low are "red" (0s), elements between low and mid-1 are "white" (1s), elements from mid to high are unexplored, and elements after high are "blue" (2s). At each step, examine arr[mid]: if 0, swap with arr[low] and advance both; if 1, advance mid; if 2, swap with arr[high] and retreat high. This is the LeetCode 75 (Sort Colors) solution.

**Q13: What is an inversion in an array? How is it related to sorting?**
An inversion is a pair (i, j) where i < j but arr[i] > arr[j]. The number of inversions measures how "unsorted" an array is: 0 inversions means sorted, n(n-1)/2 inversions means reverse-sorted. Insertion sort performs exactly one swap per inversion — so its time complexity is O(n + inversions). Counting inversions in O(n log n) is a classic problem solved by modifying merge sort's merge step.

**Q14: What is the lower bound for comparison-based sorting?**
Any comparison-based sorting algorithm requires at least O(n log n) comparisons in the worst case. The proof uses a decision tree model: any correct sort must distinguish between all n! permutations of n elements. A binary decision tree with n! leaves has height at least log₂(n!) ≈ n log n (by Stirling's approximation). So no comparison sort can beat O(n log n) in the worst case. Counting sort, radix sort, and bucket sort are non-comparison-based and can do better for special input distributions.

**Q15: How would you sort a nearly-sorted array of n elements where each element is at most k positions away from its sorted position?**
Use a min-heap of size k+1. Insert the first k+1 elements. Then for each remaining element, extract the minimum from the heap (this is the correct next element in sorted order) and insert the new element. The heap never needs to hold more than k+1 elements. Time: O(n log k). If k is small (e.g., k=10), this is much faster than O(n log n).

---

**Answer Framework — "Implement Merge Sort":**
State base case → explain divide step (mid index, avoid overflow) → explain conquer step (two recursive calls) → explain merge step (two pointers, rebuild array) → state time O(n log n) and space O(n) → mention stability.

**Answer Framework — "Quick Sort vs Merge Sort":**
Define both → time/space trade-off table → when to use each → mention real-world usage (Timsort, introsort) → stability discussion.

**Depth Gauge:**
- **Junior (SDE-1):** Implement merge sort and quick sort from scratch. Know time and space complexity. Explain stability. Solve Sort Colors.
- **Mid-level (SDE-2):** Explain why quicksort is faster in practice. Randomized quicksort. Count inversions using merge sort. Kth Largest via quickselect. Custom comparator sorts.
- **Senior (SDE-3+):** Discuss Timsort internals. External merge sort (data too large for RAM). Parallel merge sort. Sort stability in multi-key sorting. Analysis via Master Theorem.

---

## 8. Source Notes

| Source | Contribution |
|---|---|
| Striver A2Z Playlist (takeuforward.org) | Algorithm sequence, problem list (Sort Colors, Merge Intervals) |
| interviewing.io/sorting-interview-questions | Stability/in-place comparison table, when-to-use framework |
| SourceCodeSolutions sorting guide | Merge sort trace, quick sort decision tree |
| AlgoCademy sorting guide | Insertion sort for small arrays, complete code implementations |
| GeeksforGeeks sorting interview questions | Inversion count, K-way merge, quicksort on linked lists |
| InterviewCake sorting cheat sheet | Linux kernel heapsort, merge sort for external sort |
| Devinterview.io sorting questions | Stability definition, Timsort explanation |

---

## 9. Glossary

**Stable Sort:** A sort where equal elements maintain their original relative order.

**In-Place Sort:** A sort that uses O(1) extra space (operates on the original array without significant auxiliary storage).

**Adaptive Sort:** A sort that performs better on partially sorted input — insertion sort is the classic example.

**Comparison Sort:** A sort that determines order by comparing elements. Lower bound is O(n log n).

**Non-comparison Sort:** Sorting that exploits structure of the values (counting sort, radix sort). Can beat O(n log n).

**Divide and Conquer:** Algorithm strategy that splits the problem into smaller sub-problems, solves recursively, and combines.

**Inversion:** A pair (i, j) with i < j but arr[i] > arr[j]. Measures disorder in an array.

**Pivot (Quick Sort):** The element chosen to partition the array into less-than and greater-than regions.

**Partition:** The step in quick sort that places the pivot in its final position and rearranges elements around it.

**Timsort:** Python's and Java's built-in hybrid sort combining merge sort and insertion sort. O(n) best case, O(n log n) worst, stable.

**Introsort:** C++ `std::sort`'s algorithm — quicksort with heapsort fallback when recursion depth exceeds 2 log n. O(n log n) guaranteed.

---

*Next: Arrays + Hashing — the highest-ROI topic for Indian product company interviews.*
