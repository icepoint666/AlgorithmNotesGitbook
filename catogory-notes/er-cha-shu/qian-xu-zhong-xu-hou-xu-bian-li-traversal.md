# 前序，中序，后续遍历（Traversal）

**前序\(preorder\)，中序\(inorder\)，后序\(postorder\)遍历** 

**掌握递归\(recursive\)方法 以及 使用迭代\(iterative\)方法**

**前序preorder：**

* 递归：self-&gt;left-&gt;right
* 迭代：**直接用stack（开栈的迭代都不是好的迭代）**
* **parent指针**：数据结构加一个oarent指针的开销，不用开栈的**迭代方法**

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
    // current
    vector<int> res = {root->val};
    // left  
    vector<int> left = preorderTraversal(root->left);
    res.insert(res.end(), left.begin(), left.end());
    // right
    vector<int> right = preorderTraversal(root->right);
    res.insert(res.end(), right.begin(), right.end());
    return res;
}
```

**迭代模板（stack）需要记忆一下！**

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    stack <TreeNode*> s;
    vector<int>res;
    if(root!=NULL)s.push(root);
    while(!s.empty()){
        TreeNode* node = s.top();
        s.pop();
        res.push_back(node->val);
        //因为 stack 先入后出的特点，push 的时候先 push 右边的就行
        if(node->right)s.push(node->right);
        if(node->left)s.push(node->left);
    }
    return res;
}
```

**parent指针**

**主要思路就是node不仅记录left,right,val，还要记录一个parent用于回溯需要**

**（只能自己测几个样例，没法在leetcode上测试）**

* 处理当前node
* if 如果有left，node = node-&gt;left
* else if 如果有right, node = node-&gt;right
* else 向上回溯（可能是由左树回溯，也可能是右树回溯）
  * 对于左树回溯但是无右树，或者，本身就是从右树回溯的，这种需要向上继续回溯到parent
  * 找到有效的右树，处理右树

```cpp
void preorder(TreeNode* root) {
 
        TreeNode* cur = root;
        while(cur != NULL){
            System.out.println(cur.val);

            if(cur.left != null){
                cur = cur.left;
            } else if(cur.right != null){
                cur = cur.right;
            } else {
                while(cur.parent != null && (cur.parent.right == null || cur == cur.parent.right)){
                    cur = cur.parent;
                }
                if(cur.parent == null) break;
                cur = cur.parent.right;
            }
        }
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

