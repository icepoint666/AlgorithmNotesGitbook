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

**类型**

* 最长上升子序列，最长公共子序列
* 股票买卖，打家劫舍
* 正则表达式匹配，字符串匹配
* 回文字符串（最长回文子串，最长回文子序列）
* 01背包/完全背包

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 14 | 剪绳子/分数字 | 动态规划来解决 | 有坑 |
| 300 | 最长递增子序列 | 动态规划O\(N^2\) /耐心排序O\(NlogN\) | O\(NlogN）解法记一下 |
| 354 | 信封嵌套问题 | 最长递增子序列变种 | 记一下，O\(NlogN） |
| 44 | 正则表达式匹配 | 规则跟正则有点不一样，'\*’是匹配任意 | 困难 |
| 剑指 Offer 19/10 | 正则表达式匹配 | 因为匹配规则特性，从后往前会更容易 | 复杂（很容易错细节） |
| 剑指 Offer 46 | 数字翻译成字符串 | 动态规划基础 | 简单 |
| 剑指Offer 49 | 丑数 | 分条件动态规划 | 记一下 |
| 121.122.123, 309 | 买卖股票的最佳时机 | 三道类似，记忆状态转移方法 | 类型题 |
| 516 | 最长回文子序列 | 定义dp\[i\]\[j\]区间最优解+关键状态转移 | 记一下 |
| 5 | 最长回文子串 | 与上类似，定义dp\[i\]\[j\]表示是不是回文子串 | 同上类型 |
| \(开头的题\) | 石子合并问题 | 类似，定义dp\[i\]\[j\]是合并i到j的最小代价 | 同上类型 |
| 887 | 高楼扔鸡蛋 | 见动态规划——高楼扔鸡蛋 | 类型题 |
| 877\(+变体\) | 石子游戏 | 博弈问题（想到动态规划） | 类型题 |
| 1140 | 石子游戏-II | 需要对动态规划有更深的直觉\(逆向\) | 中等 |
| 978 | 最长湍流子数组 | 与最长上升子序列问题比较一下 | 中等 |
| 72 | 编辑距离 | 双串问题的方式定义dp\[i\]\[j\] | 中等 |
| 1143 | 最长公共子序列 | 双串问题的方式定义dp\[i\]\[j\] | 中等 |
| 322 | 零钱兑换 | 记忆化dp，纯01背包处理会超时 | 技巧 |
| 279 | 完全平方数 | 同上，类01背包的记忆化dp |  |
| 312 | 戳气球 | 区间dp | 技巧 hard |
| 221 | 最大正方形 | 二维矩阵dp O\(n^2\)/暴力O\(n^3\) | 技巧 |
| 32 | 最长有效括号 | O\(n^3\) dp会超时，有更高效的O\(n\)做法 | 技巧 hard |
| 面试题 | 送礼物 | 关键：找到状态转移方程 | 技巧hard |

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

**数学归纳：整除3最大**

\*\*\*\*

**300. 最长上升子序列**

**解法1：动态规划（存在一个容易粗心的地方）**

**dp\[i\]表示：以nums\[i\]作为子序列结尾\(最大元素\)的最长上升子序列长度**

```cpp
for(int i = 1; i <=n; ++i){
    dp[i] = 1;
    for(int j = 1; j < i; j++){
        if(nums[i-1]>nums[j-1])
            dp[i] = max(dp[i], dp[j]+1);
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

**354. 俄罗斯套娃信封问题**

**解法很巧妙：**

 先对宽度 `w` 进行升序排序，如果遇到 `w` 相同的情况，则按照高度 `h` 降序排序。之后把所有的 `h` 作为一个数组，在这个数组上计算 LIS 的长度就是答案。

**之后等于LIS问题了**，再用**耐心排序**来做

**（注意：耐心排序的比较条件 并不同于 前面排序的比较条件，相当于有两个比较条件）**



**44**

 匹配包含`'. '`和`'*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示任意字符出现任意次（含0次）

```text
输入:
s = "mississippi"
p = "mis*is*p*."
输出: true
```

**做法：**利用动态规划 **dp\[i\]\[j\] = true表示模板前i个字符与字符串前j个字符是匹配的**

**初始化：**

dp\[0\]\[0\]肯定是true

dp\[0\]\[j\]肯定都是false

如果模板前n个都是'\*'那么dp\[i\]\[0\]都是true，知道第一个不是'\*'的，后面dp\[i\]\[0\]都是false

**状态转移:**

**主要是处理'\*'的转换，另外两种都很简单**

**关键：如果第i个字符是'\*'那么dp\[i\]\[j\] = dp\[i\]\[j-1\] \|\| dp\[i-1\]\[j\];**

* 匹配1个：dp\[i-1\]\[j-1\]代表'\*'只匹配了一个的字符s\[j-1\]，前面的就不会用‘\*’继续匹配了
* 匹配更多：dp\[i\]\[j-1\]代表‘\*’匹配了一个字符s\[j-1\]，但是没有消耗掉‘\*’，还可以用'\*'继续往前匹配，所以i没有减1
* 一个也不匹配：dp\[i-1\]\[j\]代表'\*'一个都没有匹配，直接作废

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size();
        int m = p.size();
        bool dp[n+1][m+1];
        memset(dp, false, sizeof(dp));
        dp[0][0] = true;
        for(int i = 0; i < p.size(); i++){
            if(p[i] == '*'){
                dp[0][i+1] = true;
            }else{
                break;
            }
        }
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= m; j++){
                if(p[j-1] == '?')
                    dp[i][j] = dp[i-1][j-1];
                else if(p[j-1] == '*'){
                    dp[i][j] = dp[i-1][j] || dp[i][j-1];
                }else if(p[j-1] == s[i-1]){
                    dp[i][j] = dp[i-1][j-1];
                }
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

**121. 买卖股票的最佳时机——只能买卖一次 \(本质：数组中最大的数对差**\)

**状态**

有 **买入（buy）** 和 **卖出（sell）** 这两种状态。

**转移方程**

对于买来说，买之后可以卖出（进入卖状态），也可以不再进行股票交易（保持买状态）。

对于卖来说，卖出股票后不在进行股票交易（还在卖状态）。

只有在手上的钱才算钱，手上的钱购买当天的股票后相当于亏损。也就是说当天买的话意味着损失`-prices[i]`，当天卖的话意味着增加`prices[i]`，当天卖出总的收益就是 `prices[i]+buy` 。

所以我们只要考虑当天买和之前买哪个收益更高，当天卖和之前卖哪个收益更高。

* buy = min\(buy, -prices\[i\]\) 
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
            buy = max(buy, sell - prices[i]);
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

**309. 最佳买卖股票时机含冷冻期（重点）**

计算最大利润

你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 卖出股票后，你无法在第二天买入股票 \(即冷冻期为 1 天\)。 

**示例:**

```text
输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

**偏暴力：O\(N^2\)**

```cpp
int maxProfit(vector<int>& prices) {
    int n = prices.size();
    if(n == 0)return 0;
    int buys[n];
    int sells[n];
    for(int i = 0; i < n; i++){
        buys[i] = -prices[i]; //初始化表示之前没有买过，第一次在这里买的情况
        sells[i] = 0;
    }
    int res = 0;
    for(int i = 0; i < n; i++){
        for(int j = 0; j <= i; j++){
            sells[i] = max(sells[i], buys[j] + prices[i]);
        }
        for(int j = 0; j + 1 < i; j++){
            buys[i] = max(buys[i], sells[j] - prices[i]);
        }
        res = max(sells[i], res);
    }
    return res;
}
```

**优化（有难度，需要想清楚）：其实**由于我们最多只能同时买入（持有）一支股票，并且卖出股票后有冷冻期的限制，因此我们会有三种不同的状态：

* 我们目前持有一支股票，对应的「累计最大收益」记为 f\[i\]\[0\]；
* 我们目前不持有任何股票，并且处于冷冻期中，对应的「累计最大收益」记为 f\[i\]\[1\]；
* 我们目前不持有任何股票，并且不处于冷冻期中，对应的「累计最大收益」记为 f\[i\]\[2\]。

```cpp
int maxProfit(vector<int>& prices) {
    if (prices.empty()) {
        return 0;
    }

    int n = prices.size();
    // f[i][0]: 手上持有股票的最大收益
    // f[i][1]: 手上不持有股票，并且处于冷冻期中的累计最大收益
    // f[i][2]: 手上不持有股票，并且不在冷冻期中的累计最大收益
    vector<vector<int>> f(n, vector<int>(3));
    f[0][0] = -prices[0];
    for (int i = 1; i < n; ++i) {
        f[i][0] = max(f[i - 1][0], f[i - 1][2] - prices[i]);
        f[i][1] = f[i - 1][0] + prices[i];
        f[i][2] = max(f[i - 1][1], f[i - 1][2]);
    }
    return max(f[n - 1][1], f[n - 1][2]);
}
```

\*\*\*\*

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

**预处理**的时候要把j大于等于i的都标记成true 尽管长度可能为负，但是这种情况下也是有意义的

**转移方程：**

 如果 s 的第 i 个字符和第 j 个字符相同的话

f\[i\]\[j\] = f\[i + 1\]\[j - 1\]

\(**注意：**因为这个动态规划转移是dp\[i\]\[j\] = dp\[i+1\]\[j-1\] 所以i要反向 j要正向 但是j要比i大 所以i从n-2开始 j从i+1开始\)

**\(关键\)** 需要把i == j或者i &gt;j的dp表示为true，其余表示为false

转移完状态之后需要扫描一下dp为true的然后找到最长的j- i 

\(代码\)见leetcode 5

**877. 石子游戏**

**轮流取石子，每个人都是最佳选择，**返回先手和后手的最后得分（石头总数）之差

定义：

dp\[i\]\[j\]\[0\]表示先手从\[i,j\]这石头区间的得分差，更新的时候用max

dp\[i\]\[j\]\[1\]表示后手从\[i,j\]这石头区间的得分差， 更新的时候要用min，因为对方肯定是最小化你的利益

```cpp
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        int n = piles.size();
        int dp[n][n][2];
        memset(dp, 0, sizeof(dp));
        for(int i = 0; i < n; i++){
            dp[i][i][0] = piles[i];
            dp[i][i][1] = - piles[i];
        }
        for(int i = 0; i < n; i++){
            for(int j = i + 1; j < n; j++){
                dp[i][j][0] = max(dp[i][j-1][1] + piles[j], dp[i+1][j][1] + piles[i]);
                dp[i][j][1] = min(dp[i][j-1][0] - piles[j], dp[i+1][j][0] - piles[i]);
            }
        }
        if(dp[0][n-1][0] > 0)return true;
        return false;
    }
};
```

**1140. 石子游戏 II （类似于卡牌游戏的题）**

堆石子 排成一行，每堆都有正整数颗石子 piles\[i\]。游戏以谁手中的石子最多来决出胜负。

亚历克斯和李轮流进行，亚历克斯先开始。最初，M = 1。

在每个玩家的回合中，该玩家可以拿走剩下的 前 X 堆的所有石子，其中 1 &lt;= X &lt;= 2M。然后，令 M = max\(M, X\)。

**解决：动态规划，从后往前的逆向思路（要形成动态规划的逆向思路）**

**dp\[i\]\[j\]**表示前面从0~i-1还剩i堆石子没有挑选时，当前M=j\(理解M的定义\)的时候的某一方的最大获取量（对于后面部分的最大获取量）

所以**dp\[0\]\[1\]**就表示结果，已经挑选完了，M=1符合初始条件

更新：不一定是亚历克斯或者李，每次都是要从对方那里更新，所以动态转移的时候要**用sum减**：

**状态转移方程：**

```cpp
dp[i][M] = max(dp[i][M], sum - dp[i+x][max(x, M)]);
```

sum表示已经选过的从i~N-1的总数，总数 - 对方 = 我方

如果i+2\*M&gt;=size表示超过了界限

**代码：**

```cpp
class Solution {
public:
    int stoneGameII(vector<int>& piles) {
        int N = piles.size();
        int dp[N+1][N+1];
        memset(dp, 0, sizeof(dp));
        int sum = 0;
        for(int i = N - 1; i >= 0; i--){
            sum += piles[i];
            for(int M = 1; M <= N; M++){
                if(i + 2 * M >= N){
                    dp[i][M] = sum;
                    continue;
                }
                for(int x = 1; x <= 2 * M && i + x <= N; x++){
                    dp[i][M] = max(dp[i][M], sum - dp[i+x][max(x, M)]);
                }
            }
        }
        return dp[0][1];
    }
};

```

**978. 最长湍流子数组**

**注意：与前面的最长上升子序列是不太一致的，因为这里的湍流子数组是连续的定义**

**对于子数组问题如果条件是连续的，那么就不需要O\(N^2\)的复杂度，只用O\(N\)复杂度**

**最长上升子序列显然是不要求子序列在原数组中是连续存放的，所以这里复杂度需要O\(N^2\)，不过dp数组还是一维的**

```cpp
class Solution {
public:
    int maxTurbulenceSize(vector<int>& A) {
        int n = A.size();
        int dp[n+1][2];
        for(int i = 0; i <= n; i++){
            dp[i][0] = 1;
            dp[i][1] = 1;
        }
        for(int i = 2; i <= n; i++){
           dp[i][1] = A[i-1] > A[i-2] ? dp[i-1][0] + 1 : 1;
           dp[i][0] = A[i-1] < A[i-2] ? dp[i-1][1] + 1 : 1;
        }
        int res = 0;
        for(int i = 1; i <= n; i++){
            res = max(max(dp[i][0], dp[i][1]), res);
        }
        return res;
    }
};
```

**72.编辑距离**

**一个双串的动态规划问题，一般定义子问题的方式**

**dp\[i\]\[j\]表示，处理到串1的前i长度与串2的前j长度 的当前这个子问题**

**本题解法：**

dp\[i\]\[j\]表示从字符串\(前i长度\)word1转换到字符串\(前j长度\)word2需要最少多少次操作

\(其实这道题的操作虽然只有三个，但是对于状态转移相当于还有有一个skip的操作，就是不加操作数\)

状态转移方程：

```cpp
if(word1[i-1] == word2[j-1])dp[i][j] = dp[i-1][j-1];
else dp[i][j] = min(min(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1]) + 1;
```

**1143.最长公共子序列**

也是类似于上面的双串问题的动态规划方程的定义模式

直接上状态转移方程：

```cpp
if(text1[i-1] == text2[j-1])dp[i][j] = max(dp[i-1][j-1] + 1, max(dp[i][j-1], dp[i-1][j]));
else dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
```

**494.目标和**

**每一个num有两种选择 + or -，因为有一个特性：**

非负整数数组 + ****初始的数组的**和不会超过 1000** ==&gt; 暗示：01背包

有坑：S的范围可能会大于1000

```cpp
dp[i][j] += dp[i-1][j - nums[i-1]] + dp[i-1][j + nums[i-1]];
```

**322. 零钱兑换**

 计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

**示例 ：**

```text
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

**解决：关键千万不要复杂度i \* j \*k**

假设

coins = \[1, 2, 5\], amount = 11 则

F\(i\) 最小硬币数量 

* F\(0\) 0 //金额为0不能由硬币组成
* F\(1\) 1 //F\(1\)=min\(F\(1-1\),F\(1-2\),F\(1-5\)\)+1=1
* F\(2\) 1 //F\(2\)=min\(F\(2-1\),F\(2-2\),F\(2-5\)\)+1=1
* F\(3\) 2 //F\(3\)=min\(F\(3-1\),F\(3-2\),F\(3-5\)\)+1=2 
* F\(4\) 2 //F\(4\)=min\(F\(4-1\),F\(4-2\),F\(4-5\)\)+1=2
*  ... ... 
* F\(11\) 3 //F\(11\)=min\(F\(11-1\),F\(11-2\),F\(11-5\)\)+1=3

**对于所有只有dp\[i-1\]与dp\[i\]的关系，只用存一维就好了**

```cpp
int coinChange(vector<int>& coins, int amount) {
    int dp[amount+1];
    memset(dp, 0x3f, sizeof(dp));
    dp[0] = 0;
    for(int i = 1; i <= amount; i++)
        for(int j = 0; j < coins.size(); j++)
            if(i - coins[j] >= 0)
                dp[i] = min(dp[i], dp[i - coins[j]] + 1);
    if(dp[amount] > amount)return -1;
    return dp[amount];
}
```

**312. 戳气球**

```cpp
输入: [3,1,5,8]
输出: 167 
解释: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
     coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

**设dp\[i\]\[j\]为消除区间\[i,j\]气球的max硬币**

```cpp
int maxCoins(vector<int>& nums) {
    int n = nums.size();
    int dp[n][n];
    for(int i = 0; i < n; i++){
        int left = i - 1 < 0 ? 1: nums[i - 1];
        int right = i + 1 >= n ? 1: nums[i + 1];
        dp[i][i] = left * nums[i] *right;
    }
    for(int i = n - 1; i >= 0; i--){
        for(int j = i + 1; j < n; j++){
            dp[i][j] = 0;
            int left = i - 1 < 0 ? 1: nums[i - 1];
            int right = j + 1 >= n ? 1: nums[j + 1];
            for(int k = i; k <= j; k++){
                int tmp = 0;
                if(k - 1 >= i)tmp += dp[i][k-1];
                tmp += left * nums[k] * right;
                if(k + 1 <= j)tmp += dp[k+1][j];
                dp[i][j] = max(dp[i][j], tmp);
            }
        }
    }
    return dp[0][n-1];
}
```

**221. 最大正方形**

**动态规划：**

 `dp(i, j)` 是以 `matrix(i - 1, j - 1)` 为 **右下角** 的正方形的最大边长

**上，左，左上** 三者取最小+1

```cpp
if (matrix(i - 1, j - 1) == '1') {
    dp(i, j) = min(dp(i - 1, j), dp(i, j - 1), dp(i - 1, j - 1)) + 1;
}
```

![](../../.gitbook/assets/8c4bf78cf6396c40291e40c25d34ef56bd524313c2aa863f3a20c1f004f32ab0-image.png)

**暴力枚举O\(n^3\)**

见提交代码



**32. 最长有效括号**

**O\(1\)做法参考：**[**https://leetcode-cn.com/problems/longest-valid-parentheses/solution/zui-chang-you-xiao-gua-hao-by-leetcode-solution/**](https://leetcode-cn.com/problems/longest-valid-parentheses/solution/zui-chang-you-xiao-gua-hao-by-leetcode-solution/)\*\*\*\*

**dp\[i\]** 表示以下标 i 字符结尾的最长有效括号的长度

**只有两种可能的更新情况 s\[i\]是"\)"** 

**①s\[i-1\]是"\(" ②有效括号串之前的元素s\[i-s\[i-1\]-1\]是“\(”**

**满足这两种都会更新dp\[i\[**

```cpp
int longestValidParentheses(string s) {
    int maxans = 0, n = s.length();
    vector<int> dp(n, 0);
    for (int i = 1; i < n; i++) {
        if (s[i] == ')') {
            if (s[i - 1] == '(') {
                dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
            } else if (i - dp[i - 1] > 0 && s[i - dp[i - 1] - 1] == '(') {
                dp[i] = dp[i - 1] + ((i - dp[i - 1]) >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2;
            }
            maxans = max(maxans, dp[i]);
        }
    }
    return maxans;
}
```

**面试题：送礼物**

有2m个人 成 m个对 每个人手里有一个礼物 每对夫妻之间的礼物不能相互交换 其他人可以随意交换 问交换的情况数量 

答案：`dp[2n+2] = dp[2n]*(2n)*(2n-1)` 

**解释：相当于在原来2n个人时的情况下，又加入一对，这一对第一个人可以跟另外2n都配对，另外一个人跟剩下2n-1个人都可以配对**

