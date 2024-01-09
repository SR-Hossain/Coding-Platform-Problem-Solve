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
