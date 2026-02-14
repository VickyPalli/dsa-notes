# Recursion

## Definition

Recursion means:

```
A function calling itself.
```

---

## Important Rule

Every recursive function must have:

- A **base condition**
- Otherwise it will run infinitely.

---

## Common Pattern

Used to:

- Generate subarrays
- Generate subsets
- Explore all possible choices

---

## Core Idea

At every index, we have **two choices**:

1. **Take the element**
2. **Do not take the element**

This is called the:

```
Take / Not Take pattern
```

---

# Example Problem

## Problem

```
arr = [3 1 2]
Generate all subarrays (subsets)
```

### Expected Output

```
[]
[3]
[1]
[2]
[3 1]
[3 2]
[3 1 2]
[1 2]
```

---

# Recursion Flow Idea

At every index:

```
Index 0 → value = 3
    take → include 3
    not take → skip 3

Index 1 → value = 1
    take → include 1
    not take → skip 1

Index 2 → value = 2
    take → include 2
    not take → skip 2
```

This creates a decision tree.

---

## Decision Tree

```
                []
           /          \
        take3        not3
        [3]            []
       /   \          /   \
    take1 not1    take1  not1
     [3,1] [3]     [1]    []
     /   \  / \     / \    / \
  t2   n2 t2 n2   t2 n2  t2 n2
```

Final subsets:

```
[]
[3]
[1]
[2]
[3 1]
[3 2]
[3 1 2]
[1 2]
```

---

# Recursion Pattern (Take / Not Take)

## Pseudocode

```js
recursive(arr, i, collector) {

    // base condition
    if (i == arr.length) {
        // perform logic once we collect subarray
        print(collector)
        return
    }

    // take
    collector.push(arr[i])
    recursive(arr, i + 1, collector)

    // not take
    collector.pop()
    recursive(arr, i + 1, collector)
}
```

---

# Step-by-Step Flow

### Start

```
arr = [3, 1, 2]
i = 0
collector = []
```

---

### Step 1: Take 3

```
collector = [3]
i = 1
```

### Step 2: Take 1

```
collector = [3,1]
i = 2
```

### Step 3: Take 2

```
collector = [3,1,2]
i = 3 → base condition reached
Output: [3,1,2]
```

---

### Backtrack (Not Take 2)

```
collector = [3,1]
Output: [3,1]
```

---

### Backtrack (Not Take 1)

```
collector = [3]
```

Take 2:

```
collector = [3,2]
Output: [3,2]
```

Not take 2:

```
collector = [3]
Output: [3]
```

---

### Backtrack to Empty

```
collector = []
```

Take 1:

```
collector = [1]
Take 2 → [1,2]
Output: [1,2]

Not take 2 → [1]
Output: [1]
```

---

### Final

```
collector = []
Take 2 → [2]
Output: [2]

Not take 2 → []
Output: []
```

---

# Key Pattern to Remember

At every step:

```
1. Choose element (take)
2. Explore deeper
3. Undo choice (backtrack)
4. Skip element (not take)
```

---

# Time Complexity

```
Each element has 2 choices.
Total subsets = 2^n
Time = O(2^n)
```

---

# Space Complexity

```
Recursion stack depth = n
Space = O(n)
```
