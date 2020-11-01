# 前序，中序，后续遍历（Traversal）

**前序\(preorder\)，中序\(inorder\)，后序\(postorder\)遍历** 

**掌握递归\(recursive\)方法 以及 使用迭代\(iterative\)方法**

**前序preorder：**

* 递归：self-&gt;left-&gt;right
* 迭代：**直接用stack（开栈的迭代都不是好的迭代）**
* **parent指针**：数据结构加一个parent指针的开销，不用开栈的**迭代方法**

**中序inorder:**

* 递归：left-&gt;self-&gt;right
* 迭代：**用stack + cur**
* **parent指针：**比较有技巧型**（基本等价于inorderSuccessor\)**

**后序postorder:**

* 递归：left-&gt;right-&gt;self
* 迭代：**用stack + cur + prev（leetcode hard\)**

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
* else 都没有，就向上回溯（可能是自己是左树回溯，也可能自己是右树回溯）
  * 如果是左树回溯但是无右树，或者，自己本身就是右树回溯的，这种需要向上继续回溯到parent
  * 找到有效的右树，处理右树

（判定cur-&gt;parent!=NULL，防止到达根节点）

```cpp
vector<int> preorder(TreeNode* root) {
    vector<int>res;
    TreeNode* cur = root;
    while(cur != NULL){
        res.push_back(cur->val);
        if(cur->left != NULL){
            cur = cur->left;
        } else if(cur->right != NULL){
            cur = cur->right;
        } else {
            while(cur->parent != NULL && (cur->parent->right == NULL || cur == cur->parent->right)){
                cur = cur->parent;
            }
            if(cur->parent == NULL) break;
            cur = cur->parent->right;
        }
    }
    return res;
}
```

### 中序遍历

**递归模板：**

**（选择分成两个函数实现，前序，后序也可以这样实现）**

```cpp
vector<int>res;
void inorder(TreeNode* root){
    if(root==NULL)return;
    inorder(root->left);
    res.push_back(root->val);
    inorder(root->right);
}
vector<int> inorderTraversal(TreeNode* root) {
    inorder(root);
    return res;
}
```

**迭代模板:**

**两个while循环，都是cur!=NULL**

**内嵌的while就是要一直挖，找到最左边节点**

```cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int>res;
    stack<TreeNode*>s;
    TreeNode* cur = root;
    while(cur!=NULL || !s.empty()){
        while(cur!=NULL){
            s.push(cur);
            cur = cur->left;
        }
        cur = s.top();
        s.pop();
        res.push_back(cur->val);
        cur = cur->right;
    }
    return res;
}
```

**parent指针（类似于 找到二叉搜索数的中序后继 ）**

cur指针，比较有技巧性（思路是这样的）

* ①（初始）首先找到leftmost节点作为current，然后访问添加到vector中
  * 可以确定的是，current必然不会有左子树了
* 接下来进入大的while循环，寻找**inorderSuccessor**\(**寻找中序后继节点**）
* ②如果current有右子树，接下来找current的右子树的leftmost，继续这样操作
* ③如果current没有右子树
  * 如果current就是它父亲的右子树，那么要一直网上找，直到找到此树作为左子树的那个parent
  * 然后cur=cur-&gt;parent（每次执行这个操作都需要判定一下保证cur不是root\)

（判断cur-&gt;parent!=NULL，防止到达根节点）

```cpp
TreeNode* inorderSuccessor(TreeNode* cur){
    if(cur->right!=NULL){
        //find leftmost node in rightsubtree
        cur = cur->right;
        while(cur!=NULL)cur = cur->left;
    } else{
        //if cur is rightsubtree, continue to parent's parent
        while(cur->parent!=NULL && cur == cur->parent->right)cur = cur->parent;
        if(cur->parent==NULL)return NULL; //if cur is not root
        cur = cur->parent;
    }
    return cur;
}
vector<int> inorderTraversal(TreeNode* root){
    vector<int>res;
    TreeNode* cur = root;
    //Current is at leftmost pos
    while(cur!=NULL)cur = cur->left;
    
    while(cur!=NULL){
        //left most is push_back
        res.push_back(cur->val);
        cur = inorderSuccessor(cur);
    }
    return res;
}
```

### 后序遍历

**递归模板**

```cpp

```

### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 144 | 二叉树的前序遍历 | 递归，迭代，parent指针迭代 | 模板 |
| 94 | 二叉树的中序遍历 | 递归，迭代，parent指针迭代（难） | 模板 |
| 145 | 二叉树的后序遍历 | 递归，迭代（难） | 模板 |
| 106 | 中序 + 后序 构造二叉树 |  |  |
| 889 |  |  |  |

### 

