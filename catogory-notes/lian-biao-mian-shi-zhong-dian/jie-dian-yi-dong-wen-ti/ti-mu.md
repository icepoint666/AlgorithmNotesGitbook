# 题目

**题目：**

| 序号 | 名字 | 备注 | 难度 |
| :--- | :--- | :--- | :--- |
| 328 | 奇偶链表 | 模板样例 | 中等 |
| 86 | 分隔链表 |  |  |

**86.分割链表**

**题意：**要把小于x的数移动到链表的左边，大于等于x的数移动到链表的右边

**题解**

类似于奇偶链表：

**指针声明**

* **pv**，用来代表**小于x的数**的**链表尾端**
* **node， prev都代表正在处理的链表节点**

**因为头部可能会变所以设置一个dummyhead方便处理**

\*\*\*\*

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

