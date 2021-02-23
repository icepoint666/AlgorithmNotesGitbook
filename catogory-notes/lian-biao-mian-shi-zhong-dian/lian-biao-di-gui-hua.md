# 链表递归化

**反转链表递归解决**

```cpp
ListNode* reverseList(ListNode* head) {
    if(!head || !head->next)return head;
    ListNode* p = reverseList(head->next); //递归，返回头部节点
    head->next->next = head; //最后一个节点是head->next，连接
    head->next = NULL;  //head设置为null
    return p;
}
```

