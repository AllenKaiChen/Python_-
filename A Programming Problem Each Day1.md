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


## 2019.3.18 Easy 矩形重叠
### Math
矩形以列表 [x1, y1, x2, y2] 的形式表示，其中 (x1, y1) 为左下角的坐标，(x2, y2) 是右上角的坐标。
如果相交的面积为正，则称两矩形重叠。需要明确的是，只在角或边接触的两个矩形不构成重叠。
给出两个矩形，判断它们是否重叠并返回结果。
<https://leetcode-cn.com/problems/rectangle-overlap/>

**示例 1：**

```
输入：rec1 = [0,0,2,2], rec2 = [1,1,3,3]
输出：true

```
**示例 2：**

```
输入：rec1 = [0,0,1,1], rec2 = [1,0,2,1]
输出：false
```

**注意：**

- 时间复杂度：O(1)， 空间复杂度：O(1)
- 逆向思维：矩形如果不重叠，从x轴和y轴上看两个矩形就变成了两条线段，这两条线段肯定是不相交的，也就是说左边的矩形的最右边小于右边矩形的最左边，也就是rec1[2] < rec2[0] || rec2[2] < rec1[0]；y轴同理，下面的矩形的最上边小于上面矩形的最下边，也就是rec1[3] < rec2[1] || rec2[3] < rec1[1]。因为题目要求重叠算相离，所以加上=，最后取反。

### 代码 Python 逆向解

```python3
class Solution(object):
    def isRectangleOverlap(self, rec1, rec2):
        return not (rec1[2] <= rec2[0] or  # left
                    rec1[3] <= rec2[1] or  # bottom
                    rec1[0] >= rec2[2] or  # right
                    rec1[1] >= rec2[3])    # top
```

### 代码 Python 正向解
```python3 
class Solution:
    def isRectangleOverlap(self, rec1: List[int], rec2: List[int]) -> bool:

        w1 = rec1[2] - rec1[0]
        h1 = rec1[3] - rec1[1]
        w2 = rec2[2] - rec2[0]
        h2 = rec2[3] - rec2[1]

        if rec1[2] > rec2[2]:
            w_distance = rec1[2] - rec2[0]
        else:
            w_distance = rec2[2] - rec1[0]

        if rec1[3] > rec2[3]:
            h_distance = rec1[3] - rec2[1]
        else:
            h_distance = rec2[3] - rec1[1]

        if w_distance < (w1 + w2) and h_distance < (h1 + h2):
            return True
        else:
            return False
```



## 2019.3.19 Easy 最长回文串

给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。
在构造过程中，请注意区分大小写。比如 "Aa" 不能当做一个回文字符串。
注意:
假设字符串的长度不会超过 1010。
<https://leetcode-cn.com/problems/longest-palindrome/>

**示例 1：**

```
输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。

```

**注意：**

- 时间复杂度：O(N)
- 注意这种写法 c = collections.Counter(s)


### 代码 Python 

```python3
class Solution:
    def longestPalindrome(self, s: str) -> int:
        c = collections.Counter(s)
        res = 0
        for i in c:
            res += (c[i] // 2) * 2

        return res if res == len(s) else res + 1
```



## 2019.3.20 Easy 最小的k个数

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。
<https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/>

**示例 1：**

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]

```
**示例 2：**
```
输入：arr = [0,1,2,1], k = 1
输出：[0]

```


**注意：**

- 多种解法，以下给的第一种解法并不能让面试官满意，建议去看剑指Offer原题解法


### 代码 Python 

```python3
class Solution:
    def getLeastNumbers(self, arr: List[int], k: int) -> List[int]:
        arr.sort()
        return arr[: k]
```

## 2019.3.21 Medium 水壶问题 die hard

有两个容量分别为 x升 和 y升 的水壶以及无限多的水。请判断能否通过使用这两个水壶，从而可以得到恰好 z升 的水？
如果可以，最后请用以上水壶中的一或两个来盛放取得的 z升 水。
你允许：
装满任意一个水壶
清空任意一个水壶
从一个水壶向另外一个水壶倒水，直到装满或者倒空
<https://leetcode-cn.com/problems/water-and-jug-problem>

**示例 1：**

```
输入: x = 3, y = 5, z = 4
输出: True

```
**示例 2：**
```
输入: x = 2, y = 6, z = 5
输出: False

```


**注意：**

- 多种解法，采用深度优先搜索或者数学方法，数学方法更简单。
### Math
- 我们可以认为每次操作只会给水的总量带来 x 或者 y 的变化量。因此我们的目标可以改写成：找到一对整数 a, b使得 ax+by=z而只要满足z≤x+y，且这样的 a, b 存在，那么我们的目标就是可以达成的。这是因为：若 a≥0,b≥0，那么显然可以达成目标。
- 若 a<0，那么可以进行以下操作：往 y 壶倒水；把 y 壶的水倒入 x 壶；如果 y 壶不为空，那么 x 壶肯定是满的，把 x 壶倒空，然后再把 y 壶的水倒入 x 壶。
重复以上操作直至某一步时 x 壶进行了 a次倒空操作，y 壶进行了 b次倒水操作。若 b<0，方法同上，x 与 y 互换。

- 贝祖定理告诉我们，ax+by=z 有解当且仅当 z 是 x, y 的最大公约数的倍数。因此我们只需要找到 x, y 的最大公约数并判断 z 是否是它的倍数即可。



### 代码 Python 

```python3
class Solution:
    def canMeasureWater(self, x: int, y: int, z: int) -> bool:
        if x + y < z:
            return False
        if x == 0 or y == 0:
            return z == 0 or x + y == z
        return z % math.gcd(x, y) == 0
```





