# inplace修改树结构

**对于一个二叉树结构，直接在原本树上进行修改，不用开一个空间额外存TreeNode指针**

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 897 | 递增顺序查找树 | 掌握Inplace的修改方法 |  |

**897. 递增顺序查找树**

 给你一个树，请你 **按中序遍历** 重新排列树

```text
    3           1
   / \           \
  2   4   --->    2
 /                 \
1                   3
                     \
                      4
```

采用inplace的做法直接在TreeNode上替换

因为题意的特性，只用修改当前Node的left树为NULL，然后接在dummyhead开头的树后面

**Node的right因为没有改变**，**所以可以继续进行接下来的中序遍历**

```cpp
class Solution {
public:
    TreeNode* cur;
    void inorder(TreeNode* root){
        if(root==NULL)return;
        inorder(root->left);
        //inplace
        root->left = NULL;
        cur->right = root;
        cur = root;
        //not change for right TreeNode
        inorder(root->right);
    }
    TreeNode* increasingBST(TreeNode* root) {
        TreeNode* dummyhead = new TreeNode(0);
        cur = dummyhead;
        inorder(root);
        return dummyhead->right;
    }
};
```

