## count 1 in the array but *do not append* to your array from the input

## Solution for one
1*k <= max(k, x)

For 1st 1 in the array, there are n-1 pairs

For 2nd 1, there are more n-2 pairs

For 3rd 1, there are more n-3 pairs

...

For last 1, supposing there are k 1 in the array, there are more n-k pairs

ans_for_ones = n-1 + n-2 + n-3 + ... + n-k
= n*k - k*(k+1)/2


## we will do divide and conquer algorithm and modify merge sort like this
- find max element
- find first index of the max element
- if the max element is side by side consecutively, find its last index too

```
# in merge_sort method
mx = max(arr[l:r + 1])
midleft = l
for i in range(l, r + 1):
    if arr[i] == midleft:
        midleft = i
        break
midright = midleft
while midright + 1 <= r and arr[midright + 1] == mx:
    midright += 1
merge_sort(l, midleft - 1)
merge_sort(midright + 1, r)
```

(sorted left_side) (max elements) (sorted right_side) [after using merge sort in smaller segments of the array]

=> (merge left_side and right_side) (max elements) = (sorted array)

## For example:

4 1 3 5 1 **12 12 12 12*** 4 2 12 4 1

1 1 3 4 5 **12 12 12 12*** 1 2 4 4 12
```python
i = midleft - 1
j = midright + 1
while i >= l and j <= r:
    if arr[i] * arr[j] <= mx:
        ans += i - l + 1 # arr[p] <= arr[i] for p<i, so arr[p]*arr[j] is also less than mx
        j += 1
    else:
        i -= 1
```

## After everything

ans = ans + ans_for_ones



# Full code
```python
def merge(left_index, right_index, left_index_of_max_element, right_element_of_max_element):
    i, j = left_index, right_element_of_max_element + 1
    tmp = []
    while i <= left_index_of_max_element - 1 and j <= right_index:
        if arr[i] <= arr[j]:
            tmp.append(arr[i])
            i += 1
        else:
            tmp.append(arr[j])
            j += 1
    while i <= left_index_of_max_element - 1:
        tmp.append(arr[i])
        i += 1
    while j <= right_index:
        tmp.append(arr[j])
        j += 1
    max_element_count = right_element_of_max_element - left_index_of_max_element + 1
    max_element = arr[left_index_of_max_element]
    tmp += [max_element] * max_element_count
    for i in range(left_index, right_index + 1):
        arr[i] = tmp[i - left_index]


def merge_sort(left_index, right_index):
    if left_index >= right_index:
        return
    left_index_of_max_element = left_index
    max_element = max(arr[left_index:right_index + 1])
    for i in range(left_index, right_index + 1):
        if arr[i] == left_index_of_max_element:
            left_index_of_max_element = i
            break
    right_index_of_max_element = left_index_of_max_element
    while right_index_of_max_element + 1 <= right_index and arr[right_index_of_max_element + 1] == max_element:
        right_index_of_max_element += 1
    merge_sort(left_index, left_index_of_max_element - 1)
    merge_sort(right_index_of_max_element + 1, right_index)
    i = left_index_of_max_element - 1
    j = right_index_of_max_element + 1
    while i >= left_index and j <= right_index:
        if arr[i] * arr[j] <= max_element:
            ans[0] += i - left_index + 1
            j += 1
        else:
            i -= 1
    merge(left_index, right_index, left_index_of_max_element, right_index_of_max_element)


ans = [0]
n = int(input())
arr = []
ones = 0
for x in list(map(int, input().split())):
    if x != 1:
        arr.append(x)
    else:
        ones += 1
ans[0] = ones * n - ones * (ones + 1) // 2
n = len(arr)

merge_sort(0, n - 1)
print(ans[0])
```
