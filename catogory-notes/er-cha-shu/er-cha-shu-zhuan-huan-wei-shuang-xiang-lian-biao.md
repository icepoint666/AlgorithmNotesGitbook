# 二叉树转换为双向链表

牛客链接：

**两种思路：**

* 中序遍历（迭代）
* 后序遍历

**思路1：中序遍历（为什么要用中序遍历？因为）**

迭代的思路，事先申请一个dummyroot作为prev节点，之后中序遍历，跟prev连接

```cpp
TreeNode* Convert(TreeNode* pRootOfTree) {
    stack<TreeNode*>stk;
    TreeNode* cur = pRootOfTree;
    TreeNode* dummyroot = new TreeNode(0);
    TreeNode* prev = dummyroot;
    while(cur!=NULL || !stk.empty()){
        while(cur!=NULL){
            stk.push(cur);
            cur = cur->left;
        }
        cur = stk.top();
        stk.pop();
        //连接prev
        prev->right = cur;
        cur->left = prev;
        prev = cur;
        
        cur = cur->right;
    }
    TreeNode* ret = dummyroot->right;
    if(ret)ret->left = NULL; //删除的时候要注意把连接的指针设为NULL,注意自己也可能为NULL
    delete(dummyroot);
    return ret; 
}
```

思路2：后序遍历（返回区间）

```cpp
//二叉搜索树的一个性质，就是从上往下，当前访问到的left/right，一定是离root最近的点
pair<Node*, Node*> treeToDL(Node* root){
    if(root->left==NULL && root->right==NULL)return make_pair(root, root);
    if(root->left==NULL){
        pair<Node*, Node*> r_pair = treeToDL(root->right);
        r_pair.first->left = root;
        root->right = r_pair.first;
        return make_pair(root, r_pair.second);
    }
    if(root->right==NULL){
        pair<Node*, Node*> l_pair = treeToDL(root->left);
        l_pair.second->right = root;
        root->left = l_pair.second;
        return make_pair(l_pair.first, root);
    }
    pair<Node*, Node*> l_pair = treeToDL(root->left);
    pair<Node*, Node*> r_pair = treeToDL(root->right);
    l_pair.second->right = root;
    r_pair.first->left = root;
    root->left = l_pair.second;
    root->right = r_pair.first;
    return make_pair(l_pair.first, r_pair.second);
}

Node* treeToDoublyList(Node* root) {
    if(root==NULL)return NULL;
    auto tmp = treeToDL(root);
    tmp.first->left = tmp.second;
    tmp.second->right = tmp.first;
    return tmp.first;
}
```



