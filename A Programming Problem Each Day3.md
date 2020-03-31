# 每日一题 Week4
### 排序数组 Medium 2019.3.31
给你一个整数数组 nums，请你将该数组升序排列
<https://leetcode-cn.com/problems/sort-an-array/>

**示例 1：**

```
输入：nums = [5,2,3,1]
输出：[1,2,3,5]

```


**注意：**

- 几种常用的排序算法以及其复杂度


### 代码 Python

```python3
class Solution:
    def merge(self, nums, l, m, r):
        help = [None] * (r - l + 1)
        i = 0
        left = l
        right = m + 1
        while left <= m and right <= r:
            if nums[left] < nums[right]:
                help[i] = nums[left]
                i += 1
                left += 1
            else:
                help[i] = nums[right]
                i += 1
                right += 1
        while left <= m:
            help[i] = nums[left]
            i += 1
            left += 1
        while right <= r:
            help[i] = nums[right]
            i += 1
            right += 1
        for j in range(len(help)):
            nums[l + j] = help[j]
        return nums


    def sortProcess(self, nums, L, R):
        if L == R:
            return
        mid = L + (R - L)//2
        self.sortProcess(nums, L, mid)
        self.sortProcess(nums, mid+1, R)
        self.merge(nums, L, mid, R)



    def sortArray(self, nums: List[int]) -> List[int]:
        if len(nums) < 2:
            return nums
        self.sortProcess(nums, 0, len(nums) - 1)
        return nums

```
