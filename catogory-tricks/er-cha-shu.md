# 二叉树

* 二叉树定义与声明
* 最优二叉树：哈夫曼树
* 逆波兰表达式

**二叉树定义与声明（模板）：**

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
 
int val = 10;
TreeNode* root = new TreeNode(val);
```

二叉树以及多叉树的遍历

```cpp
//二叉树
void traverse(TreeNode* root) {
    if (root == nullptr) return;
    //前序遍历执行操作
    traverse(root->left);
    //中序遍历执行操作
    traverse(root->right);
    //后序遍历执行操作
}

//多叉树
void traverse(TreeNode* root) {
    if (root == nullptr) return;
    for (child : root->children)
        traverse(child);
}
```

* 前序遍历：根，左，右
* 中序遍历：左，根，右
* 后序遍历：左，右，根

**最优二叉树：哈夫曼树**

 _哈夫曼树_是带权路径长度最短的树，权值较大的结点离根较近。

权值较大的结点离根较近。

**构建haffman树的方法：**

* **①选取当前两棵权值最小的二叉树作为左右子树构造出来一颗新的二叉树，然后将根节点写为左右子树根节点的权值之和**
* **②从F中删除这两棵树，同时将新得到的树加入到F中，就这样循环这个过程**

**逆波兰表达式**

正常的表达式生成一棵树，对这棵树做后序遍历，得到逆波兰表达式

a\*\(-b+c\) =&gt; ab-c+\*

