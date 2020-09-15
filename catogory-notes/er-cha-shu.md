# 二叉树

**代码模板：二叉树的查找**

```cpp
void BST(TreeNode* root, int target) {
    if (root->val == target)
        // 找到目标，做点什么
    if (root->val < target) 
        BST(root->right, target);
    if (root->val > target)
        BST(root->left, target);
}
```

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 437 | 路径总和Ⅲ | 二叉树遍历，双重递归 | 简单 |
| 1367 | 二叉树中的列表 | 二叉树遍历，双重递归 | 做的时间有点长 |
| 108/109 | 数组/链表转平衡二叉搜索树 | 快慢指针+分割链表 | 简单/中等 |
| 剑指offer 07 | 前序中序结果推二叉树 | 迭代器+find功能 | 做的时间有点长 |
| 剑指Offer 26 | 树的子结构 | 二叉树遍历，双重递归 | 快速做出 |
| 剑指Offer 28 | 判断二叉树是否对称 | 先镜像右子树，再判断左右是否相等 | 有一个条件容易漏 |
| 剑指Offer 37 | 二叉树序列化 | 层序遍历bfs +vector存结果 | 过程复杂但不难 |
| 剑指Offer 68-II | 二叉树的最近公共祖先 | 想到处理关键点（后序遍历） | 不太容易立刻想出 |
| 543 | 二叉树的直径 | 二叉树遍历，双重递归\(外面维护最大值 + 坑：直径是节点数要减1） | 简单 |
| 617 | 合并二叉树 | 将t1与t2覆盖合并，直接在t1上修改处理最后返回t1 | 简单 |
| 101 | 对称二叉树 | 递归实现 + 多记忆一种队列实现 | 简答 |
| 450 & 701 | 二叉树的插入与删除 | 先找再改,记忆当成模板 | 记忆 |

**437.路径总和**

选择**偏暴力**的做法，**双重递归**，遍历二叉树，每个节点作为路径起点，然后以这个起点作为root遍历子树判断值

```cpp
class Solution {
public:
    int count(TreeNode* root, int sum){
        if(root == NULL)return 0;
        return (root->val == sum) + count(root->left, sum-root->val) + count(root->right, sum - root->val);
    }
    int pathSum(TreeNode* root, int sum) {
        if(root == NULL)return 0;
        return count(root, sum) + pathSum(root->left, sum) + pathSum(root->right, sum);
    }
};
```

**1367. 二叉树中的列表**

**题意：二叉树中找一个路径和链表路径一样的，如果有一条就true**

二叉树遍历，双重递归**，一个递归开头，另一个递归路径是否一致**

关键是搜索的一些判定还要更熟练，第一遍写了快20分钟，这种题达到五分钟最好

```cpp
class Solution {
public:
    bool compare(ListNode* head, TreeNode * root){
        if(head == NULL)return true; //匹配成功
        if(root == NULL)return false; //树结束判定
        if(head->val == root->val) 
            return compare(head->next,root->left) || compare(head->next, root->right);
        return false;
    }
    bool isSubPath(ListNode* head, TreeNode* root) {
        if(root == NULL)return false; //树结束判定
        bool find = compare(head, root); //当前树作为根节点开始与链表匹配
        if(find)return true;
        find = isSubPath(head, root->left);
        if(find)return true;
        find = isSubPath(head, root->right);
        return find;
    }
};
```

**108/109.有序数组转平衡二叉树 / 有序链表转平衡二叉树** 

都不用排序了

**有序数组**比较简单，一直取中点作为根节点，小的作为左树，大的作为右树，递归下去就可以了

**有序链表**的话: 快慢指针 找到后半段的开头 + 分割链表

**注意记忆：创建二叉树或者链表的模板**

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if(head == NULL)return NULL;
        if(head->next == NULL){
            TreeNode* root = new TreeNode(head->val);
            return root;
        }
        ListNode* slow = head;
        ListNode* fast = head->next;
        while(fast->next!=NULL && fast->next->next!=NULL){
            slow = slow->next;
            fast = fast->next->next;
        }
        TreeNode* root = new TreeNode(slow->next->val);
        ListNode* beg = slow->next->next;
        slow->next = NULL;
        root->left = sortedListToBST(head);
        root->right = sortedListToBST(beg);
        return root;
    }
};
```

**剑指 Offer 07. 重建二叉树**

利用 **迭代器+find** 完成整个过程会比较优雅

```cpp
class Solution {
public:
    int i = 0;
    TreeNode* build_tree(vector<int>&preorder, vector<int>::iterator beg, vector<int>::iterator ed){
        if(i == preorder.size())return NULL;
        auto it = find(beg, ed, preorder[i]); //返回找到的迭代器位置
        if(it == ed)return NULL;
        TreeNode* root = new TreeNode(preorder[i]);
        i++;
        root->left = build_tree(preorder, beg, it); //找到迭代器处，左边就是左树
        root->right = build_tree(preorder, ++it, ed); //右边就是右树
        return root;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return build_tree(preorder, inorder.begin(), inorder.end());
    }
};
```

**剑指 Offer 26. 树的子结构**

一个函数遍历子结构的起点

一个函数负责验证是否逐元素匹配

\(注意一些细节：题目中说明了B为空的话不是任何树的子结构，就是A为NULL或者B为NULL的时候的判定\)

\(注意这里的 && 和 \|\|）

```cpp
bool isContain(TreeNode * A, TreeNode *B){
    if(B == NULL)return true;  //到这里B为空说明匹配陈宫，前面B本身已经不可能为空
    if(A == NULL)return false; 
    if(A->val == B->val)return isContain(A->left, B->left) && isContain(A->right, B->right);
    return false;
}
bool isSubStructure(TreeNode* A, TreeNode* B) {
    if(B == NULL || A == NULL)return false; //不管A为空B为空都是说明不是子结构
    if(isContain(A, B))return true;
    return isSubStructure(A->left, B) || isSubStructure(A->right, B);
}
```

**剑指 Offer 28. 对称的二叉树**

先将镜像反转其中一子树，再比较左右两子树是否完全相等

```cpp
TreeNode* mirrorTree(TreeNode* root){
    if(root==NULL)return NULL;
    TreeNode* tmp = mirrorTree(root->left);
    root->left = mirrorTree(root->right);
    root->right = tmp;
    return root;
}
bool isSame(TreeNode* A, TreeNode *B){
    if(A==NULL && B==NULL)return true;
    else if(A == NULL || B == NULL)return false; //容易漏掉这个条件
    if(A->val == B->val)return isSame(A->left, B->left) && isSame(A->right, B->right);
    else return false;
}
bool isSymmetric(TreeNode* root) {
    if(root==NULL)return true;
    root->right = mirrorTree(root->right);
    return isSame(root->left, root->right);
}
```

**剑指 Offer 68 - II. 二叉树的最近公共祖先**

**关键点：**分四种情况讨论，如果在左边，在右边，一个在左一个在右（说明这个就是结果），都没有返回NULL

**记住这个做法和思想**

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(root==NULL)return NULL;
    if(root==p || root==q)return root;
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    if(left==NULL && right==NULL)return NULL;
    else if(left==NULL)return right;
    else if(right==NULL)return left;
    return root; 
}
```

**101. 对称二叉树**

**递归实现：见leetcode**

**使用队列迭代实现：见leetcod**

**450 二叉树的插入值**

原则：一直查找，直到找不到为止，然后把这个NULL的节点换成新插入的节点

```cpp
TreeNode* insertIntoBST(TreeNode* root, int val) {
    if(root == NULL){
        root = new TreeNode(val);
        return root;
    }
    if(root->val < val)root->right = insertIntoBST(root->right, val); //一定要修改root->right
    else if(root->val > val)root->left = insertIntoBST(root->left, val);
    return root;
}
```

**701 二叉树的删除节点**

**删除节点的三种情况：**

**①如果没有子节点：它可以当场去世**

**②如果只有子节点：直接移上去**

**③如果有两个子节点：可以通过找到左子树最大节点，右子树最小节点**

