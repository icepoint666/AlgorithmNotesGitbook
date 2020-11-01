# 路径与路径和相关问题

### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 112 | 路径之和-I |  | 简单 |
| 11**3** | 路径之和-II |  | 中等 |

**112. 路径总和**

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

```cpp
bool hasPathSum(TreeNode* root, int sum) {
    if(root==NULL)return false;
    if(!root->left && !root->right)return root->val == sum;
    return hasPathSum(root->left, sum-root->val)||hasPathSum(root->right, sum-root->val);
}
```

**113. 路径总和 II**

给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

**解法：**Tree DFS + Backtracking

```cpp
class Solution {
public:
    vector<vector<int>>res;
    void dfs(vector<int>&path, TreeNode* root, int sum){
        if(root==NULL)return;
        path.push_back(root->val);
        if(!root->left && !root->right){
            if(root->val == sum){
                res.push_back(path);
            }
            path.pop_back();
            return;
        }
        dfs(path, root->left, sum-root->val);
        dfs(path, root->right, sum-root->val);
        path.pop_back();
    }
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<int>path;
        dfs(path, root, sum);
        return res;
    }
};
```



