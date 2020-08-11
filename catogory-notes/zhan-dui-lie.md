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
| 剑指offer 30 | 包含min函数的栈 | 单调栈辅助 | 中等 |

**剑指 Offer 09. 用两个栈实现队列**

stack1作为正向栈，stack2作为反向栈，每次更新可能一方都要全部移动到另一方

* 插入元素时，用正向栈stack1
* 删除元素时，用反向栈stack2

（代码见leetcode）

**剑指 offer 30. 包含min函数的栈**

题目：需要实现栈数据结构来保证min\(\), top\(\), pop\(\), push\(\)都是O\(1\)

**解法：建一个普通的栈，再建一个min在栈顶的单调栈作为辅助栈，单调栈的特性就是**非严格降序

![](../.gitbook/assets/minstack.png)

**单调栈**特点：就是只用记录**某时刻曾经是最小值**的这类值，不用记录每个值

假如分别push9，6，8，此时min = 6, pop\(\)后min = 6，再次pop，min = 9，

所以就没有必要记录8在单调栈里

