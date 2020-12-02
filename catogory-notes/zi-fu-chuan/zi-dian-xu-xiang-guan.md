# 字典序相关

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指offer 45 | 把数组排成最小的数 | 根据自定义的偏序来排序 | 中等 |
| 类似题 | 拼接最小字典序 | 同上 |  |

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

**拼接最小字典序 --字符串数组**

对于一个给定的字符串数组，请找到一种拼接顺序，使所有小字符串拼接成的大字符串是所有可能的拼接中字典序最小的

```text
输入：["abc","de"],2
输出："abcde"
```

（同上做法，自定义排序方式）

