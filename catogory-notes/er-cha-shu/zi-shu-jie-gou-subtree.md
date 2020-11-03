# 子树结构subtree

这类题目：需要检查子树的结构，**都需要一个 helper 函数，带着两个 root，递归解决。 如果默认没给，就自己写一个**

### **题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 100 | 相同的树 | 类似preorder的递归+迭代做法 | 简单 |
| 101 | 对称二叉树 | 对于相同的树，只需要换一个方向就好了，注意迭代做法 | 简单 |
| 250 | 统计同值子树 | 后序遍历，类似于路径之和中的124题 | 中等 |
| 222 | 完全二叉树的节点个数 | 考点：肯定不能裸的直接遍历，O\(logN\*logN\) | 中等 |
| Google一道面试题 | 寻找最大相等子树 |  | 困难 |

**100. 相同的树**

**递归**

```cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
    if(p == q)return true;
    if(p == NULL || q == NULL)return false;
    if(p->val != q->val)return false;
    return isSameTree(p->left, q->left) && isSameTree(p->right,q->right);
}
```

**迭代：参照preorder的迭代**

**101. 对称二叉树**

**递归**

```cpp
class Solution {
public:
    bool issymmetric(TreeNode* p, TreeNode* q){
        if(p == q)return true;
        if(p == NULL || q == NULL)return false;
        if(p->val != q->val)return false;
        return issymmetric(p->left, q->right) && issymmetric(p->right,q->left);
    }
    bool isSymmetric(TreeNode* root) {
        if(root==NULL)return true;
        return issymmetric(root->left, root->right);
    }
};
```

**迭代做法比较加分：**

**双stack或者双queue都可以\(下面这种双stack的写法，其实重复比较了一倍的量）**

```cpp
bool isSymmetric(TreeNode* root) {
        if(root==NULL)return true;
        stack<TreeNode*>stk1;
        stack<TreeNode*>stk2;
        stk1.push(root);
        stk2.push(root);
        while(!stk1.empty()&&!stk2.empty()){
            TreeNode* node1 = stk1.top();
            TreeNode* node2 = stk2.top();
            stk1.pop();
            stk2.pop();
            if(node1->val != node2->val)return false;

            if(node1->left)  stk1.push(node1->left);
            if(node2->right) stk2.push(node2->right);
            if(stk1.size() != stk2.size()) return false;

            if(node1->right) stk1.push(node1->right);
            if(node2->left)  stk2.push(node2->left);
            if(stk1.size() != stk2.size()) return false;
        }
        return stk1.size() == stk2.size(); //如果前面发现stk1为空，stk2不为空，到这里可以判定出来
    }
```

**250. 计算拥有的所有节点的值都相同的子树的个数**

```cpp
Input:  root = [5,1,5,5,5,null,5]

              5
             / \
            1   5
           / \   \
          5   5   5

Output: 4
```

解法：类似于124.二叉树最大路径和，利用**后序遍历**，来**根据左右子树的返回值，决定当前子树是不是满足要求的子树**（这样的复杂度也只有O\(n\)）

```cpp
class Solution {
public:
    int ans = 0;
    pair<bool,int> count(TreeNode* root){
        if(!root)return make_pair(false,0);
        if(!root->left && !root->right){
            ans++;
            return make_pair(true,root->val);
        }
        if(!root->left){
            pair<bool,int>right = count(root->right);
            if(right.first && right.second == root->val){
                ans++;
                return make_pair(true, root->val);
            }
            return make_pair(false,0);
        }
        if(!root->right){
            pair<bool,int>left = count(root->left);
            if(left.first && left.second == root->val){
                ans++;
                return make_pair(true, root->val);
            }
            return make_pair(false,0);
        }
        pair<bool,int>left = count(root->left);
        pair<bool,int>right = count(root->right);
        if(left.first && right.first && left.second == right.second && root->val == right.second){
            ans++;
            return make_pair(true,root->val);
        }
        return make_pair(false, 0);
    }
    int countUnivalSubtrees(TreeNode* root) {
        auto tmp = count(root);
        return ans;
    }
}; 
```

**222.完全二叉树的节点个数**

 给出一个**完全二叉树**，求出该树的节点个数

有这样的性质：**如果一个 Tree 是 complete tree，那所有的 subtree 也都是 complete tree.**

直接扫肯定 TLE .. 要利用好 complete tree 的定义和性质

**核心思路：每次计算root左子树，右子树的leftmost的height**

如果l\_height == r\_height，表明左子树是一个**满的完全树**

根据性质：右子树也是一个完全树，所以root=root-&gt;right继续向下递归

```cpp
int getLeftMostHeight(TreeNode* root){ // 迭代法
    int h = 0;
    while(root){
        h++;
        root = root->left;
    }
    return h;
}
int countNodes(TreeNode* root) {
    if(!root)return 0;
    int l = getLeftMostHeight(root->left);
    int r = getLeftMostHeight(root->right);
    if(l == r)return countNodes(root->right) + (1 << l);
    return countNodes(root->left) + (1 << r);
}
```

**Google一道面试题**

**题目：给定一个二叉树，寻找其中相等的两个子树，并且找到最大子树并返回**

**（注意这种题写出来了之后，要自己来写测试样例，写测试样例本质上也是考察的一个部分）**

**解法：暴力**

```cpp

```
