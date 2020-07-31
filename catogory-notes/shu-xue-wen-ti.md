# 数学问题

leetcode内置了`math.h`库，所以不需要实现一些数学运算的函数

一些常见操作：

* pow\(a, n\)
* sqrt\(n\)
* ceil\(n\) 天花板
* floor\(n\) 地板
* int quotient = n / d;
* int reminder = n % d;

验证素数

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



**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 343 | 整数拆分 | 归纳/记忆，另外也有dp的方法 | 中等 |



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

