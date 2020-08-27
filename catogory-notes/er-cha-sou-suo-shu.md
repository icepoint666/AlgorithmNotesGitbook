# 二叉搜索树

**二叉搜索树的特性：**

* 从上往下遍历BST，当前访问到的left/right子节点，一定是当前访问到的所有点中离它root值最近的两个点

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指Offer 33 | 二叉搜索树后序遍历序列 | 见单调栈 | 思考 |
| 剑指Offer 36 | 二叉搜索树与双向链表 | 递归+分治+保存头尾指针 | 时间久做出来，注意最后连接头尾 |
| 剑指Offer 68-II | 二叉搜索树的最近公共祖先 | 想到处理关键点 | 不太容易立刻想出 |

**剑指 Offer 36. 二叉搜索树与双向链表**

**题意：**要求在原来二叉树的数据结构上，直接转换为双向链表，（注意：题目最后要求，要把首尾连接起来）

**解法：分治，**每一个子树返回一个**pair&lt;Node\*,Node\*&gt;**分别表示这个子树的双向链表的最左指针 与 最右指针

```cpp
pair<Node*, Node*> treeToDL(Node* root){
    //讨论4种情况
    if(root->left==NULL && root->right==NULL)return make_pair(root, root);
    if(root->left==NULL){
        pair<Node*, Node*> r_pair = treeToDL(root->right);
        r_pair.first->left = root;
        root->right = r_pair.first;
        return make_pair(root, r_pair.second);
    }
    if(root->right==NULL){
        pair<Node*, Node*> l_pair = treeToDL(root->left);
        l_pair.second->right = root;
        root->left = l_pair.second;
        return make_pair(l_pair.first, root);
    }
    pair<Node*, Node*> l_pair = treeToDL(root->left);
    pair<Node*, Node*> r_pair = treeToDL(root->right);
    l_pair.second->right = root;
    r_pair.first->left = root;
    root->left = l_pair.second;
    root->right = r_pair.first;
    return make_pair(l_pair.first, r_pair.second);
}

Node* treeToDoublyList(Node* root) {
    if(root==NULL)return NULL;
    auto tmp = treeToDL(root);
    //记得连接首尾
    tmp.first->left = tmp.second;
    tmp.second->right = tmp.first;
    return tmp.first;
}
```

**剑指 Offer 68 - I. 二叉搜索树的最近公共祖先**

**关键点：**想到一点：如果一个大于root一个小于root那么必然，当前就是最近祖先

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(root==NULL)return NULL;
    if(root->val == p->val || root->val == q->val)return root;
    if(root->val > p->val && root->val < q->val || root->val < p->val && root->val >q->val)return root;
    if(root->val > p->val && root->val > q->val)return lowestCommonAncestor(root->left, p, q);
    else return lowestCommonAncestor(root->right, p, q);
}
```

