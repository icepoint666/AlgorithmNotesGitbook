# 深搜 / 广搜

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 12 |  矩阵中的路径 | 经典字符矩阵中找一个字符串路径，经典dfs | 不够熟练 |
| 剑指 Offer 13 | 机器人的运动范围 | 经典dfs + 记录访问过的地方 | 熟练完成 |
| 剑指 Offer 32 | 从上到下打印二叉树 | 经典bfs | 模板 |
| 剑指 Offer 32 Ⅲ | 从上到下打印二叉树 | bfs变体+deque双向队列+奇偶不同逻辑 | 很容易乱，多做几遍，尽量记忆！ |

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

**剑指 Offer 32 - I. 从上到下打印二叉树**

```cpp
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        queue<TreeNode*>que;
        vector<int>res;
        if(root == NULL)return res;
        que.push(root);
        while(!que.empty()){
            TreeNode *node = que.front();
            que.pop();
            if(node == NULL){
                continue;
            }
            res.push_back(node->val);
            que.push(node->left);
            que.push(node->right);
        }
        return res;
    }
};
```

**剑指 Offer 32 - III. 从上到下打印二叉树 III**

根据奇偶来决定deque的正向反向，比较绕，逻辑见注释

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        deque<pair<TreeNode*,int>>que;//标记层数
        vector<vector<int>>res;
        if(root==NULL)return res;
        int last_level = 0;
        bool flag = false; //true表示正向，false表示反向
        //root是反向，第一子层就是被反向添加，正向访问，第二子层就是正向被添加
        //（注意因为第一子层反向，所以正向添加要先right再left
        que.push_front(make_pair(root, 0));
        while(!que.empty()){
            int level;
            if(flag)level = que.front().second;
            else level = que.back().second;
            if(level > last_level){//更新方向
                last_level = level;
                flag = !flag;
            }
            TreeNode* node;
            //添加的方向：正向就是front pop, back添加
            if(flag){
                node = que.front().first;
                que.pop_front();
            }else{
                node = que.back().first;
                que.pop_back();
            }
            if(node == NULL){
                continue;
            }
            if(level >= res.size()){
                res.push_back(vector<int>{});
            }
            res[level].push_back(node->val);
            //正向就是：
            if(flag){//back添加的时候，right left注意也要取反，这个是最容易乱的
                que.push_back(make_pair(node->right, level+1));
                que.push_back(make_pair(node->left, level+1));
            }else{
                que.push_front(make_pair(node->left, level+1));
                que.push_front(make_pair(node->right, level+1));
            }
        }
        return res;
    }
};
```

