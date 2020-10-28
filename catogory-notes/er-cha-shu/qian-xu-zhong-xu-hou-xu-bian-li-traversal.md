# 前序，中序，后续遍历（Traversal）

### 前序\(preorder\)，中序\(inorder\)，后续遍历\(postorder\) 

### 掌握递归\(recursive\)方法 以及 使用stack的迭代\(iterative\)方法

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

