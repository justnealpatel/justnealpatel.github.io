# Notes

## Sorting Algorithms

### Quick Sort



Code:  [quicksort.py](../../Development/Playground/Messing Around/quicksort.py)

### Merge Sort



Code:  [mergesort.py](../../Development/Playground/Messing Around/mergesort.py)

### Heap Sort

Here is some `inline code`

Code:  [heapsort.py](../../Development/Playground/Messing Around/heapsort.py)

## Challenges

```python
# Single comment
"""
This
is
a
multi-line
comment
"""
def test(var1, var2):
    return var1 + var2

tmp = {}
tmp2 = [45, "hello", test]
```

- Bullet point
1. Numbered bullet point

Link: [Google](https://www.google.com)

Bold: **BOLD**

Underline: <u>Underlined</u>

Italics: *italicized*

# Amazon OA Questions

## Reorder Log Files

- Custom Sort
	- Instead of specifying default order, we can write a custom one
	- The rules are:
		- Letter-logs come before digit-logs;
		- Letter-logs are sorted alphanumerically, by content then identifier;
		- Digit-logs remain in the same order.
- Time: $O(n\, log\, n)$
- Space: $O(n)$ (Timsort has $O(n)$ space complexity unlike merge sort)

```python
def reorderLogFiles(logs):
    def custom_sort(log):
        splitted = log.split(" ", 1)
        identifier = splitted[0]
        remains = splitted[1]
        return (0, remains, identifier) if remains[0].isalpha() else (1,)

    return sorted(logs, key = custom_sort)
```

## Optimal Utilization

- Given 2 lists `a` and `b`. Each element is a pair of integers where the first integer represents the unique id and the second integer represents a value. Your task is to find an element from `a` and an element from `b` such that the sum of their values is less or equal to `target` and as close to `target` as possible. Return a list of ids of selected elements. If no pair is possible, return an empty list.
- Time: $O(m \cdot n)$

```python
def findPairs(a, b, target):
	a.sort(key=lambda x: x[1])
	b.sort(key=lambda x: x[1])
	l, r = 0, len(b) - 1
	ans = []
	curDiff = float('inf')
	while l < len(a) and r >= 0:
		id1, i = a[l]
		id2, j = b[r]
		if (target - i - j == curDiff):
			ans.append([id1, id2])
		elif (i + j <= target and target - i - j < curDiff):
			ans.clear()
			ans.append([id1, id2])
			curDiff = target - i - j
		if (target > i + j):
			l += 1
		else:
			if target == i + j:
				tmp_l = l
				while a[tmp_l][1] + b[r][1] == target:
					tmp_l += 1
					if tmp_l == len(a):
						break
					if  a[tmp_l][1] + b[r][1] == target:
						ans.append([a[tmp_l][0], b[r][0]])
			r -= 1
	
	ans.sort(key = lambda x: x[1])
	ans.sort(key = lambda x: x[0])
	return ans
```

## Min Cost To Cut Ropes

- Given `n` ropes of different lengths, we need to connect these ropes into one rope. We can connect only 2 ropes at a time. The cost required to connect 2 ropes is equal to sum of their lengths. The length of this connected rope is also equal to the sum of their lengths. This process is repeated until `n` ropes are connected into a single rope. Find the min possible cost required to connect all ropes.
- Time: $O(n\, log\, n)$
- Space: $O(n)$

```python
from heapq import heappop, heappush, heapify
def minCost(ropes):
    if not ropes: return 0
    if len(ropes) == 1: return ropes[0]
    heapify(ropes)
    cost = 0
    while len(ropes) > 1:
        a, b = heappop(ropes), heappop(ropes)
        cost += a + b
        if ropes:
            heappush(ropes, a+b)
    return cost
```

## Treasure Island

- You have a map that marks the location of a treasure island. Some of the map area has jagged rocks and dangerous reefs. Other areas are safe to sail in. There are other explorers trying to find the treasure. So you must figure out a shortest route to the treasure island.
- Assume the map area is a two dimensional grid, represented by a matrix of characters. You must start from the top-left corner of the map and can move one block up, down, left or right at a time. The treasure island is marked as `X` in a block of the matrix. `X` will not be at the top-left corner. Any block with dangerous rocks or reefs will be marked as `D`. You must not enter dangerous blocks. You cannot leave the map area. Other areas `O` are safe to sail in. The top-left corner is always safe. Output the minimum number of steps to get to the treasure.

```python
from collections import deque
def treasure_island_1(m):
    if len(m) == 0 or len(m[0]) == 0:
        return -1  # impossible

    matrix = [row[:] for row in m]
    nrow, ncol = len(matrix), len(matrix[0])

    q = deque([((0, 0), 0)])  # ((x, y), step)
    matrix[0][0] = "D"
    while q:
        (x, y), step = q.popleft()
        for dx, dy in [[0, 1], [0, -1], [1, 0], [-1, 0]]:
            if 0 <= x+dx < nrow and 0 <= y+dy < ncol:
                if matrix[x+dx][y+dy] == "X":
                    return step+1
                elif matrix[x+dx][y+dy] == "O":
                    # mark visited
                    matrix[x + dx][y + dy] = "D"
                    q.append(((x+dx, y+dy), step+1))
    return -1
```

## Zombie in Matrix

- Given a 2D grid, each cell is either a zombie `1` or a human `0`. Zombies can turn adjacent (up/down/left/right) human beings into zombies every hour. Find out how many hours does it take to infect all humans?

```python
def minHour(rows, columns, grid):
    if not rows or not columns:
        return 0
    q = [[i,j] for i in range(rows) for j in range(columns) if grid[i][j]==1]
    directions = [[1,0],[-1,0],[0,1],[0,-1]]
    time = 0
    while True:
        new = []
        for [i,j] in q:
            for d in directions:
                ni, nj = i + d[0], j + d[1]
                if 0 <= ni < rows and 0 <= nj < columns and grid[ni][nj] == 0:
                    grid[ni][nj] = 1
                    new.append([ni,nj])
        q = new
        if not q:
            break
        time += 1
        
    return time
```

## Find Pair With Given Sum

- Given a list of positive integers `nums` and an int `target - 30`, return indices of the two numbers such that they add up to a `target` (TwoSum).

	- Conditions:

	- You will pick exactly 2 numbers.
	- You cannot pick the same element twice.
	- If you have muliple pairs, select the pair with the largest number.

```python
def givenSum(nums,target):
    target -= 30
    res = {}
    out = []
    for index, value in enumerate(nums):
        if target - value in res:
            if res[target-value]>any(out) or index>any(out):
                out.clear()
                out.extend([res[target-value], index])
        else:
            res[value] = index

    return out
```

## Flight Details

- You are on a flight and wanna watch two movies during this flight.
	You are given `List movieDurations` which includes all the movie durations.
	You are also given the duration of the flight which is `d` in minutes.
	Now, you need to pick two movies and the total duration of the two movies is less than or equal to `(d - 30min)`.
- Find the pair of movies with the longest total duration and return they indexes. If multiple found, return the pair with **the longest movie**.

```python
def flightDetails(arr, k):
    m = []
    for i in range(len(arr)):
        m.append((arr[i], i))

    # sort by durations
    m.sort(key=lambda x: x[0])

    k -= 30
    left = 0
    right = len(arr) - 1
    pairs = None
    global_minutes = 0

    while left < right:
        local_minutes = m[left][0] + m[right][0]
        if local_minutes <= k:
            if local_minutes == k :
                return [m[left][1], m[right][1]]
            elif local_minutes > global_minutes:
                global_minutes = local_minutes
                pairs = [m[left][1], m[right][1]]
            left += 1
        else:
            right -= 1
    return pairs
```

## Amazon LPs

- Customer Obsession
- Ownership
- Invent and Simplify
- Are Right, A Lot
- Learn and Be Curious
- Hire and Develop the Best
- Insist on the Higher Standards
- Think Big
- Bias for Action
- Frugality
- Earns Trust
- Dive Deep
- Have Backbone; Disagree and Commit
- Deliver Results