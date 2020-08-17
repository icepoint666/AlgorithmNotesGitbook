# 字符串操作

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指offer 05 | 替换空格 | 基本操作 | 简单 |
| 剑指offer 45 | 把数组排成最小的数 | 对数 | 中等 |
| 剑指 Offer 20 |  表示数值的字符串 | 关键在于列出所有情况 | 恶心 |

**剑指 Offer 45. 把数组排成最小的数**

```text
输入: [3,30,34,5,9]
输出: "3033459"
```

题意就是使输出的数值最小

**有一个很诡异的地方：**a.append\(b\) &lt; b.append\(a\)处理一个全是0的样例的时候就会overflow， a+b&lt;b+a就不会

```cpp
static bool comp(string a, string b){
    return a + b < b + a; // a.append(b) < b.append(a);本题会报错
}

string minNumber(vector<int>& nums) {
    vector<string>nums_str;
    for(auto& e:nums){
        nums_str.push_back(to_string(e));
    }
    sort(nums_str.begin(), nums_str.end(), comp);
    //sort(nums_str.begin(), nums_str.end(), [](string&s1, string&s2){return s1+s2<s2+s1;})
    string res;
    for(auto& str: nums_str){
        res.append(str);
    }
    return res;
}
```

**剑指Offer20 表示数值的字符串**

首先字符串只能是两种形式A\[.\[B\]\]\[e\|EC\]//.B\[e\|EC\] 

明确一些情况： 

* A带符号的整数；
* B无符号整数， 
* 小数点前后必有数字
* e后面必有一个（有符号）整数
* 空格只能出现在字符串首尾；

