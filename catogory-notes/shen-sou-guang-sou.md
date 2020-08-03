# 深搜 / 广搜

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 12 |  矩阵中的路径 | 经典字符矩阵中找一个字符串路径，经典dfs | 不够熟练 |
| 剑指 Offer 13 | 机器人的运动范围 | 经典dfs + 记录访问过的地方 | 熟练完成 |

**剑指 Offer 12. 矩阵中的路径**

几个点需要注意：

1. dfs路径要记录访问过的位置，可以不增加额外矩阵的情况下，用'/'标记
2. dfs上下左右遍历的时候不要写四个if，尽量用循环解决，节省代码量
3. dfs开始的判定：第一个判定是否可以往这里走，第二个判定是否到达终点

```cpp
class Solution {
public:
    int h, w;
    bool dfs(vector<vector<char>>& board, int i, int j, int res, string word){
        if(0 > i || i >= h || 0 > j || j >= w || board[i][j] != word[res])return false;
        if(res == word.size() - 1){
            return true; // 判定终点
        }
        char temp = board[i][j];
        board[i][j] = '/';  //临时替代法，节省一个vis矩阵
        int dx[] = {-1, 0, 1, 0}, dy[] = {0, -1, 0, 1};
        for(int k = 0; k < 4; ++k){ //for循环节省代码量
            int x = i + dx[k], y = j + dy[k];
            if(dfs(board, x, y, res+1, word))return true;
        }
        board[i][j] = temp;
        return false;
    }
    bool exist(vector<vector<char>>& board, string word) {
        if(word.empty())return false;
        h = board.size();
        w = board[0].size();
        for(int i = 0; i < h; ++i){
            for(int j = 0; j < w; ++j){
                if(dfs(board, i, j ,0, word))return true;
            }
        }
        return false;
    }
};
```

**剑指 Offer 13. 机器人的运动范围**

**直接贴代码**

```cpp
class Solution {
public:
    int cnt = 0;
    bool vis[101][101];
    void dfs(int x, int y, int m, int n, int k){
        bool invalid = (x/10)+(x%10)+(y/10)+(y%10) > k;
        if(x < 0 || x >= m || y < 0 || y >= n || invalid || vis[x][y])return;
        vis[x][y] = true;
        cnt++;
        int dx[] = {-1, 0, 1, 0};
        int dy[] = {0, 1, 0, -1};
        for(int i = 0; i < 4; i++){
            int xx = x + dx[i];
            int yy = y + dy[i];
            dfs(xx, yy, m, n, k);
        }
        return;
    }
    int movingCount(int m, int n, int k) {
        memset(vis, false, sizeof(vis));
        cnt = 0;
        dfs(0, 0, m, n, k);
        return cnt;
    }
};
```

