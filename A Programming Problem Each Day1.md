# 每日一题 Week2

## 2019.3.16 Easy
### 双指针
字符串压缩。利用字符重复出现的次数，编写一种方法，实现基本的字符串压缩功能。比如，字符串aabcccccaaa会变为a2b1c5a3。若“压缩”后的字符串没有变短，则返回原先的字符串。你可以假设字符串中只包含大小写英文字母（a至z）。
<https://leetcode-cn.com/problems/compress-string-lcci>

**示例 1：**

```
 输入："aabcccccaaa"
 输出："a2b1c5a3"

```
**示例 2：**

```
 输入："abbccd"
 输出："abbccd"
 解释："abbccd"压缩后为"a1b2c2d1"，比原字符串长度更长。
```

**注意：**

- 注意边界
- 考虑利用字符串本身的性质，尤其是在用python求解的时候。
- 时间复杂度：O(n)， 空间复杂度：O(1)

![双指针](https://github.com/AllenKaiChen/Programming-square/blob/master/imges/%E5%8F%8C%E6%8C%87%E9%92%88.gif)
### 代码 Python

```python3
class Solution:
    def compressString(self, S: str) -> str:
        l = len(S)
        res = ''
        left = 0

        while  left < l :
            right = left
            while right < l and S[left] == S[right]:
                right += 1
            res += S[left] + str(right - left)
            left = right
       
        if len(res) < l:
            return res 
        else:
            return S
```

### 代码 Python 更快的解答过程
```python3
class Solution:
    def compressString(self, S: str) -> str:
        if not S:
            return ""
        ch = S[0]
        ans = ''
        cnt = 0
        for c in S:
            if c == ch:
                cnt += 1
            else:
                ans += ch + str(cnt)
                ch = c
                cnt = 1
        ans += ch + str(cnt)
        return ans if len(ans) < len(S) else S
```
```python3
class Solution:
    def compressString(self, S: str) -> str:
        if len(S) == 0 or len(S) == 1:
            return S
        compressed = ""
        cnt = 1
        current = S[0]
        for i in range(1, len(S)):
            if current != S[i]:
                compressed += (current + str(cnt))
                current = S[i]
                cnt = 1
            else:
                cnt += 1
        compressed += (current + str(cnt))
        
        if len(compressed) >= len(S):
            return S
        return compressed
```
