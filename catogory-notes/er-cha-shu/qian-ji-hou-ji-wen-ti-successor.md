# 前继，后继问题\(successor\)

**二叉搜索树**中**指定节点**的**“下一个”节点**（也即中序后继）

之前介绍中序遍历的parent指针写法的时候，是依赖parent指针实现的中序后继，但是本题没有parent指针

但是这题是BST，**BST的好处是：可以通过比较数值大小来确定相对位置**

**BST的中序后继模板**

其实思路很简单：

* 如果cur-&gt;val 小于等于 指定节点p-&gt;val ，就走右子树
* 如果cur-&gt;val &gt; p-&gt;val ，就先把当前节点暂存为res，然后继续走左子树，找leftmost

```cpp
TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
    TreeNode* res = NULL;
    TreeNode* cur = root;
    while(cur!=NULL){
        if(cur->val > p->val){
            res = cur;
            cur = cur->left;
        }else cur = cur->right;
    }
    return res;
}
```

**BST的前序后继模板**

类似的如果需要找指定节点的前继，也是类似的思路，相当于往左走的时候不记录res，往右走的时候记

```cpp
TreeNode* presuccessor(TreeNode* root, TreeNode* p){
    TreeNode* res = NULL;
    TreeNode* cur = root;
    while(cur!=NULL){
        if(cur->val >= p->val)
            cur = cur->left;
        else{ 
            res = cur;
            cur = cur->right;
        }
    }
    return res;
}
```

\*\*\*\*

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 面试题04.06 | 二叉搜索树的中序后继 | BST上不依赖parent pointer完成中继后续 | 中等 |



