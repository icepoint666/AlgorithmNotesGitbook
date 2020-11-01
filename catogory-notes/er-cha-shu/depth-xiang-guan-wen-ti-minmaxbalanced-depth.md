# depth相关问题\(min/max/balanced depth\)

一个树的depth定义：最深处node的depth，所以depth的获取需要取最大值

```cpp
int maxDepth(TreeNode* root){
    if(root==NULL)return 0;
    return max(maxDepth(root->left), maxDepth(root->right))+1;
}
```



