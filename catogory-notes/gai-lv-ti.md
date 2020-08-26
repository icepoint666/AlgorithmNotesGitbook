# 概率题



**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 60 | n个骰子的点数 | dp储存概率 | 简单 |

**剑指 Offer 60. n个骰子的点数**

把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。  
****输入: 2 

输出: \[0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778\]

```cpp
class Solution {
public:
    vector<double> twoSum(int n) {
        double dp[n][n * 6 + 1];
        memset(dp, 0, sizeof(dp));
        for(int i = 1; i <= 6; i++)dp[0][i] = 1.0/6;
        double one[7] = {0, 1.0/6, 1.0/6, 1.0/6, 1.0/6, 1.0/6, 1.0/6};
        for(int k = 1; k < n; k++){
           for(int i = 1; i <= 6; i++){
               for(int j = 1; j <= k * 6; j++){
                    dp[k][i + j] += dp[k-1][j] * one[i];
               }
           }
        }
        vector<double>res;
        for(int i = n; i <= n*6; i++){
            res.push_back(dp[n-1][i]);
        }
        return res;
    }
};
```

