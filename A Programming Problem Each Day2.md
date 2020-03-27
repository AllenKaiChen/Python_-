# 每日一题 Week3
### 双指针2019.2.23
给定一个带有头结点 head 的非空单链表，返回链表的中间结点。
如果有两个中间结点，则返回第二个中间结点。
<https://leetcode-cn.com/problems/middle-of-the-linked-list/>

**示例 1：**

```
输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.

```
**示例 2：**

```
输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。

```

**注意：**

- 快慢指针：时间复杂度：O(N)， 空间复杂度：O(1)
- 数组方法：时间复杂度：O(N)， 空间复杂度：O(N)

### 代码 Python

```python3
class Solution:
    def middleNode(self, head: ListNode) -> ListNode:
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```
### 代码 C++

```python3
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};

```

### 代码 数组方法

```python3
class Solution:
    def middleNode(self, head: ListNode) -> ListNode:
        if head == None:
            return None
        
        l = []
        temp = head
        while temp:
            l.append(temp)
            temp = temp.next
            
        # mid = math.ceil((len(l)-1) / 2)
        return l[len(l) // 2]

```


### DP 动态规划 2019.2.24

一个有名的按摩师会收到源源不断的预约请求，每个预约都可以选择接或不接。在每次预约服务之间要有休息时间，因此她不能接受相邻的预约。给定一个预约请求序列，替按摩师找到最优的预约集合（总预约时间最长），返回总的分钟数。
<https://leetcode-cn.com/problems/the-masseuse-lcci/>

**示例 1：**

```
输入： [1,2,3,1]
输出： 4
解释： 选择 1 号预约和 3 号预约，总时长 = 1 + 3 = 4。

```
**示例 2：**

```
输入： [2,7,9,3,1]
输出： 12
解释： 选择 1 号预约、 3 号预约和 5 号预约，总时长 = 2 + 9 + 1 = 12。

```

**注意：**

- 时间复杂度：O(N)， 空间复杂度：O(1)

### 代码 Python

```python3
class Solution:
    def massage(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0 
        dp_cur = 0
        dp_pre = 0
        dp_next = 0
        for i in range(len(nums)):
            dp_next = max(nums[i] + dp_pre, dp_cur)
            dp_pre = dp_cur
            dp_cur = dp_next
        return dp_next
```


### Math Easy 三维形体的表面积 2019.2.25

在 N * N 的网格上，我们放置一些 1 * 1 * 1  的立方体。
每个值 v = grid[i][j] 表示 v 个正方体叠放在对应单元格 (i, j) 上。
请你返回最终形体的表面积。
<https://leetcode-cn.com/problems/surface-area-of-3d-shapes>

**示例 1：**

```
输入：[[2]]
输出：10

```
**示例 2：**

```
输入：[[1,2],[3,4]]
输出：34

```

**注意：**

- 时间复杂度：O(N^2)， 空间复杂度：O(1)

### 代码 Python

```python3
class Solution:
    def surfaceArea(self, grid: List[List[int]]) -> int:
        res = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] != 0:
                    res += grid[i][j] * 4 + 2
                if i > 0:
                    res -= min(grid[i][j], grid[i-1][j]) * 2
                if j > 0:
                    res -= min(grid[i][j], grid[i][j-1]) * 2
        return res
```



### 方向数组 Easy 车的可用捕获量 2019.2.26

在一个 8 x 8 的棋盘上，有一个白色车（rook)。也可能有空方块，白色的象（bishop）和黑色的卒（pawn）。它们分别以字符 “R”，“.”，“B” 和 “p” 给出。大写字符表示白棋，小写字符表示黑棋。
车按国际象棋中的规则移动：它选择四个基本方向中的一个（北，东，西和南），然后朝那个方向移动，直到它选择停止、到达棋盘的边缘或移动到同一方格来捕获该方格上颜色相反的卒。另外，车不能与其他友方（白色）象进入同一个方格。
返回车能够在一次移动中捕获到的卒的数量。
<https://leetcode-cn.com/problems/available-captures-for-rook>



**注意：**

- 注意方向数组的概念就行

### 代码 Python

```python3
class Solution:
    def numRookCaptures(self, board: List[List[str]]) -> int:
        x_s, y_s, cnt = 0, 0, 0
        for i in range(8):
            for j in range(8):
                if board[i][j] == 'R':
                    x_s, y_s = i, j
        
        dx = [1, 0, -1, 0] # 这行和下一行都是为了确定方向数组，一个i对应一个方向，总共四个方向。
        dy = [0, 1, 0, -1]
        for i in range(4):
            step = 0
            while True:
                tx = x_s + step * dx[i]
                ty = y_s + step * dy[i]
                if tx < 0 or tx >=8 or ty < 0 or ty >=8 or board[tx][ty] == 'B':
                    break
                if board[tx][ty] == 'p':
                    cnt += 1
                    break
                step += 1
        return cnt
```



### GCD 卡牌分组 2019.2.27

给定一副牌，每张牌上都写着一个整数。此时，你需要选定一个数字 X，使我们可以将整副牌按下述规则分成 1 组或更多组：
#### 每组都有X张牌。
#### 组内所有的牌上都写着相同的整数。
仅当你可选的 X >= 2 时返回 true。
<https://leetcode-cn.com/problems/x-of-a-kind-in-a-deck-of-cards>

**示例 1：**

```
输入：[1,2,3,4,4,3,2,1]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[3,3]，[4,4]

```
**示例 2：**

```
输入：[1,1,1,2,2,2,3,3]
输出：false
解释：没有满足要求的分组。

```

**注意：**

- 时间复杂度：O(NlogC)， 空间复杂度：O(N)

### 代码 Python

```python3
class Solution:
    def hasGroupsSizeX(self, deck: List[int]) -> bool:
        c = collections.Counter(deck)
        len_c = len(deck)
        min_group = len_c
        for i in c:
            min_group = min(min_group, c[i])
        if len_c == 0 or min_group == 1:
            return False
        gcd = min_group
        for j in c:
            gcd = math.gcd(c[j], gcd)
            if gcd < 2:
                return False
        return True
```

```python3
class Solution(object):
    def hasGroupsSizeX(self, deck):
        from fractions import gcd
        vals = collections.Counter(deck).values()
        return reduce(gcd, vals) >= 2

```
