# 根据遍历构造树的问题

构造树的问题本质上都可以通过**递归分治**的思路来解决

**核心做法**：记录当前子树对应序列中的区间\[st,ed\]，然后找寻左子树右子树的区间，从而实现分治

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 105 | 前序 + 中序 构造二叉树 |  |  |
| 106 | 中序 + 后序 构造二叉树 |  |  |
| 889 | 前序 + 后序 构造二叉树 |  |  |
| 1008 | 前序遍历构造二叉搜索树 | 记录当前子树对应序列的st,ed | 中等 |

 ****

**1008. 前序遍历构造二叉搜索树**

**（二叉搜索树只需要前序就可以构造）**

```cpp
TreeNode* buildTree(vector<int>& preorder, int st, int ed){
    if(st > ed)return NULL;
    TreeNode* root = new TreeNode(preorder[st]);
    //寻找左数右树分隔点
    int mid = ed + 1; //这个初始化设置很重要
    for(int i = st + 1; i <= ed; i++){
        if(preorder[i] > root->val){
            mid = i;
            break;
        }
    }
    root->left = buildTree(preorder, st+1, mid-1);
    root->right = buildTree(preorder, mid, ed);
    return root;
}
    
TreeNode* bstFromPreorder(vector<int>& preorder) {
    if(preorder.empty())return NULL;
    return buildTree(preorder, 0, preorder.size()-1);
}
```

**105. 从前序与中序遍历序列构造二叉树**

**类似于上述做法：分治**

前序遍历：只需要找到中间左子树右子树分隔点

中序遍历：根据前序遍历的root节点，就可以分开，左边为左子树，右边为右子树

（需要记录一个中序遍历的map，value-&gt;index\)

```cpp
class Solution {
public:
    unordered_map<int,int>index;
    TreeNode* BuildTree(vector<int>& preorder, vector<int>& inorder, int pre_st, int pre_ed, int in_st, int in_ed){
        if(pre_st > pre_ed)return NULL;
        TreeNode *node = new TreeNode(preorder[pre_st]);
        int idx = index[preorder[pre_st]];
        int len = idx - in_st;
        node->left = BuildTree(preorder, inorder, pre_st+1, pre_st+len, in_st, idx-1);
        node->right = BuildTree(preorder, inorder, pre_st+len+1, pre_ed, idx+1, in_ed);
        return node;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if(preorder.empty())return NULL;
        int len = inorder.size();
        for(int i = 0; i < len; i++){
            index[inorder[i]] = i;
        }
        return BuildTree(preorder, inorder, 0, len-1, 0, len-1);
    }
};
```

