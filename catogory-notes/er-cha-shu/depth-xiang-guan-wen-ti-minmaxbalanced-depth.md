# depth相关问题\(min/max/balanced depth\)

一个树的depth定义：最深处node的depth，所以depth的获取需要取最大值

```cpp
int maxDepth(TreeNode* root){
    if(root==NULL)return 0;
    return max(maxDepth(root->left), maxDepth(root->right))+1;
}
```



| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 111 | 二叉树的最小深度 |  | 简单 |
| 110 | 平衡二叉树 |  | 简单 |

**111.二叉树的最小深度**

使用常用的**叶子节点模板：四种情况讨论，注意判空**

```cpp
int minDepth(TreeNode* root) {
    if(root==NULL)return 0; //判空
    if(!root->left && !root->right)return 1;
    if(!root->left)return minDepth(root->right)+1;
    if(!root->right)return minDepth(root->left)+1;
    return min(minDepth(root->right), minDepth(root->left))+1;
}
```

**110. 平衡二叉树**

给定一个二叉树，判断它是否是高度平衡的二叉树。

_每个节点_ 的左右两个子树的高度差的绝对值不超过 1 

**解法：两个函数 + 一个全局flag**

```cpp
class Solution {
public:
    bool flag;
    int maxDepth(TreeNode* root){
        if(!flag)return 0;
        if(root==NULL)return 0;
        int l_depth = maxDepth(root->left);
        int r_depth = maxDepth(root->right);
        if(l_depth + 1 < r_depth || r_depth + 1 < l_depth){
            flag = false;
            return 0;
        }
        return max(l_depth, r_depth) + 1;
    }
    bool isBalanced(TreeNode* root) {
        flag = true;
        int d = maxDepth(root);
        return flag;
    }
};
```

