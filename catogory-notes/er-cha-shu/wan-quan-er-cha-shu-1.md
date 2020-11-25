# 完全二叉树

**完全二叉树**就是节点前面都排满了，只有最后一层的节点没有排满

最后一层节点也排满的就是**满二叉树**

**遍历完全二叉树**的做法比较理想的就是**层序遍历level-order traserval**

**完全二叉树的性质：**

* 左子树 与 右子树至少有一个是满二叉树（也可能两个都是）



### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 222 | 完全二叉树的节点个数 | 完全二叉树性质 | 中等 |

**222. 完全二叉树的节点个数**

因为根据性质：左子树 右子树至少有一个是满二叉树

计算左子树 右子树 **最左节点的高度leftmostheight** 

* 如果左右高度相等，表明不完全部分只会在右树部分，继续递归计算右子树完全二叉树的节点数，左子树不用算，等于1 &lt;&lt; h
* 如果左右高度不等，表明不完全的树在左树部分，递归计算左子树的完全二叉树节点数

```cpp
int getLeftMostHeight(TreeNode* root){
    int h = 0;
    while(root){
        h++;
        root = root->left;
    }
    return h;
}
int countNodes(TreeNode* root) {
    if(!root)return 0;
    int l = getLeftMostHeight(root->left);
    int r = getLeftMostHeight(root->right);
    if(l == r)return countNodes(root->right) + (1 << l);
    return countNodes(root->left) + (1 << r);
}
```

