# 特殊技巧题目

### **快慢指针模板**

```cpp
ListNode* fast = head; //快指针
ListNode* slow = head;
while(fast && fast->next) { 
    ...
    fast = fast->next->next; //2倍慢指针的速度
    slow = slow->next;
}
```

### **题目：**

| 序号 | 名字 | 备注 | 难度 |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 52 | 两个链表的第一个公共节点 | 技巧，双指针 | 简单 |
| 142 | 环形链表-II | **快慢**指针+性质 | 中等 |
| 234 | 回文链表 | 要求空间O\(1\): 反转链表+快慢指针 | 技巧 |
| 237 | 删除链表中的节点 | 理解题意，很奇怪也很巧的题 | 技巧 |
| 445.  | 两数相加 II | 方法1：反转链表（2次） | 基础 |
|  |  | 方法2：先算出长度,prev表仅为,node表当前位 |  |

### **剑指 Offer 52. 两个链表的第一个公共节点**

输入两个链表，找出它们的第一个公共节点

**要求：O\(1\)的空间，所以不能用hash\_map**

**题解：存在一定技巧**

**先各自遍历一遍计算两个链表长度l1,l2**

然后让长度长的先走l2-l1（假设l2长），那么这两个链表

* 如果遍历到相交节点，必然是同时刚好相交node1== node2
* 如果永远没有node1 == node2，那么就必然是不相交

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        while(headA == NULL || headB == NULL)return NULL;
        int la = 0, lb = 0;
        ListNode *nodea = headA;
        while(nodea != NULL){
            la++;
            nodea = nodea->next;
        }
        ListNode *nodeb = headB;
        while(nodeb!=NULL){
            lb++;
            nodeb = nodeb->next;
        }
        if(nodea != nodeb)return NULL;
        nodea = headA;
        nodeb = headB;
        int i;
        if(la <= lb){
            i = lb - la;
            while(i--)nodeb = nodeb->next;
        }else{
            i = la - lb;
            while(i--)nodea = nodea->next;
        }
        while(nodea != NULL){
            if(nodea == nodeb)return nodea;
            nodea = nodea->next;
            nodeb = nodeb->next;
        }
        return NULL;
    }
};
```

### **142. 环形链表 II**

 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`

**题解：环形链表必然是双指针**

**但是这题需要定位出来**

**存在一个性质：**

![](../../.gitbook/assets/circularlinkedlist.png)

假如慢指针slow走了**3**步，快指针走了**6**步然后相遇了

说明快指针比慢指针多走了一圈，**一圈也就是3步**

**这时候把fast指针放置到起点，然后fast,slow第一次相遇的位置就是，环的位置**

### **234.回文链表**

**判断一个链表是否是回文链表**

```text
输入: 1->2->2->1
输出: true
```

**题解：快慢指针 + 反转链表**

**\(注意到中间，奇偶处理分情况不同）**

```cpp
bool isPalindrome(ListNode* head) {
    ListNode* fast = head; //快指针
    ListNode* slow = head, * prev = NULL; //反转链表的模板
    while(fast && fast->next) { 
        fast = fast->next->next; //2倍慢指针的速度
        ListNode* nxt = slow->next; //反转链表的模板
        slow->next = prev;
        prev = slow;
        slow = nxt;
    }
    if(fast) slow = slow->next; //奇数链表处理
    while(prev) { //开始对比
        if(prev->val != slow->val) return false;
        prev = prev->next;
        slow = slow->next;
    }
    return true;
}  
```

### **237. 删除链表中的节点**

刚看这个代码有点奇怪，不是要删除链表中一个节点吗，按理应该要传入两个参数，一个链表，一个节点，怎么只传入了一个参数，要删除的节点？

**其实就是单纯把这个节点的值用后面的值覆盖，然后把**

