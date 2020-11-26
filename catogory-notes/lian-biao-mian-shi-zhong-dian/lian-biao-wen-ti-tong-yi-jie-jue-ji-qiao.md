# 链表问题统一解决技巧

### 核心思路（7点）：

**先画图，再分析下列要素**

* 确定**问题类型**
* 确定包含**哪几个行为**
* 确定要声明的**指针**，以及他变指针
* 确定**操作**
* 确定**初始设定**
* 确定**循环迭代时指针的更新**
* 确定**终止条件**

### 1.链表遍历

```text
ListNode* node = head;
while(node!=NULL){
    ...
    node = node->next;
}
```

遍历分为两种情况

* 先序遍历

### 2.dummyhead技巧

如果链表的head经过变更后，返回结果可能不再是原来的head，那么就需要**创建一个dummyhead来作为辅助**

```text
ListNode* dummyhead = new ListNode(0);
dummyhead->next = head;
...
return dummyhead->next;
```

当然**有时候**面试官可能**要求禁止使用dummyhead**，因为毕竟dummyhead使用了新的空间

这时候就需要**单独讨论head节点的情况**，**然后再进入循环**，分析的时候可能会复杂一点

### 3.特判

最开始要注意**head=null**或者**head-&gt;next=null**的情况，用不用特判

### 4. 删除节点时：如果不会在被用，要delete一下防止内存泄漏

#### \(平时可能不delete也不会报错，但在实际应用中必须delete\)

#### **delete技巧：**先创建一个临时的node再删

```cpp
ListNode* del = node;
delete(del);
```

