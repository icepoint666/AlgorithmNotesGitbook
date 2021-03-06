# 后序遍历\(Postorder Traversal\)的应用

**汇总结果**

**Binary tree 的 post-order 遍历很实用，其遍历顺序的特点决定了其 bottom-up 的返回顺序，每次子树处理完了在当前节点上汇总结果**

**可以解决很多 和 subtree, tree path 相关的问题**

**在多叉树的情况下，也很容易扩展到各类的 search 问题：Leetcode351，安卓解锁模式**

* LCA问题：见LCA专题
* Find largest subtree: 见子树结构专题

### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 988 | 从叶节点开始的最小字符串 | 容易忽视字典序的特性 | 易错 |
| 1530 | 好叶子节点对的数量 | 经典后序遍历归并题 | 中等 |
| lint 595 | 二叉树最长连续序列 | 后序遍历，归并结果O\(N\) | 中等 |

**988. 从叶结点开始的最小字符串**

这道题很容易错的一个点就是：忽视**字典序特性**

**误以为可以 只要局部比较左右子树谁的字典序小，就接下来把谁送给父节点**

**这是不行的！**

示例：

a字典序比aba小，但是之后再经过两个父节点'b','z'后，abz比ababz的字典序大

如果这道题是从根节点到叶节点这样做是可以的

**做法1：归并所有结果（但其实这种做法复杂度很高）**

* **自底向上的return顺序 -- Post order**
* **子树计算完在当前节点汇总的计算 -- Merge Sort**

**做法2：dfs:** 当我们到达一个叶子节点的时候，我们翻转路径的字符串内容来创建一个候选答案。如果候选答案比当前答案要优秀，那么我们更新答案。

**1530. 好叶子节点对的数量**

给你二叉树的根节点 root 和一个整数 distance 。

如果二叉树中两个 叶 节点之间的 最短路径长度 小于或者等于 distance ，那它们就可以构成一组 好叶子节点对 。返回树中 好叶子节点对的数量 。

**解法：**采用**后续遍历**的思想**dfs**

搜索函数**返回**所有到当前节点\(深度小于distance\)的叶子节点

当前节点，处理左子树右子树的返回vector，把**左树右树归并一下**，自己返回时，略去高度已经超过distance的节点，再继续往上

```cpp
class Solution {
public:
    int ans;
    vector<int> dfs(TreeNode* root, int distance){
        if(root==NULL)return {};
        if(!root->left && !root->right)return {0};
        
        vector<int> res;
        auto left = dfs(root->left, distance);
        for(auto& e: left){
            if(++e > distance)continue;
            res.push_back(e);
        }
        auto right = dfs(root->right, distance);
        for(auto& e: right){
            if(++e > distance)continue;
            res.push_back(e);
        }
        for(auto& l: left)
            for(auto& r: right){
                ans += (l + r <= distance);}
        return res; 
    }
    int countPairs(TreeNode* root, int distance) {
        ans = 0;
        auto _ = dfs(root, distance);
        return ans;
    }
};
```

**二叉树最长连续序列**

给一棵二叉树，找到最长连续路径的长度。

这条路径是指 任何的节点序列中的起始节点到树中的任一节点都必须遵循 父-子 联系。最长的连续路径必须是从父亲节点到孩子节点变大（​不能逆序​）。

```cpp
输入:
{2,#,3,2,#,1,#}
输出:2
说明:
这棵树如图所示：
   2
    \
     3
    / 
   2    
  / 
 1
最长连续序列是2-3，而不是3-2-1，所以返回2.
```

解法：

核心：**后序遍历 + 遍历子节点后要给父节点返回什么信息**

递归步骤：

1. 如果节点为空，返回​\(0, 0\)​。
2. 令​rootMaxLength = 1​，代表 ****该节点的最长链长度。
3. 令​subtreeMaxLength = 1​，代表 子树最长链长度。
4. 递归获得左子树信息leftResult，更新​rootMaxLength​和​subMaxLength​。
5. 递归获得右子树信息rightResult，更新​rootMaxLength​和​subMaxLength​。
6. 返回​\(rootMaxLength, subMaxLength\)​。

