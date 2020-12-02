# 回溯算法 / 深搜 / 广搜

**解决一个回溯问题，实际上就是一个决策树的遍历过程**。你只需要思考 3 个问题：

1、路径：也就是已经做出的选择。

2、选择列表：也就是你当前可以做的选择。

3、结束条件：也就是到达决策树底层，无法再做选择的条件。

**回溯算法的框架：**

```cpp
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

**树的遍历：**

```cpp
void traverse(TreeNode root) {
    for (TreeNode child : root.childern)
        // 前序遍历需要的操作
        traverse(child);
        // 后序遍历需要的操作
}
```

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 46 | 全排列 | 回溯模板 |  |
| 51 | N皇后 | 回溯模板+特殊判定 | 中等 |
| 78 | 子集 | 回溯模板变体 |  |
| 77 | 组合 | 回溯模板变体 |  |
|  |  |  |  |

**46. 全排列**

给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。

```cpp
class Solution {
public:
    vector<vector<int>> res; //全局变量记录结果
    void backtrack(vector<int>&nums, vector<int>&track){
        if(track.size() == nums.size()){
            res.push_back(track);
            return;
        }
        int n = nums.size();
        for(int i = 0; i < n; ++i){
            bool repeat = false;
            for(auto &e: track){
                if(e == nums[i]){
                    repeat = true;
                    break;
                }
            }
            if(repeat)continue;
            track.push_back(nums[i]); 
            backtrack(nums, track);
            track.pop_back();
        }
        return;
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> track; 
        backtrack(nums, track);
        return res;
    }
};
```

**51. N皇后**

**回溯模板基础上 + 对N皇后的坐标判定 + 输出处理**

```cpp
class Solution {
public:
    vector<vector<int>> res;
    void backtrack(int n, vector<int>&track){
        if(track.size() == n){
            res.push_back(track);
            return;
        }
        int y = track.size();
        for(int i = 0; i < n; ++i){
            bool conflict = false;
            for(int j = 0; j < y; ++j){
                if(i == track[j] || y + i == j + track[j] || y - j == i - track[j]){
                    conflict = true;  //判定不在一条直线，不在一条斜线
                    break;
                }
            }
            if(conflict)continue;
            track.push_back(i);
            backtrack(n, track);
            track.pop_back();
        }
        return;
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<int>track;
        backtrack(n, track);
        //输出处理
        vector<vector<string>> output;
        for(auto &r: res){
            vector<string> mat;
            for(int i = 0; i < n; ++i){
                string tmp(n, '.');
                tmp[r[i]]='Q';
                mat.push_back(tmp);
            }
            output.push_back(mat);
        }
        return output;
    }
};
```

**78. 子集**

因为是子集，每个track一被更新就直接push进去，回溯中加一个参数记录处理过的个数

注意：要添加空集

```cpp
class Solution {
public:
    vector<vector<int>> res;
    void backtrack(vector<int>& nums, vector<int>& track, int l){
        int n = nums.size();
        if(l == n){
            return;
        }
        for(int i = l; i < n; ++i){
            track.push_back(nums[i]);
            res.push_back(track);
            backtrack(nums, track, i+1);
            track.pop_back();
        }
        return;
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        if(nums.empty())return res;
        res.push_back(vector<int>{});
        vector<int>track;
        backtrack(nums, track, 0);
        return res;
    }
};
```

**77. 组合**

 给定两个整数 _n_ 和 _k_，返回 1 ... _n_ 中所有可能的 _k_ 个数的组合

```cpp
class Solution {
public:
    vector<vector<int>> res;
    void backtrack(vector<int>&track, int n, int k, int v){
        int l = track.size();
        if(l == k){
            res.push_back(track);
            return;
        }
        if(n == v){
            return;
        }
        for(int i = v; i < n; ++i){
            track.push_back(i+1);
            backtrack(track, n, k, i+1);
            track.pop_back();
        }
        return;
    }
    vector<vector<int>> combine(int n, int k) {
        if(k == 0 || n == 0)return res;
        vector<int> track;
        backtrack(track, n, k, 0);
        return res;
    }
};
```

