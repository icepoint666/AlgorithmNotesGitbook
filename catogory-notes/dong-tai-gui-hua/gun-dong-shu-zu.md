# 滚动数组

**使用背景：对于dp公式 dp\[i\] = dp\[i-1\] + dp\[i\]**

**因为后面的i+1，也需要重用dp\[i\]，所以如果这里的dp\[i\]更新了就会影响下一步运算**

**所以在这里我们需要一个滚动数组，每次结束后，用更新后的数组替换原来数组**

\*\*\*\*

**示例题目：杨辉三角Leetcode 119**

```cpp
vector<int> getRow(int rowIndex) {
    vector<int>raw_k;
    vector<int>row_k;
    raw_k.resize(rowIndex+3, 0);
    row_k.resize(rowIndex+3, 0);
    raw_k[1] = 1;
    row_k[1] = 1;
    for(int i = 2; i <= rowIndex+1; i++){
        for(int j = 1; j <= i; j++){
            row_k[j] = raw_k[j-1] + raw_k[j];
        }
        raw_k = row_k;
    }
    return vector<int>(row_k.begin() + 1, row_k.end() - 1);
}
```

