# 快速排序题目

### **题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 179 | 最大数 | 自定义排序比较条件 | 有坑 |

**179. 最大数**

```text
输入：nums = [3,30,34,5,9]
输出："9534330"
```

不要以为是字典序相关，定义**两个组合出来谁大谁小**的**偏序关系**作为比较条件

坑：\[0,0\]输入要输出"0"

```cpp
string largestNumber(vector<int>& nums) {
    sort(nums.begin(), nums.end(), [](int a, int b){
        string str_a = to_string(a);
        string str_b = to_string(b);
        return str_a+str_b > str_b+str_a;});
    string ret = "";
    for(auto&e : nums){
        ret += to_string(e);
    }
    if(ret[0] == '0')return "0";//坑：特例
    return ret;
}
```

