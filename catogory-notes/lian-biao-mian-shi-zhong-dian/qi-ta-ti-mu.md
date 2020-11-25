# 其他题目

**题目：**

| 序号 | 名字 | 备注 | 难度 |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 52 | 两个链表的第一个公共节点 | 技巧，双指针 | 简单 |

**剑指 Offer 52. 两个链表的第一个公共节点**

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

