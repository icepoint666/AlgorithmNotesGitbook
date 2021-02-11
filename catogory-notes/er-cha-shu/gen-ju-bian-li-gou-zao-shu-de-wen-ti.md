# 根据遍历构造树

构造树的问题本质上都可以通过**递归分治**的思路来解决

**核心做法**：记录当前子树对应序列中的区间\[st,ed\]，然后找寻左子树右子树的区间，从而实现分治

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 105 | 前序 + 中序 构造二叉树 | 见下方题解，用迭代器实现更加优雅 | 中等 |
| 106 | 中序 + 后序 构造二叉树 | 利用后序前序的关联性，近似于105 | 中等 |
| 889 | 前序 + 后序 构造二叉树 | 见下方题解 | 中等 |
| 1008 | 前序遍历构造二叉搜索树 | 记录当前子树对应序列的st,ed | 中等 |
| 1028 |  从先序遍历还原二叉树 | 递归，传递idx引用，从而能够复原 | 困难 |

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

**另一种写法：用迭代器实现会比较优雅**

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

**106. 从中序与后序遍历序列构造二叉树**

**利用：后序遍历与前序遍历互相转换的性质**

**性质：**

* **对于给定序列 S，定义 S' 为反序序列，**
* **定义 R 为根节点 序列，有 R = R'，定义 C 为子节点序列，正确顺序为“左-右”**
* **那么 post order 的序列顺序为 CR**

**所以如果在做pre order时生成 RC' 序列，而不是RC，那么反序之后可以得到 \(RC'\)' = CR' = CR = post order 序列**

**889. 根据前序和后序遍历构造二叉树**

**关键：**前序遍历的pre\[0\]就是后序遍历的post\[N-1\]最后一个元素

* 利用这个性质，pre\[1\]就是左子树的root
* 然后找这个元素在post中的位置，就可以得到post中左子树与右子树的各自区间
* 然后根据post左子树的位置到post\[0\]开头的这个区间就是左子树，从而也可以知道pre左子树的区间

**trick:**因为 `pre` 和 `post` 遍历中的值是不同的正整数。我们可以存一个value-&gt;index的map，减少复杂度

```cpp
class Solution {
public:
    unordered_map<int,int>mp;
    TreeNode* buildTree(vector<int>&pre, vector<int>&post, int pre_st, int pre_ed, int post_st, int post_ed){
        if(pre_st > pre_ed)return NULL;
        TreeNode* root = new TreeNode(pre[pre_st]);
        if(pre_st + 1 > pre_ed)return root;
        int index = mp[pre[pre_st+1]];
        int len = index - post_st;
        root->left =  buildTree(pre, post, pre_st+1, pre_st+1+len, post_st, index);
        root->right = buildTree(pre, post, pre_st+len+2, pre_ed, index+1, post_ed);
        return root;
    }
    TreeNode* constructFromPrePost(vector<int>& pre, vector<int>& post) {
        for(int i = 0; i < post.size(); i++){
            mp[post[i]] = i;
        }
        return buildTree(pre, post, 0, pre.size()-1, 0, post.size()-1);
    }
};
```

 **1028.从先序遍历还原二叉树**

```cpp
输入："1-2--3--4-5--6--7"
输出：[1,2,5,3,4,6,7]
```

```cpp
TreeNode* dfs(string& S, int &idx, int depth){
    if(idx == S.size())return NULL;
    int d = 0;
    int pre_idx = idx;
    while(idx < S.size() && S[idx]=='-'){
        d++;
        idx++;
    }
    if(depth > d){
        idx = pre_idx;
        return NULL;
    }
    int val = 0;
    while(idx < S.size() && S[idx]!='-'){
        val = val * 10 + S[idx] - '0';
        idx++;
    }
    TreeNode* root = new TreeNode(val);
    root->left = dfs(S, idx, depth+1);
    root->right = dfs(S, idx, depth+1);
    return root;
}
TreeNode* recoverFromPreorder(string S) {
    int idx = 0;
    return dfs(S, idx, 0);
}
```

