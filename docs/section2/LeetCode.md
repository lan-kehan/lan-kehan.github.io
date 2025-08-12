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

## 15. 3Sum

最初的想法是正、负、零分成三类，分类讨论。

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        negs = sorted(e for e in nums if e < 0)[::-1]
        zeros = [e for e in nums if e == 0]
        posts = sorted(e for e in nums if e > 0)

        res = set()

        # case: 包含0
        if zeros:
            i, j = 0, 0
            while i < len(posts) and j < len(negs):
                s = posts[i] + negs[j]
                if s == 0:
                    res.add((posts[i], negs[j], 0))
                    i += 1
                    j += 1
                elif s > 0:
                    j += 1
                else:
                    i += 1
            if len(zeros) >= 3:
                res.add((0, 0, 0))

        # case: 两负一正
        def two_sum(arr, target, fixed):
            l, r = 0, len(arr) - 1
            while l < r:
                s = arr[l] + arr[r]
                if s == target:
                    res.add((fixed, arr[l], arr[r]))
                    l += 1
                elif s > target:
                    l += 1 if arr is negs else r -= 1
                else:
                    r -= 1 if arr is negs else l += 1

        for p in posts:
            two_sum(negs, -p, p)
        for n in negs:
            two_sum(posts, -n, n)

        return [list(t) for t in res]
```

然后发现这样讨论是多余的，完全等价于直接固定一个数，剩下的两个数用双指针即可。

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = set()
        for i in range(len(nums) - 2):
            if i > 0 and nums[i] == nums[i - 1]:
                continue  
            l, r = i + 1, len(nums) - 1
            while l < r:
                s = nums[i] + nums[l] + nums[r]
                if s == 0:
                    res.add((nums[i], nums[l], nums[r]))
                    l += 1
                    r -= 1
                    while l < r and nums[l] == nums[l - 1]:  
                        l += 1
                    while l < r and nums[r] == nums[r + 1]:
                        r -= 1
                elif s < 0:
                    l += 1
                else:
                    r -= 1
        return [list(t) for t in res]
```
