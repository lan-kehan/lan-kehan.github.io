# LeetCode刷题记录

## 128.Longest Consecutive Sequence

遍历某整数排列的所有子排列，求最长连续子排列的长度。
这里我们只要考虑每个数作为起点，因此保存每个数的visited状态，避免重复计算。
最重要的是python set的查询是O(1)的，然后直接搜索即可，这样就是O(n)的复杂度。

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        if not nums:
            return 0
        num_set = set(nums)
        cnt = 1
        searched = set()

        for n in num_set:
            if n in searched:
                continue
            cur = 1
            while cur + n in num_set:
                searched.add(cur + n)
                cur += 1

            cnt = max(cnt, cur)

        return cnt
```

## 89.Gray Code

这里我们发现n-1位gray code到n位gray code只要把n-1位反转一下，然后在最高位加0，1即可。

```python
# 00, 01, 11, 10
# reverse and concat -> 00, 01, 11, 10, 10, 11, 01, 00
# add 0, 1 -> 000, 001, 011, 010, 110, 111, 101, 100

class Solution:
    def grayCode(self, n: int) -> List[int]:
        def bin2dec(bin):
            return [int(b, 2) for b in bin]

        def binGrayCode(n):
            if n == 1:
                return ['0', '1']
            else:
                rep = binGrayCode(n-1)
                rep = rep + rep[::-1]
                to_add = ['0' for _ in range(pow(2, n-1))] + ['1' for _ in range(pow(2, n-1))]
                return [to_add[i] + rep[i] for i in range(len(rep))]

        return bin2dec(binGrayCode(n))
```
