# 链表（面试重点）

**链表题一定要捋清思路，注意特判**

* 需要几个指针：node, left? right?
* 指针之间怎么更新和移动
* 怎么判定边界和终止条件**（一定要切记开头判断一下head是否为空）**

### **移动链表节点：tmp指针4步法**

**移动链表节点到指定位置的后面：取出+缝合+接头部+接尾部**

e.g.把`right->next节点`移动到`left位置`的后面，再把right缺口处缝合

```cpp
ListNode * tmp = right->next;  //取出
right->next = right->next->next; //缝合
tmp->next = left->next; //接尾部
left->next = tmp; //接头部
```

**注意：处理后并没有移动right,left的位置**

### **题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 328 | 奇偶链表 | 捋清思路 | 中等 |
| 142 | 环形链表 | 双指针 | 中等 |
| 剑指 Offer 24 | 翻转链表 | 缕清思路 | 简单 |
| 面试题 02.04 | 分割链表 | 经典：节点移动来移动去（需要总结这类规律） | 很容易乱 |
| 138 | 复杂链表的复制 | 链表里面包含random指针，怎么深拷贝，用hashmap存 | 中等/做出 |
| 剑指 Offer 52 | 两个链表的第一个公共交点 | O\(1\)的空间，所以不能用哈希表，用双指针 | 中等 |

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

**面试题 02.04. 分割链表**

**题意：**要把小于x的数移动到链表的左边，大于等于x的数移动到链表的右边

**题解：**维护两个指针

指针①：表示小于x的末尾指针

指针②：表示当前往前扫的指针

处理开头如果有特殊情况+指针初始化

* 如果开头是大于x的，要找到一个小于x的第一个节点，把它作为新的开头
  * 如果找不到直接返回head
  * 如果找到就移动这个节点到开头
* 如果开头就是小于x的，把往前扫的指针移动到第一个不是小于x的位置

然后处理正式部分

* 从当前右边的指针开始如果扫到小于x的节点，把它插入到前面部分的末尾

```cpp
ListNode* partition(ListNode* head, int x) {
    if(head==NULL)return NULL;
    ListNode* newhead = NULL;
    ListNode* right = head;
    //处理开头特殊情况以及指针初始化
    if(head->val >= x){
       while(right->next!=NULL){
           if(right->next->val < x){
               newhead = right->next;
               right->next = right->next->next;
               newhead->next = head;
               break;
           }
           right = right->next;
       }
    }else{
        while(right->next!=NULL){
            if(right->next->val < x){
                right = right->next;
            }else{
                right = right->next;
                break;
            }
        }
    }
    if(newhead == NULL)newhead = head;
    //不加这个，会漏了一种关键情况，就是本身list中就没有比x小的
    ListNode* left = newhead;
    //处理正式部分
    while(right->next!=NULL){
        if(right->next->val < x){
            //tmp指针法：4行移动节点到指定位置后面
            ListNode * tmp = right->next;
            right->next = right->next->next;
            tmp->next = left->next;
            left->next = tmp;
        }else{
            right = right->next;    
        }
    }
    return newhead;
}
```

**剑指 Offer 35. 复杂链表的复制**

**题意：深拷贝一个带有随机指针的链表**

**题解：**第一遍先循环一遍普通指针，然后建立unordered\_map&lt;Node\*, Node\*&gt;存新旧指针

第二遍更新new linked list的random指针

代码见leetcode 剑指offer35

