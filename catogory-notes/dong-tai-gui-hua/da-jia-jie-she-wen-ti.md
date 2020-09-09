# 打家劫舍问题

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 198 | 打家劫舍 | 动态规划 |  |
| 213 | 打家劫舍-II |  |  |
| 337 | 打家劫舍-III | 二叉树回溯 |  |

**198.打家劫舍**

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        int dp[n+1][2];
        memset(dp, 0, sizeof(dp));
        for(int i = 1;i <= n; i++){
            dp[i][0] = max(dp[i-1][0], dp[i-1][1]);
            dp[i][1] = dp[i-1][0] + nums[i-1];
        }
        return max(dp[n][0], dp[n][1]);
    }
};
```

**213.打家劫舍-II**

变体： 这个地方所有的房屋都**围成一圈**

**解决：正向一次，反向一次，（因为正向方向第一个要选的都会直接选择偷，那么最后一个就不会被选）取两种情况的最大值，注意讨论特殊情况**

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if(n == 0)return 0;
        if(n == 1)return nums[0];
        int dp[n+1][2];
        memset(dp, 0, sizeof(dp));
        for(int i = 1; i <= n; i++){
            if(i == n)continue;
            dp[i][0] = max(dp[i-1][0], dp[i-1][1]);
            dp[i][1] = dp[i-1][0] + nums[i-1];
        }
        int value = max(dp[n-1][0], dp[n-1][1]);
        memset(dp, 0, sizeof(dp));
        for(int i = n-1; i >= 0; i--){
            if(i == 0)continue;
            dp[i][0] = max(dp[i+1][0], dp[i+1][1]);
            dp[i][1] = dp[i+1][0] + nums[i];
        }
        value = max(max(dp[1][0], dp[1][1]), value);
        return value;
    }
};
```

**337.打家劫舍-III**

如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

```cpp
class Solution {
public:
    pair<int,int>visit(TreeNode* root){
        pair<int,int> l;
        pair<int,int> r;
        if(root->left == NULL && root->right == NULL){
            return make_pair(root->val, 0);
        }
        if(root->left == NULL){
            r = visit(root->right);
            return make_pair(r.second + root->val, max(r.second, r.first));
        }
        if(root->right == NULL){
            l = visit(root->left);
            return make_pair(l.second + root->val, max(l.second, l.first));
        }
        l = visit(root->left);
        r = visit(root->right);
        return make_pair(l.second + r.second + root->val, max(r.second, r.first) + max(l.second, l.first));
    }
    int rob(TreeNode* root) {
        if(root==NULL)return 0;
        auto res = visit(root);
        return max(res.first, res.second);
    }
};
```

