# 层序遍历\(Level Order Traversal\)

### **层序遍历**

**只有层序遍历可以独自决定二叉树**

Leetcode二叉树的输入就是按照层序遍历的方式输入

也就是说

* 通过已知**层序遍历** 可以确定**二叉树**

```cpp
TreeNode* leveloOrderToTree(vector<int>&input){
    if(input.empty())return NULL;
    queue<TreeNode*>q;
    int i = 0;
    TreeNode* root = new TreeNode(input[i]);
    q.push(root);
    while(++i != input.size()){
        TreeNode* node = q.front();
        q.pop();
        if(input[i]!=NULL){
            TreeNode* left = new TreeNode(input[i]);
            node->left = left;
            q.push(left);
        }
        if(++i == input.size())break;
        if(input[i]!=NULL){
            TreeNode* right = new TreeNode(input[i]);
            node->right = right;
            q.push(right);
        }
    }
    return root;
}
```

* 反过来已知**二叉树**也可以得到**层序遍历**

这里没有用leetcode的那种带null的写法，而是根据level用二位数组输出（leetcode102\)

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>>res;
    queue<pair<TreeNode*,int>>q;
    if(root)q.push(make_pair(root,0));
    while(!q.empty()){
        TreeNode* cur = q.front().first;
        int level = q.front().second;
        q.pop();
        if(res.size() <= level)res.push_back(vector<int>());
        res[level].push_back(cur->val);
        if(cur->left)q.push(make_pair(cur->left, level+1));
        if(cur->right)q.push(make_pair(cur->right, level+1));
    }
    return res;
}
```

### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 102 | 二叉树的层序遍历 | 队列层序遍历 | 模板 |
| 987 | 二叉树的垂序遍历 | bfs\(队列层序遍历\) + hashmap | 中等 |

**Leetcode二叉树测试程序（有一个漏洞，因为int中存储NULL\('\0'\)与0本质上都是一样的，所以节点值不能为0）**

```cpp
# include <bits/stdc++.h>
using namespace std;
struct TreeNode{
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode():val(0), left(NULL), right(NULL){}
    TreeNode(int x):val(x), left(NULL), right(NULL){}
    TreeNode(int x, TreeNode* left, TreeNode* right): val(x), left(left), right(right){}
};
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack <TreeNode*> s;
        vector<int>res;
        if(root!=NULL)s.push(root);
        while(!s.empty()){
            TreeNode* node = s.top();
            s.pop();
            res.push_back(node->val);
            if(node->right)s.push(node->right);
            if(node->left)s.push(node->left);
        }
        return res;
    }
};
TreeNode* buildTree(vector<int>&input){
    if(input.empty())return NULL;
    queue<TreeNode*>q;
    int i = 0;
    TreeNode* root = new TreeNode(input[i]);
    q.push(root);
    while(++i != input.size()){
        TreeNode* node = q.front();
        q.pop();
        if(input[i]!=NULL){
            TreeNode* left = new TreeNode(input[i]);
            node->left = left;
            q.push(left);
        }
        if(++i == input.size())break;
        if(input[i]!=NULL){
            TreeNode* right = new TreeNode(input[i]);
            node->right = right;
            q.push(right);
        }
    }
    return root;
}
void print_vector(vector<int>&res){
    cout << "[";
    for(int i = 0; i < res.size(); i++){
        if(!i)cout << res[i];
        else cout << "," << res[i];
    }
    cout << "]";
}
int main(){
    // build binary tree, leetcode 输入顺序是按照level-order的顺序来输入的
    vector<int>input = {1, NULL, 2, 3}; //Limitation: stored value != 0, 0 is as NULL
    TreeNode* root = buildTree(input);
    // test
    Solution solve;
    vector<int> res = solve.preorderTraversal(root);
    print_vector(res);
    return 0;
}
```

**层序遍历构建二叉树模板**

```cpp
TreeNode* buildTree(vector<int>&input){
    if(input.empty())return NULL;
    queue<TreeNode*>q;
    int i = 0;
    TreeNode* root = new TreeNode(input[i]);
    q.push(root);
    while(++i != input.size()){
        TreeNode* node = q.front();
        q.pop();
        if(input[i]!=NULL){
            TreeNode* left = new TreeNode(input[i]);
            node->left = left;
            q.push(left);
        }
        if(++i == input.size())break;
        if(input[i]!=NULL){
            TreeNode* right = new TreeNode(input[i]);
            node->right = right;
            q.push(right);
        }
    }
    return root;
}
```

**987. 二叉树的垂序遍历**

**题意：把二叉树类比成一个左边空间，然后根据X轴，从左往右，从上往下输出节点**

**解法：**利用层序遍历，然后根据X轴坐标，来把当前节点append到X轴对应list的最后，因为层序遍历可以保证从上到下的顺序，所以这样处理完自然是结果了

\(但是这道题**有一个坑:** 就是要求如果节点坐标相等输出值更小的那个，加上这个限制的话会更复杂一点，**暂时没有考虑这一点**，所以下面的代码也不会通过Leetcode 987）

**层序遍历：使用队列**

```cpp
class Solution {
public:
    struct Tuple{
        TreeNode* node;
        int X;
        Tuple(TreeNode* node, int x):node(node),X(x){}
    };
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        queue<Tuple>q;
        map<int, vector<int>>mp; 
        //默认有序map就是less<int>,从小到大map<int,vector<int>,less<int>>mp
        vector<vector<int>>res;
        if(root!=NULL)q.push(Tuple(root,0));
        while(!q.empty()){
            Tuple cur = q.front();
            if(!mp.count(cur.X)){
                mp[cur.X] = vector<int>();
                mp[cur.X].push_back(cur.node->val);
            }
            else mp[cur.X].push_back(cur.node->val);
            q.pop();
            if(cur.node->left)q.push(Tuple(cur.node->left, cur.X-1));
            if(cur.node->right)q.push(Tuple(cur.node->right, cur.X+1));
        }
        for(auto it=mp.begin(); it!=mp.end(); it++){
             res.push_back(it->second);
        }
        return res;
    }
};
```

