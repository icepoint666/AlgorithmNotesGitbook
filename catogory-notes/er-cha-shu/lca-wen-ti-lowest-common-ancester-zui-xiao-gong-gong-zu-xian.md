# LCA问题 \(lowest common ancester，最小公共祖先\)

### LCA问题，最近公共父节点 类型

**题型**

给定两个节点去寻找它们地公共父节点（祖先）

寻找某些点公共祖先

可能涉及**二叉搜索树**，**二叉树**，二叉搜索树的话会简单一些

**解决方法（多种思路都要想）**

* 递归\(recursive\) 注意：对于**尾递归要转化成迭代**
* 迭代\(iterative\)
* 对时间复杂度分析

**解题核心：对于找公共祖先问题都是后序遍历**，父节点根据左右子树的返回值，判断自己是不是满足题意的最小祖先

### 题目

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5E8F;&#x53F7;/&#x96BE;&#x5EA6;</th>
      <th style="text-align:left">&#x540D;&#x5B57;</th>
      <th style="text-align:left">&#x5907;&#x6CE8;</th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">235</td>
      <td style="text-align:left">&#x4E8C;&#x53C9;&#x641C;&#x7D22;&#x6811;&#x7684;&#x6700;&#x8FD1;&#x516C;&#x5171;&#x7956;&#x5148;</td>
      <td
      style="text-align:left">&#x9012;&#x5F52;&#xFF0C;&#x5C3E;&#x9012;&#x5F52;&#xFF0C;&#x590D;&#x6742;&#x5EA6;&#x5206;&#x6790;</td>
        <td
        style="text-align:left">
          <p>&#x6A21;&#x677F;</p>
          <p>&#x7B80;&#x5355;</p>
          </td>
    </tr>
    <tr>
      <td style="text-align:left">236/&#x5251;&#x6307;offer 68-II</td>
      <td style="text-align:left">&#x4E8C;&#x53C9;&#x6811;&#x7684;&#x6700;&#x8FD1;&#x516C;&#x5171;&#x7956;&#x5148;</td>
      <td
      style="text-align:left">
        <p>&#x66B4;&#x529B;&#x9012;&#x5F52;&#x6CD5;O(nlogn)&#xFF0C;
          <br /><b>&#x540E;&#x5E8F;&#x904D;&#x5386;</b>&#x4F18;&#x5316;&#x9012;&#x5F52;O(n)
          <br
          />&#x4E2D;&#x5E8F;&#x904D;&#x5386;&#x9884;&#x5904;&#x7406;&#x7684;&#x8FED;&#x4EE3;&#x505A;&#x6CD5;</p>
        <p>&#x65F6;&#x95F4;&#x590D;&#x6742;&#x5EA6;&#x5206;&#x6790;&#x601D;&#x8DEF;&#xFF01;</p>
        </td>
        <td style="text-align:left">
          <p>&#x6A21;&#x677F;</p>
          <p>&#x4E2D;&#x7B49;</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">865/1123</td>
      <td style="text-align:left">&#x5177;&#x6709;&#x6240;&#x6709;&#x6700;&#x6DF1;&#x8282;&#x70B9;&#x7684;&#x6700;&#x5C0F;&#x5B50;&#x6811;</td>
      <td
      style="text-align:left">level order traversal&#x7684;&#x505A;&#x6CD5;</td>
        <td style="text-align:left">&#x8BB0;&#x5FC6;</td>
    </tr>
  </tbody>
</table>

**235. 二叉搜索树的最近公共祖先**

LCA 问题在 BST 里就是看给定节点val相对 node.val 的大小就可以。

**1）递归**

```cpp
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Got to check if p and q is null as well;
        if(root == null) return root;

        if(root.val > p.val && root.val > q.val) 
            return lowestCommonAncestor(root.left, p, q);
        if(root.val < p.val && root.val < q.val) 
            return lowestCommonAncestor(root.right, p, q);

        return root;
    }
}
```

**2）因为是尾递归！！**

**用 while循环省去递归占用的系统栈空间（每次调用函数都要加一个函数栈）；**

```cpp
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Got to check if p and q is null as well;
        while(root != null){
            if(root.val > p.val && root.val > q.val) root = root.left;
            else if(root.val < p.val && root.val < q.val) root = root.right;
            else return root;
        }
        return root;
    }
}
```

**时间复杂度 O\(n\)，如果 Tree 的形状是一条线往左或右的 full binary tree 的话。**

\*\*\*\*

**236. 二叉树的最近公共祖先**

**自己做法：3个left, myself, right出现2个true的时候\(有p, 有q\)，就表明当前root是根节点**

```cpp
class Solution {
public:
    TreeNode* res;
    bool find(TreeNode* root, TreeNode* p, TreeNode* q){
        if(root == NULL || res)return false;
        bool m = (root == p || root == q);
        bool l = find(root->left, p, q);
        bool r = find(root->right, p, q);
        if(l && r || l && m || r && m){
            res = root;
        }
        return l || r || m;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        bool _ = find(root, p, q);
        return res;
    }
};
```

**1）暴力解法**：判断子树中 contains 的时间复杂度太高了，而且重复调用很多，完全没优化。

**暴力解法的时间复杂度是 O\(n log n\)，因为对于一个 node 来讲，它被 check 的次数等于它和 root 的距离 \(也就是 height\)。**

```cpp
 public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) return root;

        if(root.val == p.val) return p;
        if(root.val == q.val) return q;

        if(containsNode(root.left, p) && containsNode(root.left, q)){
            return lowestCommonAncestor(root.left, p, q);
        }
        if(containsNode(root.right, p) && containsNode(root.right, q)){
            return lowestCommonAncestor(root.right, p, q);
        }

        return root;
    }

    private boolean containsNode(TreeNode root, TreeNode node){
        if(root == null) return false;
        if(root.val == node.val) return true;

        return (containsNode(root.left, node) || containsNode(root.right, node));

    }
}
```

加 containsNode\(\) 函数比较多此一举，其实可以直接用这个函数在 binary tree 上的递归性质去调用自身解决。

**2）优化核心**：

函数返回的是 "**对于给定 Node 为 root 的 tree 中是否包含 p 或者 q，只要包含一个，就不返回 null 了**，而我们相对于当前节点为 root 的结果，就看两边递归的 return value 决定."

**时间复杂度分析：O\(n\)，**

**思路1：相对于每个 node 来讲，只会被函数调用和计算一次。**

**思路2：这题的递归结构不过是个 post-order traversal，遍历的复杂度当然是 O\(n\).**

**递归（也就是post-order traversal）写法**

```cpp
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == q || root == p) return root;

        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if(left != null && right != null) return root;
        else return (left == null) ? right : left;
    }
}
```

**3）迭代**

一种可能的迭代的写法是借鉴了教授 Martin Farach-Colton 的那篇 [The LCA Problem Revisited](http://www.ics.uci.edu/~eppstein/261/BenFar-LCA-00.pdf) 的 idea，就是先对 Tree 做 pre-processing 然后用 array 保存过渡结果方便快速 query.

**其实就是 inorder pre-process 了一遍之后，把 binary tree 当 BST 用。因为 in-order 的 index 就像 BST 里的大小一样，可以直接确定几个节点在树中的相对位置。同时因为我们都是从 root 开始一层一层往下搜索，也能保证每次循环的 root 都一定 valid.**

这个代码的**优点**是如果做多次 query 的话有一个 pre-processing 的缓存可以很快返回结果；**缺点**就是多了个 pre-processing 的过程，query 次数少的时候不占便宜。

**基本思路**

**①**就是先跑一遍 inorder traversal 记录下每个 node 在整个树里面对应的位置（中序遍历的顺序index）；  
②可以利用 hashmap（存index与TreeNode\*\) 对每个 node 实现均摊复杂度 O\(1\) 的查找  
③根据对应的节点 index 判断 p，q 相对于 root 在 tree 里的位置关系。

**代码**：**如果在同一个 Tree 上要跑好多次 LCA，这个做法还是比较可取的~**

```cpp
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) return root;
        if(root.val == p.val) return p;
        if(root.val == q.val) return q;

        HashMap<TreeNode, Integer> map = new HashMap<TreeNode, Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode>();

        TreeNode cur = root;
        int index = 0;
        while(cur != null || !stack.isEmpty()){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            TreeNode node = stack.pop();
            map.put(node, index++);
            cur = node.right;
        }

        return getLCA(root, p, q, map);
    }

    private TreeNode getLCA(TreeNode root, TreeNode p, TreeNode q, HashMap<TreeNode, Integer> map){
        if(root == null) return root;
        if(root.val == p.val) return p;
        if(root.val == q.val) return q;

        while(root != null){
            if(map.get(q) < map.get(root) && map.get(p) < map.get(root)){
                root = root.left;
            } else if(map.get(q) > map.get(root) && map.get(p) > map.get(root)){
                root = root.right;
            } else {
                break;
            }
        }

        return root;
    }
```

**865. 具有所有最深节点的最小子树**

**题意：** 返回能满足 **以该节点为根的子树中包含所有最深的节点** 这一条件的具有最大深度的节点

**1）递归：（对于找公共祖先问题，都是后序遍历）**

**关键思路：参数①包含一个当前node层的深度，参数②表示向下遍历后返回的当前node的高度**

**根据左右子树的高度**

* 如果左右等高，就返回当前node
* 如果左高，就返回左树刚刚返回的node
* 如果右高，就返回右树刚刚返回的node

```cpp
TreeNode* subtree(TreeNode* root, int depth, int& num){
        if(root==NULL)return NULL;
        if(!root->left && !root->right){
            num = depth;
            return root;
        }
        int num_l = depth, num_r = depth;
        TreeNode* _left, * _right;
        if(root->left) _left = subtree(root->left, depth+1, num_l);
        if(root->right) _right = subtree(root->right, depth+1, num_r);
        num = max(num_l, num_r);
        if(num_l == num_r)return root;
        else if(num_l > num_r)return _left;
        else return _right;
    }
    TreeNode* subtreeWithAllDeepest(TreeNode* root) {
        int tmp;
        return subtree(root, 0, tmp);
    }
```

**2）递归/迭代 都可以通过level order traversal的思路**

#### 关键点：意识到只需要求最底层 level 最左面和最右面节点的 LCA 就可以了 <a id="&#x8FD9;&#x9898;&#x7684;&#x5173;&#x952E;&#x70B9;&#x53EA;&#x6709;&#x4E00;&#x4E2A;&#xFF1A;&#x610F;&#x8BC6;&#x5230;&#x53EA;&#x9700;&#x8981;&#x6C42;&#x6700;&#x5E95;&#x5C42;-level-&#x6700;&#x5DE6;&#x9762;&#x548C;&#x6700;&#x53F3;&#x9762;&#x8282;&#x70B9;&#x7684;-lca-&#x5C31;&#x53EF;&#x4EE5;&#x4E86;&#x3002;"></a>

* **如何递归求 level order 和 LCA**
* **如何迭代求 level order 和 LCA**

