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

**核心：dp\[i\] = max\(dp\[i-2\]+nums\[i-1\], dp\[i-1\]\) \(i &gt;= 2\)**

```cpp
int rob(vector<int>& nums) {
    int dp[nums.size()+1];
    memset(dp,0, sizeof(dp));
    if(nums.empty())return 0;
    dp[0] = 0;
    dp[1] = nums[0];
    for(int i = 2; i <= nums.size(); i++){
        dp[i] = max(dp[i-2] + nums[i-1], dp[i-1]);
    }
    return dp[nums.size()];
}
```

**213.打家劫舍-II**

变体： 这个地方所有的房屋都**围成一圈**

**解决：因为第一个与最后一个不可能同时打劫，所以计算**

* 打家劫舍区间\[1,n-1\] 与 打家劫舍区间\[0, n-2\] 取一个最大值

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
    pair<int,int> find_max(TreeNode* root){
        if(root==NULL)return {0,0};
        pair<int,int>l = find_max(root->left);
        pair<int,int>r = find_max(root->right);
        return {max(l.first, l.second)+max(r.first, r.second), root->val+l.first+r.first};
    }
    int rob(TreeNode* root) {
        pair<int,int>res;
        res = find_max(root);
        return max(res.first, res.second);
    }
};
```

