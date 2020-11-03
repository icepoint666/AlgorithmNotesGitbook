# inplace修改树结构

**对于一个二叉树结构，直接在原本树上进行修改，不用开一个空间额外存TreeNode指针**

**题目：**掌握Inplace的修改方法

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 897 | 递增顺序查找树\(中序遍历重排\) | 递归/迭代 + 前驱 | 多种做法 |
| 114 | 二叉树展开为链表\(前序遍历重排\) | 递归/迭代 + 前驱 | 多种做法 |

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

**解法：递归、迭代（本质上都是依赖栈去存储TreeNode\*，等于都需要额外空间作为开销）**

**Node的right因为没有改变**，**所以可以继续进行接下来的中序遍历**

```cpp
class Solution {
public:
    TreeNode* cur;
    void inorder(TreeNode* root){
        if(root==NULL)return;
        inorder(root->left);
        //inplace
        root->left = NULL; //注意：一定不要遗漏set为NULL这步
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

**114. 二叉树展开为链表**

 给定一个二叉树，**按照前序遍历的方式，**[原地](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95/8010757)将它展开为一个单链表。

**解法1：递归/迭代（利用栈空间）**

```cpp
class Solution {
public:
    TreeNode* cur;
    void preorder(TreeNode* root){
        if(!root)return;
        TreeNode* left = root->left;
        TreeNode* right = root->right;
        //inplace
        cur->right = root;
        cur->left = NULL; //一定要注意清除为NULL
        cur = root;
        //preorder
        preorder(left);
        preorder(right);
    }
    void flatten(TreeNode* root) {
        TreeNode* dummyhead = new TreeNode(0);
        cur = dummyhead;
        preorder(root);
    }
};
```

解法2：前驱节点（空间复杂度是 O\(1\)O\(1\) 的做法）

