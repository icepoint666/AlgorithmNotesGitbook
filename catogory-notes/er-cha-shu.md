# 二叉树

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 437 | 路径总和Ⅲ | 二叉树遍历，双重递归 | 简单 |
| 1367 | 二叉树中的列表 | 二叉树遍历，双重递归 | 做的时间有点长 |

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

