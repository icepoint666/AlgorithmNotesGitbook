# 层序遍历\(Level Order Traversal\)

Leetcode二叉树的输入就是按照层序遍历的方式输入

也就是说

* 通过已知**层序遍历** 可以确定**二叉树**
* 反过来已知**二叉树**也可以得到**层序遍历**

### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 102 | 二叉树的层序遍历 |  | 模板 |

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

