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



## 2019.3.17 Easy 字符串
### 双指针
给你一份『词汇表』（字符串数组） words 和一张『字母表』（字符串） chars。
假如你可以用 chars 中的『字母』（字符）拼写出 words 中的某个『单词』（字符串），那么我们就认为你掌握了这个单词。
注意：每次拼写时，chars 中的每个字母都只能用一次。
返回词汇表 words 中你掌握的所有单词的 长度之和。
<https://leetcode-cn.com/problems/find-words-that-can-be-formed-by-characters>

**示例 1：**

```
 输入：words = ["cat","bt","hat","tree"], chars = "atach"
 输出：6
 解释： 可以形成字符串 "cat" 和 "hat"，所以答案是 3 + 3 = 6。

```
**示例 2：**

```
输入：words = ["hello","world","leetcode"], chars = "welldonehoneyr"
输出：10
解释：可以形成字符串 "hello" 和 "world"，所以答案是 5 + 5 = 10。
```

**注意：**

- 时间复杂度：O(n)， 空间复杂度：O(S)
- 对于一个单词 word，只要其中的每个字母的数量都不大于 chars 中对应的字母的数量，那么就可以用 chars 中的字母拼写出 word


### 代码 Python

```python3
class Solution:
    def countCharacters(self, words: List[str], chars: str) -> int:
        res = 0

        for w in words:
            for i in w:
                if w.count(i)<=chars.count(i):
                    indcator = True
                else:
                    indcator = False
                    break
            if indcator == True:
                res += len(w)
        return res
```

### 代码 Python for-else语句
```python3 
class Solution:
    def countCharacters(self, words: List[str], chars: str) -> int:
        res = 0
        for word in words:
            for w in word:
                if word.count(w) > chars.count(w):
                    break
            else:
                res += len(word)

        return res
```
### 代码 Python Hash
```python3 
class Solution:
    def countCharacters(self, words: List[str], chars: str) -> int:
        ans = 0
        cnt = collections.Counter(chars)
        for w in words:
            c = collections.Counter(w)
            if all([c[i] <= cnt[i] for i in c]):
                ans += len(w)
        return ans

```
### 代码 C++
```C++
class Solution 
{
public:
    int countCharacters(vector<string>& words, string chars) 
    {
        map<char, int> char_s;
        for(char a : chars)
            ++char_s[a];
        int ans = 0;
        for(string s : words)
        {
            map<char, int> word_s;
            bool flag = true;
            for(char a : s)
            {
                ++word_s[a];
                if(char_s[a] < word_s[a])
                {
                    flag = false;
                    break;
                }
            }
            if(flag)
                ans += s.size();
        }
        return ans;
    }
};
```
