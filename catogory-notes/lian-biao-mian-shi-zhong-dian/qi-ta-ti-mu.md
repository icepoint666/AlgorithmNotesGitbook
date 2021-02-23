# 特殊技巧题目

### **快慢指针模板** \(快指针初始化有两个版本\)

```cpp
ListNode* fast = head; //快指针第一种初始化情况
ListNode* slow = head;
while(fast && fast->next) { 
    ...
    fast = fast->next->next; //2倍慢指针的速度
    slow = slow->next;
}

ListNode* fast = head->next //快指针第二种初始化情况
ListNode* slow = head;
while(fast && fast->next) { 
    ...
    fast = fast->next->next; //2倍慢指针的速度
    slow = slow->next;
}
```

**判断奇偶可以用fast来判断**

```cpp
if(fast) slow = slow->next; //奇数链表处理
```

**（这两种初始化方式，视情况而定）**

### **题目：**

| 序号 | 名字 | 备注 | 难度 |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 52 | 两个链表的第一个公共节点 | 技巧，双指针 | 简单 |
| 142 | 环形链表-II | **快慢**指针+性质 | 中等 |
| 234 | 回文链表 | 要求空间O\(1\): 反转链表+快慢指针 | 技巧 |
| 237 | 删除链表中的节点 | 理解题意，很奇怪也很巧的题 | 技巧 |
| 445.  | 两数相加 II | 方法1：反转链表（2次） | 基础 |
|  |  | 方法2：先算出长度,prev表仅为,node表当前位 |  |
| 138 | 复制带随机指针的链表 | map | 技巧 |
| 148 | 排序链表 | 快慢指针找中点+递归 | 技巧 |

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

**其实就是单纯把这个节点的值用后面的值覆盖，然后把最后一个节点删除**

### **138. 复制带随机指针的链表**

给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。

 要求返回这个链表的 [**深拷贝**](https://baike.baidu.com/item/%E6%B7%B1%E6%8B%B7%E8%B4%9D/22785317?fr=aladdin)。

**题解：其实就是从头到尾遍历，然后重新新开一个地方建立一个链表**

**难点：**

* **创建过程中，当前节点存在指针，指向未来还没创建的节点**
* **不知道随机指针的节点位置在哪**

解法，很简单

* ①先遍历一遍创建出来一个链表
* ②然后创建一个从**原始链表每个节点**到**新链表每个节点** **地址的map**
* **③根据这个map就知道，当前节点random 指针指向的节点对应哪个新链表的节点**

```cpp
class Solution {
public:
    unordered_map <Node*, Node*> visited;
    Node* copyRandomList(Node* head) {
        if(head == NULL){
            return NULL;
        }
        if(visited.find(head)!=visited.end()){
            return visited[head];
        }
        Node *newnode = new Node(head->val);
        visited[head] = newnode;
        newnode->next = copyRandomList(head->next);
        newnode->random = copyRandomList(head->random);
        return newnode;
    }
};
```

### **148. 排序链表**

给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

进阶：在 O\(n log n\) 时间复杂度和常数级空间复杂度下，对链表进行排序

**题解：**

* 快慢指针找中点（注意这里fast的初始时fast = head-&gt;next\)
* 分治，递归

```cpp
ListNode* sortList(ListNode* head) {
    if(!head || !head->next)return head;
    ListNode* dummyhead = new ListNode(0);
    ListNode* res = dummyhead;
    ListNode* slow = head, *fast = head->next; //fast初始设定 fast = head->next这个细节非常重要
    while(fast && fast->next){
        slow = slow->next;
        fast = fast->next->next;
    }
    ListNode* mid = slow->next;
    slow->next = NULL; //cut
    ListNode* left = sortList(head);
    ListNode* right = sortList(mid);
    
    while(left && right){
        if(left->val < right->val){
            res->next = left;
            left = left->next;
        }else{
            res->next = right;
            right = right->next;
        }
        res = res->next;
    }
    res->next = left!=NULL ? left : right;
    return dummyhead->next;
} 
```



