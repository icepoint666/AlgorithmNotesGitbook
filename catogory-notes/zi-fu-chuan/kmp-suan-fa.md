# KMP算法

与C语言的strstr\(\)， C++的find\(\)功能比较近似

**题目：leetcode 28**

**关键记忆点**

1. 初始化dp
2. 影子建立以及，影子的更新
3. 匹配模板构建：dp数组更新
4. 用模板匹配实际字符串

```cpp
int strStr(string haystack, string needle) {
    int m = needle.size();
    if(!m)return 0;
    int dp[m][256];
    memset(dp,0,sizeof(dp));
    dp[0][needle[0]] = 1; //初始化
    
    int X = 0;  //影子
    //生成匹配模板，也就是dp数组
    for(int i = 1; i < m; i++){
        for(int c = 0; c < 256; c++){
            dp[i][c] = dp[X][c];
        }
        dp[i][needle[i]] = i + 1;
        X = dp[X][needle[i]]; 
    }
    int n = haystack.size();
    //匹配字符串
    int j = 0;
    for(int i = 0; i < n; i++){
        j = dp[j][haystack[i]];
        if(j == m)return i - m + 1;
    }
    return -1;
}
```

