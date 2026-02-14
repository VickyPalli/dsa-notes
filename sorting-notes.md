# Sorting Notes

---

# Merge Sort

**Technique:** Divide and Conquer

## Example

```
[1 2 5 2 3 1 6 5 8]
```

---

## Flow Explanation

Merge sort works in two main phases:

1. **Divide**
   - Split the array into two halves.
   - Keep dividing until each part has only one element.

2. **Merge**
   - Compare elements from both sides.
   - Build a temporary sorted array.
   - Copy back into the original array.

---

## Step-by-Step Division

```
[1 2 5 2 3 1 6 5 8]
          |
      divide at mid
          |
[1 2 5 2 3]   [1 6 5 8]
     |             |
   divide        divide
     |             |
[1 2 5] [2 3]   [1 6] [5 8]
   |      |        |     |
 divide divide   divide divide
   |      |        |     |
[1][2][5][2][3][1][6][5][8]
```

---

## Step-by-Step Merging

```
[1] + [2] → [1 2]
[1 2] + [5] → [1 2 5]

[2] + [3] → [2 3]

[1 2 5] + [2 3]
Compare:
1 vs 2 → 1
2 vs 2 → 2
5 vs 2 → 2
5 vs 3 → 3
remaining → 5

Result:
[1 2 2 3 5]
```

Right side:

```
[1] + [6] → [1 6]
[5] + [8] → [5 8]
[1 6] + [5 8] → [1 5 6 8]
```

Final merge:

```
[1 2 2 3 5] + [1 5 6 5 8]
→ [1 1 2 2 3 5 5 6 8]
```

---

## Merge Sort Pseudocode

```js
merge_sort(arr, left, right) {
    if (left >= right) {
        return
    }

    let mid = (left + right) / 2

    // left part recursive call
    merge_sort(arr, left, mid)

    // right part recursive call
    merge_sort(arr, mid + 1, right)

    merging(arr, left, mid, right)
}
```

---

## Merging Logic

```js
merging(arr, left, mid, end) {
    i = left
    j = mid + 1
    temp = []

    while (i <= mid && j <= end) {
        if (arr[i] <= arr[j]) {
            temp.push(arr[i])
            i++
        } else {
            temp.push(arr[j])
            j++
        }
    }

    // push remaining left part
    while (i <= mid) {
        temp.push(arr[i])
        i++
    }

    // push remaining right part
    while (j <= end) {
        temp.push(arr[j])
        j++
    }
}
```

---

# Quick Sort

## Flow Explanation

Quick sort steps:

1. Pick a **pivot element**.
2. Move smaller elements to the left.
3. Move larger elements to the right.
4. Recursively sort both sides.

---

## Quick Sort Flow Example

```
Array:
[5 2 8 1 3]

Pivot = 5
```

### Partition Step

```
2 < 5 → left
8 > 5 → right
1 < 5 → left
3 < 5 → left

Result:
[2 1 3] 5 [8]
```

### Sort Left Side

```
[2 1 3]
Pivot = 2

1 < 2 → left
3 > 2 → right

Result:
[1] 2 [3]
```

### Final Result

```
[1 2 3] 5 [8]
→ [1 2 3 5 8]
```

---

## Quick Sort Pseudocode

```js
quick_sort(arr, left, end) {
    if (left >= right) {
        return
    }

    partition_index = quick_algo(arr, left, end)

    // left part recursive call
    quick_sort(arr, left, partition_index - 1)

    // right part recursive call
    quick_sort(arr, partition_index + 1, end)
}
```

---

## Partition Logic

```js
quick_algo(arr, left, end) {
    pivot_element = arr[left]
    i = left
    j = end

    while (i <= j) {

        // find first higher value from left
        while (arr[i] <= pivot_element && i < end - 1) {
            i++
        }

        // find smaller value from right
        while (arr[j] > pivot_element) {
            j++
        }

        if (i < j) {
            swap(arr[i], arr[j])
        }
    }

    return j
}
```
