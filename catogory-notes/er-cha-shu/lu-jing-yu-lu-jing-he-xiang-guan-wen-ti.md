# 路径与路径和相关问题

### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 112 | 路径之和-I |  | 简单 |
| 113 | 路径之和-II | dfs+backtracing | 中等 |
| 124 | 二叉树的最大路径和 | 思路清晰 | 困难/一遍A |
| 129 | 求根到叶子节点数字之和 | dfs+backtracing | 简单 |
| 298 | 二叉树最长连续序列 | dfs | 中等 |
| Facebook面试题 | 路径重排序后字典序比较大小 |  | 待解决 |
|  |  |  |  |

**112. 路径总和**

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

```cpp
bool hasPathSum(TreeNode* root, int sum) {
    if(root==NULL)return false;
    if(!root->left && !root->right)return root->val == sum;
    return hasPathSum(root->left, sum-root->val)||hasPathSum(root->right, sum-root->val);
}
```

**113. 路径总和 II（等价于面试题 04.12. 求和路径）**

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

**124. 二叉树中的最大路径和**

**最核心点：后序遍历**

**后续遍历归结的时候，需要考虑两个问题：**

**①返回值返回什么：**返回**包含当前root的路径**的最大向上路径和 m（不包含折返）

 因为路径至少一个节点，所以m只可能是**root-&gt;val**和**子树的返回值+root-&gt;val**的最大值

**②包含当前root的路径**去**更新最大路径和**可能的几种情况

* **包含当前root的路径**的最大向上路径和 m
  * 其中其实蕴含了①中三种情况\(取最大值）
    * root-&gt;val
    * root-&gt;val + root-&gt;left返回值
    * root-&gt;val + root-&gt;right返回值
* **经过当前root的折返路径和**
  * 这个折返路径和最大值必然等于 root-&gt;left返回值 + root-&gt;val + root-&gt;right返回值

根据**有无子树模板**的**四种情况讨论**来解决

```cpp
class Solution {
public:
    int maxsum;
    int findMax(TreeNode* root){
        if(!root->left && !root->right){
            maxsum = max(maxsum, root->val);
            return root->val;
        }
        if(!root->left){
            int m = max(root->val, findMax(root->right)+root->val);
            maxsum = max(maxsum, m);
            return m;
        }
        if(!root->right){
            int m = max(root->val, findMax(root->left)+root->val);
            maxsum = max(maxsum, m);
            return m;
        }
        int l = findMax(root->left);
        int r = findMax(root->right);
        int m = max(max(l+root->val, r+root->val),root->val);
        maxsum = max(maxsum, m);
        maxsum = max(maxsum, l+r+root->val);
        return m;
    }
    int maxPathSum(TreeNode* root) {
        maxsum = -0x3f3f3f3f;
        int _ = findMax(root);
        return maxsum;
    }
};
```

**129. 求根到叶子节点数字之和**

**dfs+backtracing**

```cpp
class Solution {
public:
    int dfs(TreeNode* root, int num){
        if(root == NULL) return 0; // Prevent root is NULL

        if(!root->left && !root->right){ //leaf node
            return num + root->val;
        }

        int left = dfs(root->left, (num + root->val) * 10);
        int right = dfs(root->right, (num + root->val) * 10);

        return left + right;
    }
    int sumNumbers(TreeNode* root) {
        return dfs(root, 0);
    }
};
```

**298.二叉树最长连续序列**

意思：就是找到连续上升或者下降的序列，必须是父节点到子节点方向（不能折返）

For example,

```text
   1
    \
     3
    / \
   2   4
        \
         5
```

Longest consecutive sequence path is `3-4-5`, so return `3`.

java代码

```java
public class Solution {
    private int max = 0;//global var
    public int longestConsecutive(TreeNode root) {
        if(root==null)return 0;
        helper(root,0,root.val);//node,len,last value
        return max;
    }
    public void helper(TreeNode root,int len,int val){
        if(root==null)return;
        if(root.val == val+1)len++;
        else len = 1;
        max = Math.max(max,len);
        helper(root.left,len,root.val);
        helper(root.right,len,root.val);
    }
}
```

**Facebook面试题**

**给定一个二叉树，要求把所有从根节点到叶子结点的路径，升序排序后组合成字符串，并且对这些路径字符串进行字典序排序，找出最小的路径 时间要求线性的**

 **示例如下**

```java
    5
   / \
  10  3
 / \   \
1   7   8

路径：5-10-1 5-10-7 5-3-8
排序后：1-5-10 5-7-10 3-5-8
按字符串类型排序 为 1-5-10 < 3-5-8 < 5-7-10
return 1-5-10
```

**解法：待解决**

