# 深搜 / 广搜

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 12/79 |  矩阵中的路径 | 经典字符矩阵中找一个字符串路径，经典dfs | 尽量熟练 |
| 剑指 Offer 13 | 机器人的运动范围 | 经典dfs + 记录访问过的地方 | 熟练完成 |
| 386 | 字典序排数 | dfs枚举 | 想到dfs |
| 301 | 删除无效的括号 | dfs枚举/剪枝 | 想到dfs，困难 |
| 剑指 Offer 32 | 从上到下打印二叉树 | 经典bfs | 模板 |
| 剑指 Offer 32 Ⅲ | 从上到下打印二叉树 | bfs变体+deque双向队列+奇偶不同逻辑 | 很容易乱 |
| lint 600 | 包裹黑色像素点的最小矩形 | 方法1：dfs/bfs 方法2：二分 |  |
| 200 | 岛屿数量 | inplace修改矩阵，作为visited数组 | 简单 |

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

**386. 字典序排数**

 给定 _n_ =13，返回 \[1,10,11,12,13,2,3,4,5,6,7,8,9\] 。

**解法：能够想到通过递归dfs的方式**

```cpp
vector<int> ans;
void dfs(int num, int& n){
    if(num > n)return;
    ans.push_back(num);
    for(int i = 0; i <= 9; ++i)dfs(num * 10 + i, n);
}
vector<int> lexicalOrder(int n) {
    for(int i = 1; i <= 9; ++i)dfs(i, n);
    return ans;
}
```

**301. 删除无效的括号**

dfs来枚举几种情况，注意几种剪枝都要考虑到

```cpp
class Solution {
public:
    set<string>res;
    int min_op;
    void dfs(string &s, string cur, int pos, int left){
        if(pos == s.size()){
            if(!left){
                if(cur.size() < min_op){
                    return;
                }
                if(cur.size() > min_op){
                    res.clear();
                    min_op = min_op < cur.size() ? cur.size() : min_op;
                }
                res.insert(cur);
            }
            return;
        }
        if(left < 0)
            return;
        if(pos + left > s.size() || pos - left > s.size()){ //剪枝
            return;
        }
        if(cur.size() + s.size() - pos < min_op){ //剪枝
            return;
        }
        if(s[pos] == '('){
            dfs(s, cur+"(", pos+1, left+1);
            dfs(s, cur, pos+1, left);
        }else if(s[pos] == ')'){
            dfs(s, cur+")", pos+1, left-1);
            dfs(s, cur, pos+1, left);
        }else{
            dfs(s, cur+s[pos], pos+1, left);
        }
    }
    vector<string> removeInvalidPaarentheses(string s) {
        min_op = 0;
        dfs(s, "", 0, 0);
        vector<string> ret(res.begin(), res.end());
        return ret;
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

**lint 600 包裹黑色像素点的最小矩形**

一个由二进制矩阵表示的图，​0​ 表示白色像素点，​1​ 表示黑色像素点。黑色像素点是联通的，即只有一块黑色区域。像素是水平和竖直连接的，给一个黑色像素点的坐标 ​\(x, y\)​ ，返回囊括所有黑色像素点的矩阵的最小面积。

**样例 1:**

```text
输入：["0010","0110","0100"]，x=0，y=2
输出：6
解释：
矩阵左上角坐标是(0, 1), 右下角的坐标是(2, 2) 
```

**样例 2:**

```text
输入：["1110","1100","0000","0000"], x = 0, y = 1
输出：6
解释：
矩阵左上角坐标是(0,  0), 右下角坐标是(1, 3) 
```

**\(这题为什么需要\(x,y\)呢？因为如果没有x,y，需要遍历整个矩阵找到黑色像素点，复杂度O\(M\*N\)，对于一个黑色像素点的稀疏矩阵是代价比较高的）**

**解法：**

**方法1：**关于BFS和DFS

> BFS和DFS的做法就是遍历这个黑色像素连通块，得到各个方向上的坐标极值，时间复杂度O\(K\)，K为黑色像素点个数，这种做法在黑色像素点数量巨大时效率极低。  
> 另外关于BFS和DFS如何选择，这里建议使用BFS，因为DFS会占用大量的系统栈，空间复杂度上要劣于BFS

**方法2：**二分

一种在大部分情况下空间和时间上均优于DFS和BFS的算法——二分

二分找到四个方向上黑色像素点出现的坐标极值

* 这边以二分最左侧黑色像素为例

1. 设置左指针为0，右指针为​y​，因为我们保证​y​列上存在黑色像素，最左侧黑色像素所在列一定在​y​或者其左侧
2. 若​mid​所在列存在黑色像素，说明最左侧黑色像素在​mid​列或者其左侧，​r = mid​，否则​l = mid​
3. 判断​l​列是否存在黑色像素，若存在则​left = l​，否则​left = r​。注意一定要先判​l​列，因为​r​可能存在黑色像素，但并不是最左侧

* 以此类推继续找到最右侧，最上侧，最下侧的黑色像素所在列或行

