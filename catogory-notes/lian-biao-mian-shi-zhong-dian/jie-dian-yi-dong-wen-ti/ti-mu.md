# 题目

### **删除节点问题**

是一种特殊的节点移动问题：相当于只有**抽离**

* 只需要prev，node即可
* 注意需要delete被删除节点，防止内存泄漏

### **题目**

| 序号 | 名字 | 备注 | 难度 |
| :--- | :--- | :--- | :--- |
| 328 | 奇偶链表 | 模板样例 | 中等 |
| 86 | 分隔链表 | 同上模板 | 中等 |
| 24. | 两两交换链表中的节点 | 转换为这类问题 | 中等 |
| 82 | 删除排序链表中的重复元素 II | 转换为这类问题 | 中等 |
| 147 | 对链表进行插入排序 | 模板变体：pv指针是每次循环比较出来的 | 中等 |
| 面试题02-01 | 移除重复节点 | 三种解法（合理选择） | 技巧 |
| 143 | 重排链表 | 快慢指针 + 节点移动模板 + 反转链表 | 中等 |
|  |  |  |  |

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

* **①head为NULL或者单节点，保持不变：直接返回**
* **②头节点会被删：dummyhead**
* ③确定指针：
  * prev：前面处理的末尾（上一个数字）
  * node: 正在处理的节点
* ④处理时，**通过node-&gt;next试探**
  * 如果与node一样，prev不变，node继续
  * 如果与node不一样或者node-&gt;next为NULL
    * 根据重复flag，决定prev移动到哪

```cpp
ListNode* deleteDuplicates(ListNode* head) {
    if(!head || !head->next)return head; //1.特判
    ListNode* dummyhead = new ListNode(0); //2
    dummyhead->next = head;
    ListNode* prev, *node; //3
    prev = dummyhead;
    node = head;
    bool flag = false;
    while(node){
        if(!node->next ||(node->next->val!=node->val)){ //4
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

**147. 对链表进行插入排序**

**详见题目：**

* **这题也完完全全属于节点移动问题：pv, prev, node**
* 同样根据问题特性：判重 + dummyhead
* 需要判断自插入pv == prev的情况

关键在于**pv节点怎么获得**：**（使用插入排序的比较方式即可）**

`while(pv->next!=node&& pv->next->val < node->val )pv = pv->next;`

```cpp
ListNode* insertionSortList(ListNode* head) {
    if(!head || !head->next)return head;
    ListNode* dummyhead = new ListNode(-INT_MAX); //dummyhead设为最小
    dummyhead->next = head;
    ListNode *prev, *node;
    prev = head;
    node = head->next;
    while(node){
        ListNode* pv = dummyhead;
        while(pv->next!=node&& pv->next->val < node->val ){//node一定在pv前面,pv一定存在
            pv = pv->next;
        }
        if(pv == prev){
            prev = node;
            node = node->next;
        }else{
            prev->next = node->next;
            node->next = pv->next;
            pv->next = node;

            node = prev->next;
        }
    }
    return dummyhead->next;
}
```

**面试题 02.01. 移除重复节点**

编写代码，移除未排序链表中的重复节点。保留最开始出现的节点。

**示例:**

```text
 输入：[1, 2, 3, 3, 2, 1]
 输出：[1, 2, 3]
```

**需要注意的是它的范围：**

链表元素在\[0, 20000\]范围内。

**三种解法：（删除节点时需要delete\)**

* ①使用空间：哈希表存值，O\(NlogN）
* ②不用空间：双指针，两次循环O\(N^2\)
* **③利用取值范围**\[0, 20000\]：如果数较多可以使用，O\(N\)

```cpp
ListNode* removeDuplicateNodes(ListNode* head) {
    if(!head || !head->next)return head;
    vector<bool>bits;
    bits.resize(20001);
    ListNode* prev = head;
    ListNode* node = head->next;
    bits[head->val] = true;
    while(node){
        if(bits[node->val]){
            prev->next = node->next;
            ListNode* del = node;
            delete(del);
        }else{
            bits[node->val] = true;
            prev = node;
        }
        node = prev->next;
    }
    return head;
}
```

**143.重排链表**

 将其重新排列后变为： _L_0→_Ln_→_L_1→_Ln_-1→_L_2→_Ln_-2→…

**题解：**

* part1: 快慢指针，找中点
  * 注意：slow-&gt;next = NULL; //很关键，要把前半部分尾端删了
* part2: 反转链表：反转后半部分链表
* part3: 节点移动问题：将后半部分节点插入前半部分

```cpp
void reorderList(ListNode* head) {
    if(!head || !head->next || !head->next->next)return;
    /*part1：快慢指针，找中点*/
    ListNode* slow = head;
    ListNode* fast = head;
    while(fast && fast->next){
        slow = slow->next;
        fast = fast->next->next;
    }
    //发现好像不需要对链表长度奇偶判断，slow都要后移一位
    ListNode* prev = slow->next;//前面的slow往后移动一位，代表后半部分链表头
    slow->next = NULL; //很关键，要把尾端删了

    /*part2：反转后半部分链表*/
    ListNode* node = prev->next;
    prev->next = NULL;
    while(node){
        ListNode* nxt = node->next;
        node->next = prev;
        prev = node;
        node = nxt;
    }
    /*part3：将后半部分节点插入前半部分*/
    ListNode* pv = head;
    ListNode* nt = head->next;
    while(prev){//这里抽离的是叫做“prev"的节点，其实跟我们模型中的node一个东西
        /*临时保存下一个节点，其实代表已经抽离了*/
        node = prev->next;
        /*插入*/
        prev->next = nt;
        pv->next = prev;
        /*更新迭代*/
        pv = nt;
        nt = nt->next; //如果正确的话nt是会和prev一起为NULL的
        prev = node;
    }
}
```

