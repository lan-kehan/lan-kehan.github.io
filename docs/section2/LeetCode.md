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

## 15.Three Sum

对于给定数组的所有三元子数组，假设子数组间两两至少有一个不同元素，求所有这样的子数组。
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

## 72.Edit Distance

定义s1和s2的编辑距离为将s1变成s2所需的最少操作数，允许的操作为插入、删除、替换一个字符。求两个字符串的编辑距离。
这里想到dp就可以，`dp[i][j]`表示s1前i个字符和s2前j个字符的编辑距离。

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        for i in range(m + 1):
            dp[i][0] = i
        for j in range(n + 1):
            dp[0][j] = j

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    dp[i][j] = 1 + min(dp[i - 1][j],
                                       dp[i][j - 1],
                                       dp[i - 1][j - 1])

        return dp[m][n]
```

## 55.Jump Game

给定一个非负整数数组`nums`，你最初位于数组的第一个位置。数组中的每个元素代表你在该位置可以跳跃的最大长度。判断你是否能够到达最后一个位置。

考虑使用双指针，维护一个最长到达的位置.

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        
        i = 0
        reach = 0
        n = len(nums)

        while i < n:
            
            if i > reach:
                return False

            reach = max(reach, nums[i] + i)
            i += 1

        return True
```

## 274.H Index

给定一个数组`citations`，其中`citations[i]`表示第`i`篇论文的引用次数。计算该数组的H指数。

直接对`[0, len(citations)]`的闭区间进行二分查找。

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        cit = sorted(citations)

        if sum(cit) == 0:
            return 0

        begin, end = 0, len(cit)
        mid = begin + (end - begin) // 2
        h = 1

        while begin <= end:
            if cit[-mid] >= mid:
                if mid > h:
                    h = mid
                begin = mid + 1
            else:
                end = mid - 1
            mid = begin + (end - begin) // 2

        return h
```

## 114.Flatten Binary Tree to Linked List

把二叉树展开成链表（左节点空，右节点指向前序遍历的下一个节点）

简单的想法是先遍历节点，存下来再连接

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def flatten(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        def tra(root):
            if not root:
                return []
            return [root]+tra(root.left)+tra(root.right)

        fl = tra(root)
        for i in range(len(fl)-1):
            fl[i].left = None
            fl[i].right = fl[i+1]
```

优化一个原地遍历的版本，且不用递归

```python
class Solution:
    def flatten(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        curr = root
        while curr:
            if curr.left:
                rightmost = curr.left
                while rightmost.right:
                    rightmost = rightmost.right
                rightmost.right = curr.right
                curr.right = curr.left
                curr.left = None
            curr = curr.right
```

## 300. Longest Increasing Subsequence

给定一个整数数组`nums`，找到其中最长递增子序列的长度。

想到dp就可以.

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [1 for _ in range(n)]
    
        for i in range(n):
            for dp_j, nums_j in zip(dp[:i][::-1], nums[:i][::-1]):
                if nums_j < nums[i] and dp_j + 1 > dp[i]:
                    dp[i] = dp_j + 1
                    
        return max(dp)
```

可以优化一个贪心 + 二分。维护最小的子序列。

```python
from bisect import bisect_left

class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        sub = []
        for num in nums:
            pos = bisect_left(sub, num)  
            if pos == len(sub):
                sub.append(num)  
            else:
                sub[pos] = num  
        return len(sub)
```
