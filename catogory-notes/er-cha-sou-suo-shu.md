# 二叉搜索树

**二叉搜索树的特性：**

* 从上往下遍历BST，当前访问到的left/right子节点，一定是当前访问到的所有点中离它root值最近的两个点

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指Offer 33 | 二叉搜索树后序遍历序列 | 见单调栈 | 思考 |
| 剑指Offer 36 | 二叉搜索树与双向链表 | 递归+分治+保存头尾指针 | 时间久做出来，注意最后连接头尾 |
| 剑指Offer 68-II | 二叉搜索树的最近公共祖先 | 想到处理关键点 | 不太容易立刻想出 |
| 面试题 04.05 | 合法二叉搜索树 | 向下遍历参数要加入min节点与max节点 | 易错 |
| 1382 | 将二叉搜索树变平衡 | 不要误会成将搜索树转换成AVL树 | 易错 |

**剑指 Offer 36. 二叉搜索树与双向链表**

**题意：**要求在原来二叉树的数据结构上，直接转换为双向链表，（注意：题目最后要求，要把首尾连接起来）

**解法：分治，**每一个子树返回一个**pair&lt;Node\*,Node\*&gt;**分别表示这个子树的双向链表的最左指针 与 最右指针

```cpp
pair<Node*, Node*> treeToDL(Node* root){
    //讨论4种情况
    if(root->left==NULL && root->right==NULL)return make_pair(root, root);
    if(root->left==NULL){
        pair<Node*, Node*> r_pair = treeToDL(root->right);
        r_pair.first->left = root;
        root->right = r_pair.first;
        return make_pair(root, r_pair.second);
    }
    if(root->right==NULL){
        pair<Node*, Node*> l_pair = treeToDL(root->left);
        l_pair.second->right = root;
        root->left = l_pair.second;
        return make_pair(l_pair.first, root);
    }
    pair<Node*, Node*> l_pair = treeToDL(root->left);
    pair<Node*, Node*> r_pair = treeToDL(root->right);
    l_pair.second->right = root;
    r_pair.first->left = root;
    root->left = l_pair.second;
    root->right = r_pair.first;
    return make_pair(l_pair.first, r_pair.second);
}

Node* treeToDoublyList(Node* root) {
    if(root==NULL)return NULL;
    auto tmp = treeToDL(root);
    //记得连接首尾
    tmp.first->left = tmp.second;
    tmp.second->right = tmp.first;
    return tmp.first;
}
```

**剑指 Offer 68 - I. 二叉搜索树的最近公共祖先**

**关键点：**想到一点：如果一个大于root一个小于root那么必然，当前就是最近祖先

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(root==NULL)return NULL;
    if(root->val == p->val || root->val == q->val)return root;
    if(root->val > p->val && root->val < q->val || root->val < p->val && root->val >q->val)return root;
    if(root->val > p->val && root->val > q->val)return lowestCommonAncestor(root->left, p, q);
    else return lowestCommonAncestor(root->right, p, q);
}
```

**面试题 04.05. 合法二叉搜索树**

**注意1**：向下遍历的时候需要保留min以及max，来限定子节点合法取值的范围

**注意2**：二叉搜索树是不允许相等节点的（不可能有在下一层子节点还等于它的父节点的情况）

```cpp
bool isValidBST(TreeNode* root, TreeNode* min, TreeNode* max){
    if(root == NULL)return true;
    if(min != NULL && root->val <= min->val)return false; //注意这个等号很关键，二叉搜索树是不允许相等的
    if(max != NULL && root->val >= max->val)return false;
    return isValidBST(root->left, min, root) && isValidBST(root->right, root, max);
}
bool isValidBST(TreeNode* root) {
    return isValidBST(root, NULL, NULL);
}
```

**1382. 将二叉搜索树变平衡**

**这道题不要误以为是去手撕AVL，注意前面的AVL树的插入或者删除代码是在AVL树上修改的，如果想把BST变成AVL单纯依靠左旋右旋是不行的**

**解法: 先将数据中序遍历下来，都存储到vector里面，然后构造一个平衡二叉树**

```cpp
class Solution {
public:
    vector<int>values;
    void inOrder(TreeNode* root){ //中序遍历
        if(root==NULL)return;
        inOrder(root->left);
        values.push_back(root->val);
        inOrder(root->right);
    }
    TreeNode* buildTree(int l, int r){ //利用二分法建立平衡二叉树
        while(l > r)return NULL;
        int mid = (l + r) / 2;
        TreeNode* root = new TreeNode(values[mid]);
        root->left = buildTree(l, mid-1);
        root->right = buildTree(mid+1, r);
        return root;
    }
    TreeNode* balanceBST(TreeNode* root) {
        values.clear();
        inOrder(root);
        return buildTree(0, values.size()-1);
    }
}
```

（如果要求不能使用额外空间，那就将原来BST的node delete一个，然后再在AVL树上insert一个，利用AVL树的treeInsert方法一个一个插入）

