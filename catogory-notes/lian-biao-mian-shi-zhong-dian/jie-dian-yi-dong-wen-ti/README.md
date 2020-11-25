# 节点移动问题



**移动链表节点到指定位置的后面：\(remove + add）取出+缝合+接头部+接尾部\(4步法\)**

e.g.把`right->next节点`移动到`left位置`的后面，再把right缺口处缝合

```cpp
ListNode * tmp = right->next;  //取出
right->next = right->next->next; //缝合
tmp->next = left->next; //接尾部
left->next = tmp; //接头部
```

