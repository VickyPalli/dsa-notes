# Sliding Window & Prefix Sum — Concise Guide

A compact reference for sliding window and prefix-sum patterns with working implementations.

---

## When to use which pattern

- Sliding Window: range-based conditions (≤, ≥, at most), arrays with positive or binary values, longest/shortest window, max/min sum.
- Prefix Sum + Hash Map: exact equality (sum = target), arrays with negatives or mixed values, counting or longest exact-sum subarrays.
- Binary exact-sum trick: count(sum = k) = count(sum ≤ k) − count(sum ≤ k−1)

---

## Core idea — Sliding Window

- Two pointers: expand (right++), shrink (left++).
- Maintain running sum or frequency map.
- Works well when window behavior is monotonic (positive/binary values).

### Fixed-window example (constant k)

Find max sum of k consecutive elements:

```js
// Fixed window (concept)
let sum = 0,
  maxSum = -Infinity;
for (let i = 0; i < arr.length; i++) {
  sum += arr[i];
  if (i >= k) sum -= arr[i - k];
  if (i >= k - 1) maxSum = Math.max(maxSum, sum);
}
```

### Sliding-window template (variable window)

```js
let l = 0, sum = 0, best = 0;
for (let r = 0; r < arr.length; r++) {
  // expand
  sum += arr[r];

  // shrink while invalid
  while (/* invalid condition */) {
    sum -= arr[l];
    l++;
  }

  // valid window → update answer
  best = Math.max(best, /* window value e.g., r-l+1 or sum */);
}
```

---

## Binary array: exact sum via "at most" helper

```js
function countAtMost(arr, k) {
  if (k < 0) return 0;
  let l = 0,
    sum = 0,
    count = 0;
  for (let r = 0; r < arr.length; r++) {
    sum += arr[r];
    while (sum > k) {
      sum -= arr[l++];
    }
    count += r - l + 1;
  }
  return count;
}

function countSubarraysExact(arr, k) {
  return countAtMost(arr, k) - countAtMost(arr, k - 1);
}
```

Time: O(n), Space: O(1)

---

## Prefix Sum + Hash Map patterns

Use when exact equality or negatives are present.

### Count subarrays with sum = target

```js
function countSubarrays(arr, target) {
  let map = new Map([[0, 1]]);
  let sum = 0,
    count = 0;
  for (let x of arr) {
    sum += x;
    let rem = sum - target;
    if (map.has(rem)) count += map.get(rem);
    map.set(sum, (map.get(sum) || 0) + 1);
  }
  return count;
}
```

### Longest subarray with sum = target

```js
function longestSubarray(arr, target) {
  let map = new Map(); // prefixSum -> firstIndex
  let sum = 0,
    maxLen = 0;
  for (let i = 0; i < arr.length; i++) {
    sum += arr[i];
    if (sum === target) maxLen = i + 1;
    let rem = sum - target;
    if (map.has(rem)) maxLen = Math.max(maxLen, i - map.get(rem));
    if (!map.has(sum)) map.set(sum, i);
  }
  return maxLen;
}
```

Time: O(n), Space: O(n)

---

## Quick checklist

- Sliding window: choose when condition is "range-like" and array values are non-negative or binary.
- Prefix sum: choose when condition is exact (=) or array contains negatives.
- For counting exact sums in binary arrays, convert to two "at most" queries.

---

## Complexity summary

- Sliding window: O(n) time, O(1..k) space depending on counters.
- Prefix-sum hash map: O(n) time, O(n) space.
