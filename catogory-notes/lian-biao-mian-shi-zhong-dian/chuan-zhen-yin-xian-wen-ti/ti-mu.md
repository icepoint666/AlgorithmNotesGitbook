# 题目

**题目：**

| 序号 | 名字 | 备注 | 难度 |
| :--- | :--- | :--- | :--- |
| 61 | 旋转链表 | 穿针引线 | 中等 |
| 92 | 反转链表 II | 变体 | 中等 |

**92. 反转链表 II**

反转从位置 _m_ 到 _n_ 的链表。请使用一趟扫描完成反转。

**说明:**  
1 ≤ _m_ ≤ _n_ ≤ 链表长度。

**示例:**

```text
输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL
```

* 特判 :注意加上,m == n的情况
* 开头可能会反转，加dummyhead
* **穿针引线问题**，找到pv, nt, prev, node（找的过程中注意边界条件的判断）
* **反转链表问题**：把中间部分套用反转链表模板，完成反转

```cpp
ListNode* reverseBetween(ListNode* head, int m, int n) {
    if(!head || !head->next ||m == n)return head; //m == n加入特判
    ListNode* dummyhead = new ListNode(0);
    dummyhead->next = head;
    ListNode* pv, *nt, *prev, *node;
    pv = dummyhead;
    nt = head;
    int cnt = 1;
    while(cnt < m){
        pv = nt;
        nt = nt->next;
        cnt++;
    }
    prev = pv;
    node = nt;
    while(cnt <= n){ //这里的边界要注意 前面是小于，这里是小于等于
        prev = node;
        node = node->next;
        cnt++;
    }
    /*找到了pv,nt,prev,node*，就完全变成了一个反转链表模板题*/
    ListNode* prev_ = nt;
    ListNode* node_ = nt->next;
    prev_->next = node; //穿针引线1
    while(node_!=node){
        ListNode* nxt = node_->next;
        node_->next = prev_;
        prev_ = node_;
        node_ = nxt;
    }
    pv->next = prev; //穿针引线2
    return dummyhead->next;
}
```

**（可以再优化一下，就是改成只用一次循环，这里没有写出来）**

