# 快慢双指针

**题目特征**

* 主要解决链表中的问题，比如典型的判定链表中是否包含环
* 一般都初始化指向链表的头结点 head，前进时快指针 fast 在前，慢指针 slow 在后，巧妙解决一些链表中的问题

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 141 | 环形链表 | 快慢指针 | 简单 |
| 142 | 返回链表入环的位置 | 快慢指针（找位置的脑筋急转弯） | 中等 |
| 876 | 链表的中间节点 | 快慢指针 | 简单 |
| 剑指 Offer 22 | 链表中倒数第k个节点 | 快慢指针（快指针先走k下） | 简单 |
| 234 | 回文链表 | 快慢指针 | 中等 |
| 922 | 按奇偶排序数组 | 奇偶指针 | 技巧 |

#### 题目笔记

**141. 环形链表**

判断链表是不是环形链表

**快慢指针模板**

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *fast = head;
        ListNode *slow = head;
        while(fast != NULL && slow != NULL){
            if(fast->next == NULL)return false; //注意一定要对快指针进行一下提前判断，防止下一个就是NULL
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow){
                return true;
            }
        }
        return false;
    }
};
```

**142. 环形链表②**

**关键：快慢指针相遇时，让其中任一个指针指向头节点，然后让它俩以相同速度前进，再次相遇时所在的节点位置就是环开始的位置**

**原因：**原本第一次相遇时相当于fast比slow多走了一个环的长度，但是位置是不明确的可能在环中间的某个位置

等价关系：**slow从出发到这个位置走的距离 = 环的长度**

所以，**fast,slow在环上共同走过的部分+slow环上未走的部分 = 从开始到环入口的部分 + fast,slow在环上共同走过的部分**

所以，**slow环上未走的部分 = 从开始到环入口的部分**

那么让其中任一个指针指向头节点，然后让它俩以相同速度前进，再次相遇时所在的节点位置就是环开始的位置

**876. 链表的中间结点**

利用前面的特殊判定，作为一个奇数判断

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast!=NULL && slow!=NULL){
            if(fast->next == NULL)return slow;
            fast = fast->next->next;
            slow = slow->next;
        }
        return slow;
    }
};
```

**剑指 Offer 22. 链表中倒数第k个节点**

输出倒数第k个节点，让快指针比慢指针先走k步，到终点，输出慢指针

**234. 回文链表**

用快慢双指针解决是最好的办法，能够实现O\(n\)的时间复杂度，O\(1\)的空间复杂度

操作：

* 1.首先用快指针，慢指针同时遍历，快指针到达链表
* 2.慢指针对链表的后半部分进行反转链表的处理，然后快指针不用动
* 3.这时候快指针停在相当于后半部分的反向链表的head的位置，head与快指针同时向中间逐个值比较

