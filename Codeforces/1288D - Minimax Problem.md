# [1288D - MiniMax Problem](https://codeforces.com/problemset/problem/1288/D)

Binary Search from 0 to 10**10

bitmask the array element with 1 where element>=ans (binary search)

input:

```
6 5
5 0 3 1 2
1 8 9 1 3
1 2 3 4 5
9 1 0 3 7
2 3 0 6 3
6 4 1 7 0
```

output:

```
1 5
```

The answer is: 3

For 3,

arr[1] = 10100, here arr[1][i]=1 if arr[1][i]>=3

arr[5] = 01011

So, we are replacing the max value from both arr[1] and arr[5], we have

11111 (2**m-1)

```python
n, m = map(int, input().split())

arr = [list(map(int, input().split())) for _ in range(n)]

l = 0
r = 10 ** 10
u = v = 1

while l < r:
    mid = (l + r) // 2
    d = dict()
    for i in range(n):
        cnt = 0
        for j in range(m):
            cnt = cnt * 2 + (arr[i][j] >= mid)
        d[cnt] = i
    f = True
    for x in d:
        for y in d:
            if x | y == 2 ** m - 1:
                u, v = d[x] + 1, d[y] + 1
                l = mid + 1
                f = False
                break
        if not f:
            break
    if f:
        r = mid

print(u, v)
```
