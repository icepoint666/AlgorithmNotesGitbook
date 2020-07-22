# 二叉树

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 437 | 路径总和Ⅲ | 二叉树遍历，双重递归 | 简单 |

**437.路径总和**

选择**偏暴力**的做法，**双重递归**，遍历二叉树，每个节点作为路径起点，然后以这个起点作为root遍历子树判断值

```cpp
class Solution {
public:
    int count(TreeNode* root, int sum){
        if(root == NULL)return 0;
        return (root->val == sum) + count(root->left, sum-root->val) + count(root->right, sum - root->val);
    }
    int pathSum(TreeNode* root, int sum) {
        if(root == NULL)return 0;
        return count(root, sum) + pathSum(root->left, sum) + pathSum(root->right, sum);
    }
};
```

