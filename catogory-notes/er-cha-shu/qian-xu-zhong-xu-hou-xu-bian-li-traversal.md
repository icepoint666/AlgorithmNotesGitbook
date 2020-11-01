# 前序，中序，后续遍历（Traversal）

**前序\(preorder\)，中序\(inorder\)，后序\(postorder\)遍历** 

**掌握递归\(recursive\)方法 以及 使用迭代\(iterative\)方法**

**前序preorder：**

* 递归：self-&gt;left-&gt;right
* 迭代：**直接用stack**
* **parent指针**：**不开空间的迭代方法**，最高效！

**中序inorder:**

* 递归：left-&gt;self-&gt;right
* 迭代：**用stack + cur**
* \*\*\*\*

**后序postorder:**

* 递归：left-&gt;right-&gt;self
* 迭代：**用stack + cur + prev**

**重点在于迭代的模板，需要掌握迭代的写法**

### 前序遍历

**递归模板**

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    if(root==NULL)return vector<int>();
    # current
    vector<int> res = {root->val};
    # left  
    vector<int> left = preorderTraversal(root->left);
    res.insert(res.end(), left.begin(), left.end());
    # right
    vector<int> right = preorderTraversal(root->right);
    res.insert(res.end(), right.begin(), right.end());
    return res;
}
```

### 中序遍历

### 后序遍历

### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 144 | 二叉树的前序遍历 | 递归，迭代，parent指针迭代，复杂度分析 | 模板 |

### 前序遍历



#### 中序遍历 迭代模板

```cpp
Stack<TreeNode> stack = new Stack<TreeNode>();

TreeNode cur = root;
while(cur != null || !stack.isEmpty()){
    while(cur != null){
        stack.push(cur);
        cur = cur.left;
    }
    TreeNode node = stack.pop();
    //执行操作
    cur = node.right;
}
```

