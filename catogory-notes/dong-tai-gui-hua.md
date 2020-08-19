# 动态规划

本质：一种备忘录

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 14 | 剪绳子/分数字 | 动态规划来解决 | 有坑 |
| 300 | 最长递增子序列 | 动态规划O\(N^2\) /耐心排序O\(NlogN\) | O\(NlogN）解法记一下 |
| （自创） | 正则表达式匹配 | 规则跟leetcode有点不一样，'\*’是匹配任意 | 复杂 |
| 剑指 Offer 19 | 正则表达式匹配 | 因为匹配规则特性，从后往前会更容易 | 复杂（很容易错细节） |
| 剑指 Offer 46 | 数字翻译成字符串 | 动态规划基础 | 简单 |
| 剑指Offer 49 | 丑数 | 分条件动态规划 | 记一下 |

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

\*\*\*\*

**300. 最长上升子序列**

**解法1：动态规划（存在一个容易粗心的地方）**

```cpp
for(int i = 1; i <=n; ++i){
    dp[i] = 1;
    for(int j = 1; j < i; j++){
        dp[i] = max(dp[i], nums[i-1]>nums[j-1]?dp[j]+1:1);
    }
}
```

容易粗心的地方：注意返回所有dp\[i\]的最大值**（看清要返回什么）**

**解法2：**patience sorting（耐心排序）O\(NlogN\)

解法讲解：类似蜘蛛纸牌那种问题

[**https://github.com/labuladong/fucking-algorithm/blob/master/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E7%B3%BB%E5%88%97/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E8%AE%BE%E8%AE%A1%EF%BC%9A%E6%9C%80%E9%95%BF%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.md\#%E4%BA%8C%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE%E8%A7%A3%E6%B3%95**](https://github.com/labuladong/fucking-algorithm/blob/master/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E7%B3%BB%E5%88%97/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E8%AE%BE%E8%AE%A1%EF%BC%9A%E6%9C%80%E9%95%BF%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.md#%E4%BA%8C%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE%E8%A7%A3%E6%B3%95)\*\*\*\*

```cpp
int lengthOfLIS(vector<int>& nums) {
    vector<int> temp;
    int n = nums.size();
    for(int i = 0; i < n; ++i){
        bool flag = false;
        for(int j = 0; j < temp.size(); ++j){
            if(nums[i] <= temp[j]){ // 到底加不加等号，想一下极端情况全是一个数的话会怎么样
                temp[j] = nums[i];
                flag = true;
                break;
           }
       }
       if(!flag)temp.push_back(nums[i]);
    }
    return temp.size();
}
```

\*\*\*\*

**（自创）正则表达式匹配**

 匹配包含`'. '`和`'*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示任意字符出现任意次（含0次）

```text
输入:
s = "mississippi"
p = "mis*is*p*."
输出: true
```

做法：利用动态规划 **dp\[i\]\[j\] = true表示模板前i个字符与字符串前j个字符是匹配的**

**初始化：**

dp\[0\]\[0\]肯定是true

dp\[0\]\[j\]肯定都是false

如果模板前n个都是'\*'那么dp\[i\]\[0\]都是true，知道第一个不是'\*'的，后面dp\[i\]\[0\]都是false

**状态转移:**

**主要是处理'\*'的转换，另外两种都很简单**

如果第i个字符是'\*'那么dp\[i\]\[j\] = dp\[i-1\]\[j-1\] \|\| dp\[i\]\[j-1\] \|\| dp\[i-1\]\[j\];

* 匹配1个：dp\[i-1\]\[j-1\]代表'\*'只匹配了一个的字符s\[j-1\]，前面的就不会用‘\*’继续匹配了
* 匹配更多：dp\[i\]\[j-1\]代表‘\*’匹配了一个字符s\[j-1\]，但是没有消耗掉‘\*’，还可以用'\*'继续往前匹配，所以i没有减1
* 一个也不匹配：dp\[i-1\]\[j\]代表'\*'一个都没有匹配，直接作废

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();
        bool dp[n+1][m+1];
        memset(dp, false, sizeof(dp));
        dp[0][0] = true;
        for(int i = 1; i <= m; i++){
            dp[0][i] = false;
        }//可以不写
        bool inMatch = false;
        for(int i = 1; i <= n; i++){
            if(p[i-1] == '*' && !inMatch){
                dp[i][0] = true;
                continue;
            }else if(!inMatch){
                inMatch = true;
            }
            dp[i][0] = false;
        }
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <=m; j++){
                if(p[i-1] == '*')dp[i][j] = dp[i-1][j-1] || dp[i][j-1] ||dp[i-1][j];
                else if(p[i-1] == '.')dp[i][j] = dp[i-1][j-1];
                else if(p[i-1] == s[j-1])dp[i][j] = dp[i-1][j-1];
                else dp[i][j] = false;
            }
        }
        return dp[n][m];
    }
};
```

\*\*\*\*

**剑指 Offer 19. 正则表达式匹配**

 不同于上面，`'*'`表示前面的字符可以出现任意次（含0次），`'.*'`所以表示可以匹配任意字符任意次

识别特性，从前往后感觉会比较麻烦，从后往前会比较容易，因为先看到`'*'`再做判断会比较好

**直接公布代码**

```cpp
bool isMatch(string s, string p) {
    int m = s.size();
    int n = p.size();
    bool dp[n+1][m+1];
    memset(dp, false, sizeof(dp));
    dp[n][m] = true;
    bool inMatch = false;
    for(int i = n-1; i >= 0; i--){
        if(p[i] == '*' && !inMatch){
            dp[i][m] = true;
            dp[i-1][m] = true;
            i--;
            continue;
        }else if(!inMatch){
            inMatch = true;
        }
        dp[i][m] = false;
    }
    for(int i = n-1; i >= 0; i--){
        for(int j = m-1; j >= 0; j--){
            if(i < n-1 && p[i+1] == '*' && (p[i] == s[j] || p[i] == '.'))
                dp[i][j] = dp[i+2][j+1] || dp[i+2][j] ||dp[i][j+1];//表示'？*'可以匹配的三种情况
            else if(i < n-1 && p[i+1] == '*')dp[i][j] = dp[i+2][j];//意味着'？*'不匹配字符，所以跳过这个'?*'结构
            else if(p[i] == '.' || p[i] == s[j])dp[i][j] = dp[i+1][j+1];
            else dp[i][j] = false;
        }
    }
    return dp[0][0];
}
```

**剑指 Offer49：丑数**

[https://leetcode-cn.com/problems/chou-shu-lcof/solution/mian-shi-ti-49-chou-shu-dong-tai-gui-hua-qing-xi-t/](https://leetcode-cn.com/problems/chou-shu-lcof/solution/mian-shi-ti-49-chou-shu-dong-tai-gui-hua-qing-xi-t/)

