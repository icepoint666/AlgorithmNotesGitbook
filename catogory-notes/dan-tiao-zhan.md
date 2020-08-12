# 单调栈

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指Offer 33 | 二叉搜索树后序遍历序列 | 逆序后序遍历，单调栈 | 不会，值得记忆 |
|  |  |  |  |

**剑指 Offer 33. 二叉搜索树的后序遍历序列**

题意：给你一个整数序列，让你判断是不是二叉搜索树的后序遍历序列

转换问题：如果把后序遍历的序列倒序的话，那么就是\[中，右，左\]，这个顺序其实很类似于判断一个序列是不是二叉树的正序遍历序列

基本相同的问题：**判断一个序列是不是二叉搜索树的正序遍历序列**

**怎么使用栈来实现判断？**

倒序遍历序列，关键点在于发现如果**当前节点posterior\[i\]**比它root的值大时，表明矛盾

这个root的维护是关键，你怎么知道这个节点是root呢

我们把每个元素都加入到栈中，一旦**当前元素posterior\[i\]**小于栈中top的元素，那么就开始找root

**root就是栈中比posterior\[i\]大的最后一个元素**，（这个是一个关键特性，用循环实现）然后更新root

再把posterior\[i\]加入到栈中

```cpp
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        stack<int> stk;
        int root = 0x3f3f3f3f;
        int n = postorder.size();
        if(n == 0)return true;
        for(int i = n - 1; i >= 0; i--){
            if(postorder[i] > root)return false;
            while(!stk.empty() && postorder[i] < stk.top()){ //这个循环是太关键了！理解这个循环的意思
                root = stk.top();
                stk.pop();
            }
            stk.push(postorder[i]);
        }
        return true;
    }
};
```

