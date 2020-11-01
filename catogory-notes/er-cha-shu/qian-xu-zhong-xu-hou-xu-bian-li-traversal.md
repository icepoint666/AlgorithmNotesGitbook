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

类似于前面，用两个函数实现或者一个函数都可以，比较简单



**迭代模板**（在leetcode是hard）

**stack + cur + prev**

注意点：

* ①每一步用 stack 顶元素作为新的 cur，在 while 循环中只需要以 !stack.isEmpty\(\) 为条件去判断
  * 用一个 stack 存着所有的 candidate node，栈顶为当前 candidate，并且以栈是否为空做唯一判断标准
* ②post-order 是【左，右，中】，压栈的时候还是先压右再压左
* **③最重要的是 prev 与 cur 的相对关系，相当于存了上一步的动作，用作下一步的方向**
  * prev表示上一次访问的节点，三种可能情况需要讨论
    * cur在prev的下面
      * 如果有左子树，开始看左边
      * 如果没有左子树，那么直接看右边
      * 如果没有左右子树，表明是叶节点（易错）不用操作！！给下一轮去操作
    * prev是cur的左子树，接下来要看右边
    * prev是cur的右子树/prev就是自己（前一步的遗留），这时候需要把cur写回result

```cpp
vector<int> postorderTraversal(TreeNode* root) {
    vector<int>res;
    stack<TreeNode*>s;
    TreeNode* prev = NULL;
    if(root!=NULL)s.push(root);
    while(!s.empty()){
        TreeNode* cur = s.top();
        if(prev==NULL || prev->left == cur || prev->right == cur){ //1
            if(cur->left)s.push(cur->left);
            else if(cur->right)s.push(cur->right); 
        }else if(cur->left == prev){//2
            if(cur->right)s.push(cur->right);
        }else{//3
            res.push_back(cur->val);
            s.pop();
        }
        prev = cur; //update
    }
    return res;
}
```



**利用preorder序列与postorder序列的关系**（比较巧）

**性质：**

* **对于给定序列 S，定义 S' 为反序序列，**
* **定义 R 为根节点 序列，有 R = R'，定义 C 为子节点序列，正确顺序为“左-右”**
* **那么 post order 的序列顺序为 CR**

**所以如果在做pre order时生成 RC' 序列，而不是RC，那么反序之后可以得到 \(RC'\)' = CR' = CR = post order 序列**

```cpp
vector<int> postorderTraversal(TreeNode* root) {
        vector<int>res;
        stack<TreeNode*>s;
        if(root!=NULL)s.push(root);
        while(!s.empty()){
            TreeNode* node = s.top();
            res.push_back(node->val);
            s.pop();
            //先压左，再压右，因为这次是要先看右边
            if(node->left)s.push(node->left);
            if(node->right)s.push(node->right);
        }
        reverse(res.begin(), res.end());
        return res;
    }
```

### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 144 | 二叉树的前序遍历 | 递归，迭代，parent指针迭代 | 模板 |
| 94 | 二叉树的中序遍历 | 递归，迭代，parent指针迭代（难） | 模板 |
| 145 | 二叉树的后序遍历 | 递归，迭代（难），前序遍历迭代处理（巧） | 模板 |
| 105 | 前序 + 中序 构造二叉树 |  |  |
| 106 | 中序 + 后序 构造二叉树 |  |  |
| 889 | 前序 + 后序 构造二叉树 |  |  |

### 前序，中序，后序任意两个构造二叉树

只记忆思路

