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
- Maintain map of element counts.

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
- Window moves in parallel.

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
