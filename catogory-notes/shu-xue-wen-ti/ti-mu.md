# 题目



**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 343/剑指offer14 | 整数拆分 | 归纳/记忆，另外也有dp的方法 | 中等 |
| 剑指Offer43 | 1~n整数中1出现的次数 | 掌握规律，判断每一个位的1的个数 | 易错，overflow |
| 剑指Offer44 | 数字序列中某一位的数字 | 掌握规律，注意个位数因为有10个具有特殊性 | 中等 |
| 剑指 Offer 62 | 圆圈中最后剩下的数字 | 约瑟夫环问题 | 套路**/**记忆 |
| 29 | 两数相除 | 二分法 + 快速乘 + 负数位运算overflow的坑 | 易错 |
| 50 | pow\(x, n\) | 注意讨论 |  |
| 172 | 阶乘后的零 | 统计阶乘后有多少个末尾0 | 记忆 |
|  |  |  |  |



**343. 整数拆分**

讲正整数拆分成至少两个正整数（的加和），然后使它们的成绩最大

**解法理念：**

* 拆分出尽量多的3（应该是跟自然对数有关？自然对数e的值离3最近）
* 如果除3余1，那么就是4（也等于两个2）
* 如果除3余2，那么就是刚好一个2

```cpp
class Solution {
public:
    int integerBreak(int n) {
        if (n <= 3) {
            return n - 1;
        }
        int quotient = n / 3;
        int remainder = n % 3;
        if (remainder == 0) {
            return (int)pow(3, quotient);
        } else if (remainder == 1) {
            return (int)pow(3, quotient - 1) * 4;
        } else {
            return (int)pow(3, quotient) * 2;
        }
    }
};
```

**进阶版: n最大到1000，需要MOD=1e9+7，使用MOD\_POW + long long类型**

\*\*\*\*

**剑指 Offer 43. 1～n整数中1出现的次数**

**解法：**需要判断余数&gt;1, =1,=0三个不同情况

**一个坑：**如果n没有overflow，对于mul在最后结束前如果\*10会overflow，要注意这个

代码见leetcode

**剑指 Offer 44. 数字序列中某一位的数字**

**解法：**先求出个位，十位，百位...分界的数组\[10,10+2\*90,...\]

判断n在哪个分界，然后在算出在哪个数中，求出位数

**注意：**想想什么情况下需要longlong

代码见leetcode



**剑指 Offer 62. 圆圈中最后剩下的数字**

**约瑟夫环问题**：记忆

```cpp
int lastRemaining(int n, int m) {
    int dp[n+1];
    dp[1] = 0;
    for(int i = 2; i <=n; i++){
        dp[i] = (m + dp[i-1]) % i; 
    }
    return dp[n];
}
//方便理解：
int f(int n, int m) {
     if (n == 1) {
         return 0;
     }
     return (m + f(n-1, m)) % n; //在不考虑溢出的情况下，(a%d + c)%d == (a+c)%d
     //return (m%n + f(n-1, m)) % n;
}
int lastRemaining(int n, int m) {
     return f(n,m);
}
```

**50.pow\(x,n\)**

```cpp
double pow_(double x, long n){
    double a = 1.0;
    while(n){
        if(n & 1)a *= x;
        x *= x;
        n >>= 1;
    }
    return a;
}
double myPow(double x, int n) {
    long N = n;
    return N >= 0 ? pow_(x, N) : 1.0 / pow_(x, -N);
}
```

**172. 阶乘后的零**

 给定一个整数 _n_，返回 _n_! 结果尾数中零的数量。

**解决：**含有 2 的因子每两个出现一次，含有 5 的因子每 5 个出现一次，所有 2 出现的个数远远多于 5，换言之找到一个 5，一定能找到一个 2 与之配对。所以我们只需要找有多少个 5

 出现一个 `5`，每隔 `25` 个数，出现 `2` 个 `5`，每隔 `125` 个数，出现 `3` 个 `5`... 以此类推。

```cpp
int trailingZeroes(int n) {
    int cnt = 0;
    while(n){
        cnt += n / 5;
        n /= 5;
    }
    return cnt;
}
```

