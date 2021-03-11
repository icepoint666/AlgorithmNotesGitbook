# 题目

**题目：**

| 序号 | 名字 | 备注 | 难度 |
| :--- | :--- | :--- | :--- |
| 61 | 旋转链表 | 穿针引线 | 中等 |
| 24 | 反转链表 | 反转链表 | 模板 |
| 92 | 反转链表 II | 穿针引线 + 反转链表 | 中等 |
| 25 | K 个一组翻转链表 | 穿针引线 + 反转链表（类似反转链表-ii做法，只不过进行了多次） | 复杂 |

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
    while(node_!=node){ /需要注意很多情况下node是
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

**25. K 个一组翻转链表**

给你一个链表，每 _k_ 个节点一组进行翻转，请你返回翻转后的链表。

**示例：**

给你这个链表：`1->2->3->4->5`

当 _k_ = 2 时，应当返回: `2->1->4->3->5`

当 _k_ = 3 时，应当返回: `3->2->1->4->5`

**解法：**

* 特判
* dummyhead
* 需要提前窥探k个看看有没有到尽头
  * 如果没有，执行一轮**穿针引线+反转链表**
    * 确定pv,nt,prev,node
    * 执行反转链表，需要`prev_`, `node_`
  * 重置初始化：每执行一轮要更新pv,nt,prev,node

**需要注意的点**：这种pv,nt,prev,node模型是允许node为NULL的，prev不能为NULL，所以判断条件需要改两个地方：①while\(prev\) ②if\(!node\)break;

```cpp
ListNode* reverseKGroup(ListNode* head, int k) {
        if(!head || !head->next || k == 1)return head;
        //初始化
        ListNode* pv, *nt, *prev, *node;
        ListNode* dummyhead = new ListNode(0);
        dummyhead->next = head;
        pv = dummyhead, prev = dummyhead;
        nt = head, node = head;
        //计数
        int cnt = 0;
        while(prev){ //prev很关键
            if(cnt == k){
                pv->next = prev;
                ListNode* cur = nt->next;
                nt->next = node;
                ListNode* p = nt;
                while(cur!=node){
                    ListNode* nxt = cur->next;
                    cur->next = p;
                    p = cur;
                    cur = nxt;
                }
                pv = nt;
                nt = node;
                prev = pv;
                cnt = 0;
            }
            if(!node)break; //配合prev来写注意！！
            prev = node;
            node = node->next;
            cnt++;
        }
        //删除节点
        ListNode* ret = dummyhead->next;
        delete(dummyhead);
        return ret;
    }
```

