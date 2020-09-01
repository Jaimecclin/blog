# 39. Combination Sum
## Level - Medium
## Question
https://leetcode.com/problems/combination-sum/description/

Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

Note:

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
Example 1:
```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```
Example 2:
```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]‵
```

### Thought 1 - DFS

Refer to [MY GOD Huahua's video](https://zxi.mytechroad.com/blog/searching/leetcode-39-combination-sum/). At first I came up with a bad solution because it is quite slow. I used build-in function sum() to determine whether to end up recursion. Huahua skips this via minusing the candidiate value. Excellent!

Runtime: 52 ms, faster than 92.98% of Python3 online submissions 

#### Implementation
```python=
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        ans = []
        curAns = []
        candidates.sort()
        def dfs(start, t):
            if t == 0:
                ans.append(curAns.copy())
                return
            for i in range(start, len(candidates)):
                if candidates[i] > t:
                    break
                curAns.append(candidates[i])
                dfs(i, t-candidates[i])
                curAns.pop()
            return
        dfs(0, target)
        return ans
```