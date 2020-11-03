# inplace修改树结构

中等**对于一个二叉树结构，直接在原本树上进行修改，不用开一个空间额外存TreeNode指针**

**通常解法有2个：**

* 基本解法：递归，迭代
* 前驱节点法：往往更高效，O\(1\)空间

**题目：**掌握Inplace的修改方法

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 897 | 递增顺序查找树\(中序遍历重排\) | 递归/迭代  | 多种做法 |
| 114 | 二叉树展开为链表\(前序遍历重排\) | 递归/迭代 + 前驱 | 多种做法 |
| 814 | 二叉树剪枝 | 后序遍历剪枝法 | 巧法 |
| 1110 | 删点成林 | 注意一个细节 | 中灯光 |

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

**解法1：递归**

**Node的right因为没有改变**，**所以可以继续进行接下来的中序遍历**

**（相比于下面的前序遍历的题，由于中序遍历的特性，递归迭代也是不需要开辟额外栈空间的）**

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

**解法2：迭代（裸的inorder迭代模板 + inplace操作）**

```cpp
class Solution {
public:
    TreeNode* increasingBST(TreeNode* root) {
        TreeNode* dummyhead = new TreeNode(0);
        TreeNode* node = dummyhead;
        TreeNode* cur = root;
        stack<TreeNode*>s;
        while(cur!=NULL||!s.empty()){
            while(cur!=NULL){
                s.push(cur);
                cur = cur->left;
            }
            cur = s.top();
            s.pop();
            //inplace
            node->right = cur;
            node->left = NULL;
            node = node->right;

            cur = cur->right;
        }
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

**解法2：前驱节点（空间复杂度真正是 O\(1\) 的做法）**

问题转化成**寻找当前节点的前驱节点（前驱这个概念是指中序遍历的前驱）**

具体做法：

对于**当前节点**，如果其左子节点不为空，则在其**左子树中找到最右边的节点**，作为**前驱节点**

* 将**当前节点的右子节点**赋给**前驱节点的右子节点 //1**
* 然后将**当前节点的左子节点**赋给**当前节点的右子节点 //2**
* 并**将当前节点的左子节点设为空 //3**

对当前节点处理结束后，继续处理链表中的下一个节点（也就是右节点），直到所有节点都处理结束。

```cpp
class Solution {
public:
    void flatten(TreeNode* root) {
        TreeNode *curr = root;
        while (curr != nullptr) {
            if (curr->left != nullptr) {
                auto predecessor = curr->left;
                while (predecessor->right != nullptr) {
                    predecessor = predecessor->right;
                } //寻找前驱
                predecessor->right = curr->right; //1
                curr->right = curr->left;         //2
                curr->left = nullptr;             //3
            }
            curr = curr->right;
        }
    }
};
```

**814. 二叉树剪枝**

给定二叉树根结点 `root` ，此外树的每个结点的值要么是 0，要么是 1。

inplace的移除所有不包含 1 的子树。

**做法：后序遍历，比较巧**

```cpp
TreeNode* pruneTree(TreeNode* root) {
    if(root == NULL)return root;
    if(root->left)root->left = pruneTree(root->left);
    if(root->right)root->right = pruneTree(root->right);
    if(!root->left && !root->right && root->val == 0)return NULL;
    return root;
}
```

