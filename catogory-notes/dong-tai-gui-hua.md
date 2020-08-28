# 动态规划

**思考方式（3步）：**

* **寻找子问题（先解决子问题，子问题本质一定要与总问题等价，定义dp）**
* **子问题如何转移成总问题（定义状态转移方程）**
* **初始化，最简单子问题定义**

**问题分解：拿石子合并问题来想**

**题意：有n长度的数组，表示某堆石子的质量，合并任意两堆需要的代价是两者之和，求将这n堆合并的最小代价**

dp\[i,j\]表示第i个石子到第j个石子合并的最小代价，dp\[1,n\]表示求解的目标

dp\[i,j\] = dp\[i,k\] + dp\[k+1,j\] + sum\[j\] - sum\[i-1\] \(sum表示前缀和）

很多类似问题暴力复杂度都是阶乘级别的，所以一般关于这类是用动态规划，优化到O\(N^2\)已经是比较满意的了

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 14 | 剪绳子/分数字 | 动态规划来解决 | 有坑 |
| 300 | 最长递增子序列 | 动态规划O\(N^2\) /耐心排序O\(NlogN\) | O\(NlogN）解法记一下 |
| （自创） | 正则表达式匹配 | 规则跟leetcode有点不一样，'\*’是匹配任意 | 复杂 |
| 剑指 Offer 19 | 正则表达式匹配 | 因为匹配规则特性，从后往前会更容易 | 复杂（很容易错细节） |
| 剑指 Offer 46 | 数字翻译成字符串 | 动态规划基础 | 简单 |
| 剑指Offer 49 | 丑数 | 分条件动态规划 | 记一下 |
| 121.122.123 | 买卖股票的最佳时机 | 三道类似，记忆状态转移方法 | 类型题 |
| 516 | 最长回文子序列 | 定义dp\[i\]\[j\]区间最优解+关键状态转移 | 记一下 |
| 5 | 最长回文子串 | 与上类似，定义dp\[i\]\[j\]表示是不是回文子串 | 同上类型 |
| \(开头的题\) | 石子合并问题 | 类似，定义dp\[i\]\[j\]是合并i到j的最小代价 | 同上类型 |

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

**剑指 Offer49. 丑数**

**121. 买卖股票的最佳时机——只能买卖一次**

**状态**

有 **买入（buy）** 和 **卖出（sell）** 这两种状态。

**转移方程**

对于买来说，买之后可以卖出（进入卖状态），也可以不再进行股票交易（保持买状态）。

对于卖来说，卖出股票后不在进行股票交易（还在卖状态）。

只有在手上的钱才算钱，手上的钱购买当天的股票后相当于亏损。也就是说当天买的话意味着损失`-prices[i]`，当天卖的话意味着增加`prices[i]`，当天卖出总的收益就是 `buy+prices[i]` 。

所以我们只要考虑当天买和之前买哪个收益更高，当天卖和之前卖哪个收益更高。

* buy = max\(buy, -price\[i\]\) （注意：根据定义 buy 是负数）
* sell = max\(sell, prices\[i\] + buy\)

**边界**

第一天 `buy = -prices[0]`, `sell = 0`，最后返回 sell 即可。

代码实现

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() <= 1)
            return 0;
        int buy = -prices[0], sell = 0;
        for(int i = 1; i < prices.size(); i++) {
            buy = max(buy, -prices[i]);
            sell = max(sell, prices[i] + buy);
        
        }
        return sell;
    }
}
```

**122. 买卖股票的最佳时机——可以买卖无数次**

**状态**

有 **买入（buy）** 和 **卖出（sell）** 这两种状态。

**转移方程**

对比上题，这里可以有无限次的买入和卖出，也就是说 **买入** 状态之前可拥有 **卖出** 状态，所以买入的转移方程需要变化。

* buy = max\(buy, sell - price\[i\]\)
* sell = max\(sell, buy + prices\[i\] \)

**边界**

第一天 `buy = -prices[0]`, `sell = 0`，最后返回 sell 即可。

#### 代码实现

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() <= 1)
            return 0;
        int buy = -prices[0], sell = 0;
        for(int i = 1; i < prices.size(); i++) {
            sell = max(sell, prices[i] + buy);
            buy = max( buy,sell - prices[i]);
        }
        return sell;
    }
};
```

**123. 买卖股票的最佳时机——可以买卖 k 次**

**题目是k=2**

**状态**

有 **第一次买入（fstBuy）** 、 **第一次卖出（fstSell）**、**第二次买入（secBuy）** 和 **第二次卖出（secSell）** 这四种状态。

**转移方程**

这里最多两次买入和两次卖出，也就是说 **买入** 状态之前可拥有 **卖出** 状态，**卖出** 状态之前可拥有 **买入** 状态，所以买入和卖出的转移方程都需要变化。

* fstBuy = max\(fstBuy ， -price\[i\]\)
* fstSell = max\(fstSell，fstBuy + prices\[i\] \)
* secBuy = max\(secBuy ，fstSell -price\[i\]\) \(受第一次卖出状态的影响\)
* secSell = max\(secSell ，secBuy + prices\[i\] \)

**边界**

* 一开始 `fstBuy = -prices[0]`
* 买入后直接卖出，`fstSell = 0`
* 买入后再卖出再买入，`secBuy - prices[0]`
* 买入后再卖出再买入再卖出，`secSell = 0`

最后返回 secSell 。

#### 代码实现\(两次）

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int fstBuy = INT_MIN, fstSell = 0;
        int secBuy = INT_MIN, secSell = 0;
        for(int i = 0; i < prices.size(); i++) {
            fstBuy = max(fstBuy, -prices[i]);
            fstSell = max(fstSell, fstBuy + prices[i]);
            secBuy = max(secBuy, fstSell -  prices[i]);
            secSell = max(secSell, secBuy +  prices[i]); 
        }
        return secSell;
        
    }
};
```

**代码实现\(k次\)**

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int k) {
        int buy[k+1];
        int sell[k+1];
        for(int i = 0; i <= k; i++){
            buy[i] = INT_MIN;
            sell[i] = 0;
        }
        for(int i = 0; i < prices.size(); i++){
            for(int j = 1; j <= k; j++){
                buy[j] = max(buy[j], sell[j-1]-prices[i]);
                sell[j] = max(sell[j], buy[j]+prices[i]);
            }
        }
        return sell[k];
    }
};
```

**516. 最长回文子序列**

**状态：**

f\[i\]\[j\] 表示 s 的第 i 个字符到第 j 个字符组成的子串中，最长的回文序列长度是多少。

**转移方程：**

 如果 s 的第 i 个字符和第 j 个字符相同的话

f\[i\]\[j\] = f\[i + 1\]\[j - 1\] + 2

**\(关键\)** 如果 s 的第 i 个字符和第 j 个字符不同的话

f\[i\]\[j\] = max\(f\[i + 1\]\[j\], f\[i\]\[j - 1\]\)

```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        int dp[n][n];
        memset(dp, 0, sizeof(dp));
        for(int i = 0; i < n; i++){
            dp[i][i] = 1;
        }
        for(int i = n - 2; i >= 0; i--){
            for(int j = i + 1; j < n; j++){
                if(s[i] == s[j]){
                    dp[i][j] = dp[i+1][j-1] + 2;
                }else{
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1]);
                }
            }
        }
        return dp[0][n-1];
    }
};
```

**5. 最长回文子串**

**状态：**

f\[i\]\[j\] 表示 s 的第 i 个字符到第 j 个字符组成的子串**是回文子串**。

**转移方程：**

 如果 s 的第 i 个字符和第 j 个字符相同的话

f\[i\]\[j\] = f\[i + 1\]\[j - 1\]

**\(关键\)** 需要把i == j或者i &gt;j的dp表示为true，其余表示为false

转移完状态之后需要扫描一下dp为true的然后找到最长的j- i 

\(代码\)见leetcode 5

