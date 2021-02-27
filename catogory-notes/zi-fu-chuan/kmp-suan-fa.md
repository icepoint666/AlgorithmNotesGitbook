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
    
    int X = 0;  //影子：abcabc b的影子就是上一个b
    //生成匹配模板，也就是dp数组
    for(int i = 1; i < m; i++){ //从第二个字符开始，很关键
        for(int c = 0; c < 256; c++){
            dp[i][c] = dp[X][c];  //其他没匹配上的字符都是回到影子时的匹配量
        }
        dp[i][needle[i]] = i + 1;  //每一轮只有当前的字符可以往前说明匹配到这里
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

