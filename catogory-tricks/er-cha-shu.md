# 二叉树

**二叉树定义与声明（模板）：**

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
 
int val = 10;
TreeNode* root = new TreeNode(val);
```

二叉树以及多叉树的遍历

```cpp
//二叉树
void traverse(TreeNode* root) {
    if (root == nullptr) return;
    //前序遍历执行操作
    traverse(root->left);
    //中序遍历执行操作
    traverse(root->right);
    //后序遍历执行操作
}

//多叉树
void traverse(TreeNode* root) {
    if (root == nullptr) return;
    for (child : root->children)
        traverse(child);
}
```



