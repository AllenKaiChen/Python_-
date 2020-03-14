# 每日一题
## 2019.3.11 Easy
给你一个整数数组 A，只有可以将其划分为三个和相等的非空部分时才返回 true，否则返回 false。
形式上，如果可以找出索引 i+1 < j 且满足 (A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1]) 就可以将数组三等分。
链接：https://leetcode-cn.com/problems/partition-array-into-three-parts-with-equal-sum

**示例 1：**

```
输入：[0,2,1,-6,6,-7,9,1,2,0,1]
输出：true
解释：0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1
```

**示例 2：**

```
输入：[0,2,1,-6,6,7,9,-1,2,0,1]
输出：false
```

**示例 3：**

```
输入：[3,3,6,5,-2,2,5,1,-9,4]
输出：true
解释：3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4
```
**注意：**

- 利用Python解答该题目时，一般有两种思路：1.遍历整个list，一边遍历，一边累加，如果出现了一个average结果，将当前的累加和置零，结果次数加一次，接着继续累加，最后统计得到average的次数；2.利用双指针操作，分别从前后开始遍历，在这个过程中可能会遇到类似[1,-1,1,-1]的测试样例无法通过的情况，这是由于初始化的值和average相等，指针没有移动，为了解决这个问题可以考虑先做加法再去做判断，如果首位两端有一端已经和average相等，就把该节点固定住，同时节点值置零。
### 代码 Python

```python3
class Solution:
    def canThreePartsEqualSum(self, A: List[int]) -> bool:
        l = len(A)  
        s = sum(A)
        if s % 3 != 0 :
            return False
        ave = s // 3
        left = 0  
        right = l - 1
        left_sum, right_sum = 0, 0
        res = False


        while left < right - 1:
            left_sum += A[left]
            right_sum += A[right]
            if left_sum != ave:
                left += 1
            else:
                left = left
                A[left] = 0

            if right_sum != ave:
                right -= 1
            else:
                right = right
                A[right] = 0
            if left_sum == ave and right_sum == ave:
                res = True
                break
            
        return res
```
### 代码 Python

```python3
class Solution:
    def canThreePartsEqualSum(self, A: List[int]) -> bool:
        l = len(A)  
        s = sum(A)
        if s % 3 != 0 or l < 3:
            return False
        ave = s // 3
        temp = 0
        res_times = 0
        for i in range(l):
            temp += A[i]
            if temp == ave:
                temp = 0
                res_times += 1

        return res_times >= 3
```




## 2019.3.12 Easy Math Problem
对于字符串 S 和 T，只有在 S = T + ... + T（T 与自身连接 1 次或多次）时，我们才认定 “T 能除尽 S”。
返回最长字符串 X，要求满足 X 能除尽 str1 且 X 能除尽 str2。
链接：https://leetcode-cn.com/problems/greatest-common-divisor-of-strings

**示例 1：**

```
输入：str1 = "ABCABC", str2 = "ABC"
输出："ABC"
```

**示例 2：**

```
输入：str1 = "ABABAB", str2 = "ABAB"
输出："AB"
```

**示例 3：**

```
输入：str1 = "LEET", str2 = "CODE"
输出：""
解释：
```
**注意：**

- 一个性质：如果存在一个符合要求的字符串 X，那么也一定存在一个符合要求的字符串 X'，它的长度为 str1 和 str2 长度的最大公约数。
 一个性质：如果 str1 和 str2 拼接后等于 str2和 str1 拼接起来的字符串（注意拼接顺序不同），那么一定存在符合条件的字符串 X
 
### 代码 Python

```python3
class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        candidate_len = math.gcd(len(str1), len(str2))
        candidate = str1[: candidate_len]
        if candidate * (len(str1) // candidate_len) == str1 and candidate * (len(str2) // candidate_len) == str2:
            return candidate
        return ''
```
### 代码 Python

```python3
class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        candidate_len = math.gcd(len(str1), len(str2))
        candidate = str1[: candidate_len]
        if str1 + str2 == str2 + str1:
            return candidate
        return ''
```

## 2019.3.13 Easy
给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 ⌊ n/2 ⌋ sy的元素。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。
链接：https://leetcode-cn.com/problems/majority-element


**示例 1：**

```
输入：[3,2,3]
输出：3
```

**示例 2：**

```
输入：[2,2,1,1,1,2,2]
输出：2
```

**注意：**

- 考虑先排序再取中间那个元素
 
### 代码 Python

```python3
class Solution:
    def majorityElement(self, nums):
        nums.sort()
        return nums[len(nums)//2]
```


## 2019.3.14 Medium DP
给定一个无序的整数数组，找到其中最长上升子序列的长度。


**示例 1：**

```
输入：[10,9,2,5,3,7,101,18]
输出：4
解释：最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```


**注意：**

- 采用二分查找或者二分
- 采用DP时：dp[i]=max(dp[j])+1,其中0≤j<i且num[j]<num[i]，时间复杂度O(n^2),空间复杂度O(n)
 
### 代码 Python

```python3
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        if not nums:
            return 0
        dp_len = []
        #dp_len = [1] * len(nums)
        for i in range(len(nums)):
            dp_len.append(1)
            for j in range(i):
                if nums[i] > nums[j]:
                    dp_len[i] = max(dp_len[i], dp_len[j] + 1) 
        return max(dp_len)
```
