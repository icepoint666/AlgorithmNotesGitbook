# 前序，中序，后续遍历（Traversal）

**前序\(preorder\)，中序\(inorder\)，后序\(postorder\)遍历** 

**掌握递归\(recursive\)方法 以及 使用stack的迭代\(iterative\)方法**

**前序preorder：**

* 递归：self-&gt;left-&gt;right
* 迭代：**直接用stack**

**中序inorder:**

* 递归：left-&gt;self-&gt;right
* 迭代：**用stack + cur**

**后序postorder:**

* 递归：left-&gt;right-&gt;self
* 迭代：**用stack + cur + prev**

**重点在于迭代的模板，需要掌握迭代的写法**

#### **前序**遍历 迭代模板

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
      <td style="text-align:left">144</td>
      <td style="text-align:left">&#x4E8C;&#x53C9;&#x6811;&#x7684;&#x524D;&#x5E8F;&#x904D;&#x5386;</td>
      <td
      style="text-align:left">&#x524D;&#x5E8F;&#x904D;&#x5386;&#x7684;&#x8FED;&#x4EE3;&#x6A21;&#x677F;</td>
        <td
        style="text-align:left">&#x6A21;&#x677F;</td>
    </tr>
  </tbody>
</table>

前序遍历

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

