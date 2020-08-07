# 数学问题

leetcode内置了`math.h`库，所以不需要实现一些数学运算的函数

### 一些常见操作：

* pow\(a, n\)
* sqrt\(n\)
* ceil\(n\) 天花板
* floor\(n\) 地板
* int quotient = n / d;
* int reminder = n % d;

### 验证素数

```cpp
bool isPrime(int n){
    if(n <= 1)return false;
    bool res = true;
    for(int i = 2; i <= (int)sqrt(n); ++i){
        if(n % i == 0)return false;
    }
    return true;
}
```

### **MOD的处理**

有时候数字过大，可能会overflow，为了防止这种情况，需要将结果模MOD

但是如果模的数很接近1e9+7，这时候再乘3，很容易会overflow，因为int overflow范围也就比2e9多一点

这时候需要转换为**long long类型**

**long long类型 + MOD版本的普通pow：**

```cpp
long long MOD = 1e9+7;
int mod_pow(int a, int b){
    long long res = 1;
    while(b--){
        res = (res * a) % MOD;
    }
    return (int)res;
}
```

### **快速幂**

```cpp
int qpow(int a, int n){
    int ans = 1;
    while(n){
        if(n&1)        //如果n的当前末位为1
            ans *= a;  //ans乘上当前的a
        a *= a;        //a自乘
        n >>= 1;       //n往右移一位
    }
    return ans;
}
```

### **负数位运算 与 overflow的坑**

注意负数的位运算，并不会考虑前面的“负位”

例如：-32&gt;&gt;1 = -16

但是注意**-1是不能在右移** -1&gt;&gt;1 = -1

注意：MIN\_INT = -\(1&lt;&lt;31\)，如果它取反会overflow，因为范围是 -\(1&lt;&lt;31\) ~\(1 &lt;&lt;31-1\)，注意取反的话转成**long类型**就可以了



**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 343/剑指offer14 | 整数拆分 | 归纳/记忆，另外也有dp的方法 | 中等 |



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

