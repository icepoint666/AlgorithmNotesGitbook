# 技巧：int数可以代替一个长度为26的bool标记数组

题目：实现一个算法，确定一个字符串 s 的所有字符是否全都不同。

示例 1：

输入: s = "leetcode" 输出: false 示例 2：

输入: s = "abc" 输出: true

```cpp
bool isUnique(string astr) {
    int mark = 0; //int本身是有32位的，可以代替26位的bool数组
    for(auto&c: astr){
        int bit = c - 'a';
        if(mark & (1 << bit))return false;
        mark |= (1 << bit);
    }
    return true;
}
```

