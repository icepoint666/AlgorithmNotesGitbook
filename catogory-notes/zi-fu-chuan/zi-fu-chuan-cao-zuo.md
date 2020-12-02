# 字符串操作

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指offer 20 | 表示数值的字符串 | 仿照json库时解析数字的操作 | 复杂 |
| 剑指offer 58 | 左旋转字符串 | 无法避免空间O\(N\), 巧的方法：余数来做 | 技巧 |

**剑指Offer20 表示数值的字符串**

首先字符串只能是两种形式A\[.\[B\]\]\[e\|EC\]//.B\[e\|EC\] 

明确一些情况： 

* A带符号的整数；
* B无符号整数， 
* 小数点前后必有数字
* e后面必有一个（有符号）整数
* 空格只能出现在字符串首尾；

**剑指 Offer 58 - II. 左旋转字符串**

```cpp
string reverseLeftWords(string s, int n) {
    string res(s);
    int len = s.size();
    for(int i = 0; i < len; i++){
        res[i] = s[(i+n)%len];
    }
    return res;
}
```

