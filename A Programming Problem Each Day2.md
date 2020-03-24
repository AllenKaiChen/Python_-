# 每日一题 Week3
### 双指针
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


### DP 动态规划

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
- 

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
