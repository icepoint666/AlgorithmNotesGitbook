# 二叉树定义题

**树根节点相对于leftmost高度：**

**递归**

```cpp
int height(TreeNode* root){
    if(!root)return 0;
    return height(root->left)+1;
}
```

**迭代**

```cpp
int height(TreeNode* root){
    int h = 0;
    while(root!=NULL){
        h++;
        root = root->left;
    }
    return h;
}
```

### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 543 | 二叉树的直径 | 双重递归\(外面维护最大值，注意坑：直径是节点数要减1） | 简单 |



