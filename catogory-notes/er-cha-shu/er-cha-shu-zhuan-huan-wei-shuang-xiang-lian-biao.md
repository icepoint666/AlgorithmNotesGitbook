# 二叉树转换为双向链表

两种思路：

* 中序遍历（推荐）
* 后序遍历

思路1：中序遍历（存一个pre,head, dfs出来之后再连接pre与head）

```cpp
struct Node{
    int val;
    Node* left;
    Node* right;
};
Node* head, *pre;
void dfs(Node* cur) {
    if(!cur) return;
    dfs(cur->left);
    //如果没有pre的话，就是只用让cur = head就好了，不然的话，就是需要用Pre连到cur
    if(pre) pre->right = cur;
    else head = cur;
    cur->left = pre;
    pre = cur;
    dfs(cur->right);
}
Node* treeToDoublyList(Node* root) {
    if(!root)return root;
    dfs(root);
    head->left = pre;
    pre->right = head;
    return head;
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



