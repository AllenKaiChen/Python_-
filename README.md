# LeetCode and Other Code problems
## 业余Python刷题者
>438. [找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

>给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词的子串，返回这些子串的起始索引。不考虑答案输出的顺序。
>异位词 指由相同字母重排列形成的字符串（包括相同的字符串）。

```
示例 1:
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```
```
示例 2:
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```
```
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        res = []
        m, n = len(s), len(p)
        if m < n:
            return res
        p_hash = collections.Counter(p)
        left, right = 0, n - 1
        helper = collections.defaultdict(int)
        for i in range(n):
            if s[i] in p_hash:
                helper[s[i]] += 1

        while right < m-1:
            if helper == p_hash:
                res.append(left)
            if s[left] in p_hash:
                helper[s[left]] -= 1
            left += 1
            right += 1
            if s[right] in p_hash:
                helper[s[right]] += 1

        if helper == p_hash:
            res.append(left)

        return res
```


