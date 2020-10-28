# LCA问题 \(lowest common ancester，最小公共祖先\)

### LCA问题，最近公共父节点 类型

**题型**

给定两个节点去寻找它们地公共父节点（祖先）

寻找某些点公共祖先

可能涉及**二叉搜索树**，**二叉树**，二叉搜索树的话会简单一些

**解决方法（多种思路都要想）**

* 递归\(recursive\) 注意：对于尾递归要转化成迭代
* 迭代\(iterative\)
* 对时间复杂度分析

**解题核心：后序返回的时候**，父节点根据左右子树的返回值，判断自己是不是满足题意的最小祖先

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
        style="text-align:left">&#x7B80;&#x5355;</td>
    </tr>
    <tr>
      <td style="text-align:left">236/&#x5251;&#x6307;offer 68-II</td>
      <td style="text-align:left">&#x4E8C;&#x53C9;&#x6811;&#x7684;&#x6700;&#x8FD1;&#x516C;&#x5171;&#x7956;&#x5148;</td>
      <td
      style="text-align:left">
        <p>&#x66B4;&#x529B;&#x6CD5;O(nlogn)&#xFF0C;&#x4F18;&#x5316;O(n)</p>
        <p>&#x65F6;&#x95F4;&#x590D;&#x6742;&#x5EA6;&#x5206;&#x6790;&#x601D;&#x8DEF;&#xFF01;</p>
        </td>
        <td style="text-align:left">&#x4E2D;&#x7B49;</td>
    </tr>
    <tr>
      <td style="text-align:left">865/1123</td>
      <td style="text-align:left">&#x5177;&#x6709;&#x6240;&#x6709;&#x6700;&#x6DF1;&#x8282;&#x70B9;&#x7684;&#x6700;&#x5C0F;&#x5B50;&#x6811;</td>
      <td
      style="text-align:left"></td>
        <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

**235. 二叉搜索树的最近公共祖先**

LCA 问题在 BST 里就是看给定节点val相对 node.val 的大小就可以。

**递归**

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

**因为是尾递归！！**

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

**暴力解法**：判断子树中 contains 的时间复杂度太高了，而且重复调用很多，完全没优化。

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

**优化核心**：

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

**迭代**

一种可能的迭代的写法是借鉴了教授 Martin Farach-Colton 的那篇 [The LCA Problem Revisited](http://www.ics.uci.edu/~eppstein/261/BenFar-LCA-00.pdf) 的 idea，就是先对 Tree 做 pre-processing 然后用 array 保存过渡结果方便快速 query.

**其实就是 inorder pre-process 了一遍之后，把 binary tree 当 BST 用。因为 in-order 的 index 就像 BST 里的大小一样，可以直接确定几个节点在树中的相对位置。同时因为我们都是从 root 开始一层一层往下搜索，也能保证每次循环的 root 都一定 valid.**

这个代码的优点是如果做多次 query 的话有一个 pre-processing 的缓存可以很快返回结果；缺点就是多了个 pre-processing 的过程，query 次数少的时候不占便宜。

基本思路就是先跑一遍 inorder traversal 记录下每个 node 在整个树里面对应的位置；利用 hashmap 对每个 node 实现均摊复杂度 O\(1\) 的查找，然后根据对应的节点 index 判断 p，q 相对于 root 在 tree 里的位置关系。

