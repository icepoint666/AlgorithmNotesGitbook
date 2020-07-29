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

