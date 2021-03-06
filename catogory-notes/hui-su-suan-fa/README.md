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
| 46 | 全排列 | 回溯模板\(利用交换来遍历！） |  |
| 51 | N皇后 | 回溯模板+判定易错 | 中等 |
| 78 | 子集 | 回溯模板变体 |  |
| 77 | 组合 | 回溯模板变体 |  |
| 131 | 分割回文串 | 回文串用dp处理 + 回溯模板 | 技巧 |
| 140 | 单词拆分-II | 字典树的问题 回溯解决 | 技巧 |

**46. 全排列**

给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。

**解决：回溯 + dfs**

**简单版：swap交换！**

```cpp
vector<vector<int>> res;
void backtrack(vector<int>&nums, int pos, vector<int>&track){
    if(pos == nums.size())res.push_back(track);
    for(int i = pos; i < nums.size(); i++){
        swap(nums[i], nums[pos]);
        track.push_back(nums[pos]);
        backtrack(nums, pos+1, track);
        track.pop_back();
        swap(nums[i], nums[pos]);
    }
}
vector<vector<int>> permute(vector<int>& nums) {
    vector<int> track;
    backtrack(nums, 0,track);
    return res;
}
```

**复杂版：**

无论对于**有重复**还是**没有重复，**思路都是**定义三个数组**：

* 数组​used​：​used\[i\]​表示​nums\[i\]​是否已使用过，初始化全为​false​
* 数组​path​，表示从根结点到该节点经过的路径，即当前已生成的数组，初始化为空。
* 数组​res​存储结果。

下面代码没有直接按照上述方式走，因为是以前的代码

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

**判定一定是：**abs\(val - cur\[i\]\) == abs\(tmp - i\) 

**不要写反！！是abs\(y2 - y1\) = abs \(x2 - x1\) 而不是abs\(y1 - x1\) = abs\(y2- x2\)**

```cpp
class Solution {
public:
    bool valid(vector<int>&cur, int val){
        for(int i = 0; i < cur.size(); i++){
            int tmp = cur.size();
            if(val == cur[i] || abs(val - cur[i]) == abs(tmp - i))return false;
        }
        return true;
    }
    void dfs(vector<int>&cur, int n){
        if(cur.size() == n){
            cnt++;
            return;
        }
        for(int i = 0; i < n; i++){
            int v = i;
            if(valid(cur, v)){
                cur.push_back(v);
                dfs(cur, n);
                cur.pop_back();
            }
        }
    }
    int Nqueen(int n) {
        cnt = 0;
        vector<int>vec;
        dfs(vec, n);
        return cnt;
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

**131. 分割回文串**

给定一个字符串 _s_，将 _s_ 分割成一些子串，使每个子串都是回文串。

返回 _s_ 所有可能的分割方案。

**示例:**

```text
输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]
```

**关键：回文串需要用dp来处理，避免以后每次都要O\(n\)的检查回文串**

```cpp
vector<vector<string>>res;
vector<vector<bool>>dp;
void dfs(string& s, vector<string>&cur, int pos){
    if(pos == s.size())
        res.push_back(cur);
    for(int i = pos + 1; i <= s.size(); i++){
        if(!dp[pos][i])continue;
        cur.push_back(s.substr(pos, i - pos));
        dfs(s, cur, i);
        cur.pop_back();
    }
}
vector<vector<string>> partition(string s) {
    if(s.size() == 0)return res;
    int n = s.size();
    dp.resize(n, vector<bool>(n+1, false));
    for(int i = 0; i < s.size(); i++){
        dp[i][i] = true;
        dp[i][i+1] = true;
    }
    for(int i = 2; i <= s.size(); i++){
        for(int start = 0; start + i <= s.size(); start++){
            if(s[start] == s[start + i - 1])dp[start][start + i] = dp[start+1][start + i - 1];
        }
    }
    vector<string>cur;
    dfs(s, cur, 0);
    return res;
}
```

**140. 单词拆分 II**

给定一个**非空**字符串 _s_ 和一个包含**非空**单词列表的字典 _wordDict_，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。

```cpp
class Solution {
private:
    unordered_map<int, vector<string>> ans;
    unordered_set<string> wordSet;

public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        wordSet = unordered_set(wordDict.begin(), wordDict.end());
        backtrack(s, 0);
        return ans[0];
    }

    void backtrack(const string& s, int index) {
        if (!ans.count(index)) {
            if (index == s.size()) {
                ans[index] = {""};
                return;
            }
            ans[index] = {};
            for (int i = index + 1; i <= s.size(); ++i) {
                string word = s.substr(index, i - index);
                if (wordSet.count(word)) {
                    backtrack(s, i);
                    for (const string& succ: ans[i]) {
                        ans[index].push_back(succ.empty() ? word : word + " " + succ);
                    }
                }
            }
        }
    }
};
```

