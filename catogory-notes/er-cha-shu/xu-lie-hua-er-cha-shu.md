# 序列化二叉树

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指Offer 37 / 297 | 二叉树序列化 | 层序遍历bfs +vector存结果 | 过程复杂但不难 |

**297.二叉树的序列化与反序列化**

```text
你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
```

这其实就是leetcode里面测试输入时的原理

leetcode序列化就是指**层序遍历，只不过这道题需要将数组转成字符串**

**层序遍历特性：**

* 二叉树 可以得出 层序遍历
* 层序遍历 也可以唯一确定 二叉树

**层序遍历反序列化buildtree的代码**

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
输入：vector<int>input = {1,2,3,NULL,NULL,4,5} 
存在一个漏洞，就是NULL在int中等于0，所以真正应该用字符串表示序列化
```

