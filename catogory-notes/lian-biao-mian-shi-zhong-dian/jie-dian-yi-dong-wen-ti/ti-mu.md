# 题目

**题目：**

| 序号 | 名字 | 备注 | 难度 |
| :--- | :--- | :--- | :--- |
| 328 | 奇偶链表 | 模板样例 | 中等 |
| 86 | 分隔链表 | 同上 | 中等 |

**86.分割链表**

**题意：**要把小于x的数移动到链表的左边，大于等于x的数移动到链表的右边

**题解**

类似于奇偶链表：

**①指针声明**

* **pv**，用来代表**小于x的数**的**链表尾端**
* **node， prev都代表正在处理的链表节点**

**②因为头部可能会变所以设置一个dummyhead方便处理**

**③考虑自插入的情况**

```cpp
ListNode* partition(ListNode* head, int x) {
    if(!head || !head->next)return head; //特判
    ListNode* pv, *prev, *node; //1.声明指针
    ListNode* dummyhead = new ListNode(0) //2.dummyhead
    dummyhead->next = head;
    //自动先讨论了一个节点
    pv = dummyhead;
    if(head->val < x)pv = head;
    prev = head;
    node = head->next;
    while(node!=NULL){
        if(node->val < x && prev != pv){ //3.防止自插入
            prev->next = node->next;
            node->next = pv->next;
            pv->next = node;

            node = prev->next;
            pv = pv->next;
        }else{
            if(node->val < x)pv = pv->next;//3.自插入讨论
            prev = node;
            node = node->next;
        }
    }
    return dummyhead->next;
}
```

