# 链表排序类

**合并有序链表：迭代+递归**

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode* dummyhead = new ListNode(0);
    ListNode* head = dummyhead;
    while(l1 && l2){
        if(l1->val <= l2->val){
            head->next = l1;
            l1 = l1->next;
        }else{
            head->next = l2;
            l2 = l2->next;
        }
        head = head->next;
    }
    if(l1)head->next = l1;
    if(l2)head->next = l2;
    ListNode* ret = dummyhead->next;
    delete(dummyhead);
    return ret;
}

ListNode* mergeTwoLists(ListNode* l1, ListNode* l2){
    if(!l1)return l2;
    if(!l2)return l1;
    ListNode* cur;
    if(l1->val < l2->val){
        cur = l1;
        l1->next = mergeTwoLists(l1->next, l2);
    }else{
        cur = l2;
        l2->next = mergeTwoLists(l1, l2->next);
    }
    return cur;
}
```

**Leetcode148,** 

**要求掌握：**

**归并排序（不需要额外空间）**

```cpp
ListNode* sortList(ListNode* head) {
    if(!head || !head->next)return head;
    cout << head->val << endl;
    ListNode* dummyhead = new ListNode(0);
    ListNode* res = dummyhead;
    ListNode* slow = head, *fast = head;
    while(fast && fast->next){
        slow = slow->next;
        fast = fast->next->next;
    }
    ListNode* mid = slow->next;
    slow->next = NULL; //cut
    ListNode* left = sortList(head);
    ListNode* right = sortList(mid);
    
    while(left && right){
        if(left->val < right->val){
            res->next = left;
            left = left->next;
        }else{
            res->next = right;
            right = right->next;
        }
        res = res->next;
    }
    res->next = left!=NULL ? left : right;
    return dummyhead->next;
}
```

  
**快速排序（不需要额外空间）**

```cpp
void sort(ListNode* left, ListNode* right){
    if(left == right)return;
    int val = left->val;
    ListNode* p0 = left;
    ListNode* p1 = left->next;
    while(p1 != right){ //目的就是Partition，将小的分在左边，大的分在右边，可以通过快慢指针来实现
        if(p1->val < val){
            p0 = p0->next;
            swap(p0->val, p1->val); //换指针，比换值的效率更高
        }
        p1 = p1->next;
    }
    swap(left->val, p0->val); //保证p0是ref_val
    sort(left, p0);
    sort(p0->next, right);
}
ListNode* sortList(ListNode* head) {
    if(!head)return NULL;
    sort(head, NULL);
    return head;
}
```

  
**Leetcode147 插入排序（不需要额外空间）**

```cpp
ListNode* sortList(ListNode* head) {
    if(!head)return nullptr;
    ListNode* dummyhead = new ListNode(0);
    ListNode* node = head->next;
    dummyhead->next = head;
    ListNode* last = head;
    while(node){
        if(last->val <= node->val){ //直到找到逆序的node，再去插入
            last = last->next;
        }else{
            ListNode* prev = dummyhead;
            while(prev->next->val <= node->val){ //prev->next遍历法
                prev = prev->next;
            }
            last->next = node->next;
            node->next = prev->next;
            prev->next = node;
        }
        node = last->next;
    }
    return dummyhead->next;
}
```

**Leetcode23. 合并K个升序链表**

**多个链表两两归并 合并成一个长链表**

```cpp
ListNode* mergeTwoLists(ListNode *a, ListNode *b) {
    if ((!a) || (!b)) return a ? a : b;
    ListNode head, *tail = &head, *aPtr = a, *bPtr = b;
    while (aPtr && bPtr) {
        if (aPtr->val < bPtr->val) {
            tail->next = aPtr; aPtr = aPtr->next;
        } else {
            tail->next = bPtr; bPtr = bPtr->next;
        }
        tail = tail->next;
    }
    tail->next = (aPtr ? aPtr : bPtr);
    return head.next;
}

ListNode* merge(vector <ListNode*> &lists, int l, int r) {
    if (l == r) return lists[l];
    if (l > r) return nullptr;
    int mid = (l + r) / 2;
    return mergeTwoLists(merge(lists, l, mid), merge(lists, mid + 1, r));
}

ListNode* mergeKLists(vector<ListNode*>& lists) {
    return merge(lists, 0, lists.size() - 1);
}
```

