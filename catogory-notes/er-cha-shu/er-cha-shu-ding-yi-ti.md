# 二叉树定义题

**树根节点相对于leftmost高度：**

**递归**

```cpp
int height(TreeNode* root){
    if(!root)return 0;
    return height(root->left)+1;
}
```

**迭代**

```cpp
int height(TreeNode* root){
    int h = 0;
    while(root!=NULL){
        h++;
        root = root->left;
    }
    return h;
}
```

