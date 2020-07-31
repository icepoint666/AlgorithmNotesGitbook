# 动态规划

以小跳蛙为例，一次可以跳一格或者两个，问跳到n阶楼梯有几种可能跳法

* 初始条件：dp\[0\] = 1（不太严谨），dp\[1\] = 1, dp\[2\] = 2\(如果dp\[0\]确定的话，可以省略）
* 状态转移：dp\[i\] = dp\[i-1\] +dp\[i-2\]

模板：

```cpp
int numWays(int n) {
    int dp[max(n+1, 2)];
    //memset(dp, 0, sizeof(dp)); 
    // 初始化数组，其实这个问题没必要初始化
    dp[0] = 1;
    dp[1] = 1;
    for(int i = 2; i <= n; ++i){
        dp[i] = (dp[i-1] + dp[i-2])%(int)(1e9+7); //如果数过大，需要取模，因为单纯1e9+7：leetcode会报错
    }
    return dp[n];
}
```

