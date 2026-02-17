# Sliding Window Pattern

---

## Solution Patterns

1. Brute force solution
2. Better solution
3. Optimum solution

---

## Hint to Identify Sliding Window Questions

Look for keywords like:

- **Longest sub-array**
- **Longest sub-string**
- **Maximum/minimum sum**
- **At most k elements**
- **Exactly k elements**

---

## Core Idea of Sliding Window

Two pointers:

- **Expand window → right pointer (r++)**
- **Shrink window → left pointer (l++)**

---

# Fixed Window Example

## Problem

```
arr = [6 2 3 4 7 2 1 7 10]
k = 4  (number of consecutive elements)

Find maximum sum.
```

Window size is constant (always 4).

---

# Variable Window Examples

## Example 1

```
arr = [1 1 1 0 0 0 1 1 1 1 0 0 1 0 0]
k = 2 (maximum two flips: 0 → 1)

Find longest consecutive ones.
```

Equivalent to:

```
Longest subarray with at most 2 zeros.
```

---

## Example 2

```
arr = [3 3 3 1 2 1 1 2 1 3 3 2 1]

Find longest subarray with at most 2 unique digits.
```

---

## Example 3

```
str = aaabacbbeaacd

Find longest substring with at most 2 unique characters.
```

---

# Solution Pattern

## 1. Brute Force Approach

### Example

```
str = aaabacbbeaacd
Find longest substring with at most 2 unique characters.
```

### Pseudocode

```js
maxiLen = 0

for(i = 0 → n) {
    // set to store unique characters
    set = {}

    for(j = i → n) {
        set.add(str[j])

        if(set.size <= k) {
            maxiLen = max(maxiLen, j - i + 1)
        } else {
            break
        }
    }
}

return maxiLen
```

### Complexity

```
Time: O(n²)
Space: O(k+1)
```

---

## 2. Better Solution (Sliding Window)

### Idea

- Expand window using right pointer.
- If condition breaks, shrink using left pointer.
- Maintain a map or counter to track element frequencies.
- Adjust the window until the condition becomes valid again.
- Sliding window works best when the condition is: Based on a range (e.g., sum ≤ k, at most k distinct characters).
  - Number of valid subarrays
  - Number of valid substrings
- Sliding window usually does not work when count sub arrays:The condition is exact equality (e.g., sum = target)
  example [1 0 0 1 1 0] target =2 here we are not sure either expand or shrink this process we will loss sub arrays

### Pseudocode

```js
map = {}
maxiLen = 0
l = 0
r = 0

while(r < n) {

    // expand
    map[arr[r]]++

    // shrink until condition valid
    while(map.size > k) {
        map[arr[l]]--

        if(map[arr[l]] === 0) {
            remove map[arr[l]]
        }

        l++
    }

    // valid condition
    if(map.size <= k) {
        maxiLen = max(maxiLen, r - l + 1)
    }

    r++
}

return maxiLen
```

### Complexity

```
r moves from 0 → n
l moves from 0 → n

Total steps:
n + n = 2n ≈ O(n)

Time: O(n)
Space: O(k+1)
```

---

## 3. Optimum Solution

### Idea

- Expand with right pointer.
- Shrink only once when condition breaks.
- No need to fully shrink every time.
- The window moves forward in parallel (left and right pointers progress together).
- Mainly used when we are:
- Looking for maximum window size
- Finding longest valid subarray/substring

### Limitation

- This approach cannot be used when we need count of all valid subarrays/substrings
- It is mainly for optimization problems, not counting problems.

### Pseudocode

```js
map = {}
maxiLen = 0
l = 0
r = 0

while(r < n) {

    // expand
    map[arr[r]]++

    // shrink once if condition breaks
    if(map.size > k) {
        map[arr[l]]--

        if(map[arr[l]] === 0) {
            remove map[arr[l]]
        }

        l++
    }

    // valid condition
    if(map.size <= k) {
        maxiLen = max(maxiLen, r - l + 1)
    }

    r++
}

return maxiLen
```

---

## Important Note (Optimum Logic)

- Once we get a valid `maxiLen`,
- We keep moving both pointers forward.
- No need to shrink fully every time.
- If a larger window exists, it will be found naturally.

---

## Complexity

```
Time: O(n)
Space: O(k+1)
```

# Sliding Window & Prefix Sum Patterns

## Two Major Patterns for Subarray/Substring Problems

1. Sliding Window
2. Prefix Sum + Hash Map

Choosing the correct pattern depends on:

- Type of condition (range vs exact)
- Type of values (positive, negative, binary)

---

# Sliding Window Pattern

## Core Idea

Use two pointers:

- Expand window → right++
- Shrink window → left++

Maintain:

- Running sum
- Frequency map
- Counter
- Set

---

## When Sliding Window Works

Sliding window works when:

- Condition is range-based

Examples:

- Sum ≤ k
- At most k distinct elements
- At most k zeros
- Window length constraints

### Works Best When Array Contains

- Only positive numbers
- Binary values (0 and 1)

Reason:

- Expanding window → sum increases
- Shrinking window → sum decreases
- Window behaves predictably

---

## Common Sliding Window Problems

### Optimization Problems

- Longest subarray
- Shortest subarray
- Maximum sum
- Minimum window

### Counting Problems (Range-Based)

- Number of subarrays with sum ≤ k
- Number of substrings with at most k distinct characters

---

## When Sliding Window Fails

Sliding window fails when:

1. Array contains negative numbers
2. Condition requires exact equality
   sum = target

Example:
arr = [2, -1, 2]
target = 3

Problem:

- Expanding does not always increase sum.
- Shrinking does not always decrease sum.
- Window becomes unpredictable.

So sliding window logic breaks.

---

## Special Case: Binary Arrays

For binary arrays (0 and 1), sliding window can still be used for exact sums:

count(sum = k) = count(sum ≤ k) − count(sum ≤ k−1)

This converts exact equality into a range condition.

---

# Prefix Sum + Hash Map Pattern

## Core Idea

Track cumulative sums and store:

- Frequency (for counting)
- First index (for longest length)

At each step:
currentSum − target = previousSum

If that previous sum exists:

- A valid subarray is found.

---

## When Prefix Sum Works

Prefix sum works for:

- Exact sum conditions
- Negative numbers
- Positive numbers
- Mixed values
- Binary arrays

# Pattern Selection Guide

## Use Sliding Window When

- Condition is:
  - ≤
  - ≥
  - At most
  - At least
- Array has:
  - Positive values
  - Binary values
- Need:
  - Longest window
  - Shortest window
  - Count with range condition

---

## Use Prefix Sum When

- Condition is exact equality (=)
- Need:
  - Count subarrays with sum = target
  - Longest subarray with sum = target
- Array contains:
  - Negative numbers
  - Mixed values

---

# Prefix Sum Solution Patterns

## Pattern 1: Count Subarrays with Sum = Target

### Use When

- Need number of subarrays
- Condition is sum = target
- Works with:
  - Positive
  - Negative
  - Mixed arrays
  - Binary arrays

### Logic

1. Maintain running sum.
2. Store frequency of each prefix sum in a map.
3. At each index:
   remaining = currentSum − target
4. If remaining exists:
   add its frequency to count.

### Code (JavaScript)

function countSubarrays(arr, target) {
let map = new Map([[0, 1]]);
let sum = 0;
let count = 0;

for (let i = 0; i < arr.length; i++) {
sum += arr[i];

    let remaining = sum - target;
    if (map.has(remaining)) {
      count += map.get(remaining);
    }

    map.set(sum, (map.get(sum) || 0) + 1);

}

return count;
}

### Complexity

Time: O(n)  
Space: O(n)

---

## Pattern 2: Maximum Length Subarray with Sum = Target

### Use When

- Need longest subarray
- Condition is sum = target
- Works with:
  - Positive
  - Negative
  - Mixed arrays

### Logic

1. Maintain running sum.
2. Store first occurrence of each prefix sum.
3. At each index:
   remaining = currentSum − target
4. If remaining exists:
   length = currentIndex − storedIndex
5. Update maximum length.

### Code (JavaScript)

function longestSubarray(arr, target) {
let map = new Map();
let sum = 0;
let maxLen = 0;

for (let i = 0; i < arr.length; i++) {
sum += arr[i];

    if (sum === target) {
      maxLen = i + 1;
    }

    let remaining = sum - target;
    if (map.has(remaining)) {
      let prevIndex = map.get(remaining);
      maxLen = Math.max(maxLen, i - prevIndex);
    }

    if (!map.has(sum)) {
      map.set(sum, i);
    }

}

return maxLen;
}

### Complexity

Time: O(n)  
Space: O(n)

---

# Key Difference Between the Two Prefix Sum Patterns

| Problem Type     | Map Stores                | Purpose                   |
| ---------------- | ------------------------- | ------------------------- |
| Count subarrays  | Frequency of prefix sums  | Count all valid subarrays |
| Longest subarray | First index of prefix sum | Get maximum length        |

---

# Quick Interview Cheat Sheet

## Sliding Window

- Range-based conditions
- Positive or binary arrays
- Longest/shortest window
- Sum ≤ k

## Prefix Sum

- Exact equality
- Negative or mixed arrays
- Count or longest subarray with sum = target
