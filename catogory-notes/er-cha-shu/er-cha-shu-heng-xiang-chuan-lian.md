# 二叉树横向串联

**Leetcode 116 填充每个节点的下一个右侧节点指针**

**要求不能用额外空间**

**关键：串完这一层去串下一层，下一层可以利用上一层的next来移动是核心！！**

```cpp
Node* connect(Node* root) {
    if(!root)return root;
    Node* pre = root;
    while(pre->left){
        //开始每一层
        Node* tmp = pre;
        while(tmp){
            tmp->left->next = tmp->right;
            if(tmp->next){ //说明上一层已经串联了，所以可以将right串联到下一层了，关键！！
                tmp->right->next = tmp->next->left; 
            }
            tmp = tmp->next;
        }
        //开始下一层，从最左边开始
        pre = pre->left;
    }
    return root;
}
```

