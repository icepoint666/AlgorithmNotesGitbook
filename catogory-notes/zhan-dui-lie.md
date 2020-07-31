# 栈，队列

利用deque实现一个栈，先进先出

```cpp
deque<int>s;

ListNode* node = head;
while(node!=NULL){
    s.push_front(node->val); //添加
    node = node->next;
}

vector<int> res;
while(!s.empty()){
    res.push_back(s.front()); //访问
    s.pop_front();            //移出
}
```

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指offer 06 | 从尾到头打印链表 | 基本操作 | 简单 |
| 剑指offer 09 | 用两个栈实现队列 | 两个栈来实现队列 | 简单 |

**剑指 Offer 09. 用两个栈实现队列**

stack1作为正向栈，stack2作为反向栈，每次更新可能一方都要全部移动到另一方

* 插入元素时，用正向栈stack1
* 删除元素时，用反向栈stack2

（代码见leetcode）

