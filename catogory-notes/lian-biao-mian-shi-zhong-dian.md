# 链表（面试重点）

**链表题一定要捋清思路，注意特判**

* 需要几个指针：node, left? right?
* 指针之间怎么更新和移动
* 怎么判定边界和终止条件**（一定要切记开头判断一下head是否为空）**

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 328 | 奇偶链表 | 捋清思路 | 中等 |
| 142 | 环形链表 | 双指针 | 中等 |
| 剑指 Offer 24 | 翻转链表 | 缕清思路 | 简单 |
|  |  |  |  |

**328. 奇偶链表**

给定一个单链表，把所有的奇数位置节点和偶数位置节点分别排在一起**，**要求空间复杂度O\(1\)，时间O\(N\)

**捋清整个过程**

假如我有1-&gt;2-&gt;3-&gt;null

**需要几个指针**：

* odd指针：作用指向odd部分尾端，初始是节点1
* even指针：作用指向even部分尾端，初始是节点2
* node指针：指向even后面的部分，表示这一步需要处理的节点

**更新和移动：**

node = even-&gt;next node是3

* 处理even: 
  * 移动2的next节点从3到null: 将even-&gt;next更新为node-&gt;next
  * 更新even：even = even-&gt;next
* 处理odd: 
  * 移动3到1与2中间，将node-&gt;next更新为even，将odd-&gt;next更新为node
  * 更新odd：odd = odd-&gt;next

**边界条件+终止条件：**

1. 如果head为null或者head-&gt;next为null，那么只用返回head就可以了
2. 如果开始进入while正片的时候，终止条件就是even后面没有节点了+even-&gt;next没有节点了

   这个判定其实可以在前面编程中意识到

代码：

```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head == NULL || head->next == NULL)return head;
        ListNode* odd = head;
        ListNode* even = head->next;
        while(even!=NULL && even->next!=NULL){
            ListNode* node = even->next;
            even->next = node->next;
            even = even->next;
            node->next = odd->next;
            odd->next = node;
            odd = odd->next;
        }
        return head;
    }
};
```

**剑指 Offer 24 反转链表**

有条件尽量在纸上画一下，node, nxt怎么移动，以及next指针怎么更新

**反转链表需要tmp node来协助**

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL)return NULL;
        ListNode * nxt = head->next;
        ListNode * node = head;
        node->next = NULL;
        while(nxt!=NULL){
            ListNode * tmp = nxt;
            nxt = nxt->next;
            tmp->next = node;
            node = tmp;
        }
        return node;
    }
};
```

