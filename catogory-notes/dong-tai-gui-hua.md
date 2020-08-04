# 动态规划

本质：一种备忘录

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 14 | 剪绳子/分数字 | 动态规划来解决 | 有坑 |
| 300 | 最长递增子序列 | 动态规划O\(N^2\) / 二分查找O\(NlogN\) | O\(NlogN）解法记一下 |

**剑指 Offer 14- I. 剪绳子**

**分数字：**

不单纯是max的动态规划更新，注意一定要max\(dp\[j\],j\)，不加这个max，n = 4就会出问题

```cpp
class Solution {
public:
    int dp[59];
    int cuttingRope(int n) {
        memset(dp, 0, sizeof(dp));
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n; i++){
            for(int j = 1; j < n; j++){ // 保证不能j不是0，也不是n，所以确保m > 1
                dp[i] = max(dp[i], max(dp[j],j)*(i-j)); //第二个max一定要注意加
            }
        }
        return dp[n];
    }
};
```

