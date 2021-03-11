# 回文字符串

**Manacher算法：马拉车算法 O\(N\)** [**https://www.cnblogs.com/grandyang/p/4475985.html**](https://www.cnblogs.com/grandyang/p/4475985.html)\*\*\*\*

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 647 | 回文子串 | 动态规划\(lc有提交bug\) | 中等 |

dp\[j\]\[i\]表示第j个字符为开头，长度为i的字符串是否是回文串

如果s\[j\] == s\[j+i-1\]，而且dp\[j+1\]\[i-2\]为true，那么dp\[j\]\[i\]就是回文串

```cpp
int countSubstrings(string s) {
    int count = 0;
    int n = s.size();
    int dp[n+1][n+1];
    for(int i = 0; i <= n; i++){
        dp[i][0] = true;
    }
    for(int i = 1; i <= n; i++){
        for(int j = 0; j + i <= n; j++){
            if(i == 1){
                dp[j][i] = true;
                count++;
            }else if(s[j] == s[j+i-1]){
                dp[j][i] = dp[j+1][i-2];
                count++;
            }
        }
    }
    return count;
}
```

