# 矩阵

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 面试题01.07 | 旋转矩阵 | 90°旋转时 i,j坐标的位置关系 | 思路清晰 |



**面试题 01.07. 旋转矩阵**

**思路清晰：只用处理矩阵的半径即可，然后每次处理4个坐标的值依次轮转，用tmp存储一个值**

```cpp
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]

代码：
void rotate(vector<vector<int>>& matrix) {
    int N = matrix.size(); 
    int n = N / 2;
    for(int i = 0; i < n; i++){
        for(int j = i; j < N - i - 1; j++){
            int tmp = matrix[j][N-i-1];
            matrix[j][N-i-1] = matrix[i][j];
            matrix[i][j] = matrix[N-j-1][i];
            matrix[N-j-1][i] = matrix[N-i-1][N-j-1];
            matrix[N-i-1][N-j-1] = tmp;
        }
    }
}
```

