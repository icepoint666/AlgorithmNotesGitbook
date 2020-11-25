# 题目

**题目：**

| 序号 | 名字 | 备注 | 难度 |
| :--- | :--- | :--- | :--- |
| 328 | 奇偶链表 | 模板样例 | 中等 |
| 86 | 分隔链表 | 同上模板 | 中等 |
| 24. | 两两交换链表中的节点 | 转换为这类问题 | 中等 |
| 82 | 删除排序链表中的重复元素 II |  | 中等 |

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

**24.两两交换链表中的节点**

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表

**注意：空数组，以及奇数个元素的最后一个不交换**

**题解：**

**在前面加一个dummyhead，这题就转化为了对偶数位置节点进行移动，移动到它前一个节点中**

（而且也不可能自己插入自己）

**代码：（还是类似模板的思路）**

```cpp
ListNode* swapPairs(ListNode* head) {
    if(!head || !head->next)return head;
    ListNode* dummyhead = new ListNode(0);
    ListNode* pv, *prev, *node;
    dummyhead->next = head;
    pv = dummyhead;
    prev = head;
    node = head->next;
    int cnt = 0;
    while(node){
        if(cnt % 2 == 0){
            prev->next = node->next;
            node->next = pv->next;
            pv->next = node;

            pv = node;
            node = prev->next;
        }else{
            pv = prev;
            prev = node;
            node = node->next;
        }
        cnt++;
    }
    return dummyhead->next;
}
```

**82. 删除排序链表中的重复元素 II**

题意：给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 没有重复出现 的数字。

输入: 1-&gt;2-&gt;3-&gt;3-&gt;4-&gt;4-&gt;5 

输出: 1-&gt;2-&gt;5

**题解：**

* **head为NULL或者单节点，保持不变：直接返回**
* **头节点会被删：dummyhead**
* 确定指针：
  * prev：前面处理的末尾（上一个数字）
  * node: 正在处理的节点
* 处理时，**通过node-&gt;next试探**
  * 如果与node一样，prev不变，node继续
  * 如果与node不一样或者node-&gt;next为NULL
    * 根据重复flag，决定prev移动到哪

```cpp
ListNode* deleteDuplicates(ListNode* head) {
    if(!head || !head->next)return head;
    ListNode* dummyhead = new ListNode(0);
    dummyhead->next = head;
    ListNode* prev, *node;
    prev = dummyhead;
    node = head;
    bool flag = false;
    while(node){
        if(!node->next ||(node->next->val!=node->val)){ 
            if(!flag){
                prev = node;
            }else{
                prev->next = node->next;
                flag = false;
            }
        }else flag = true;
        node = node->next;
    }
    return dummyhead->next;
}
```



