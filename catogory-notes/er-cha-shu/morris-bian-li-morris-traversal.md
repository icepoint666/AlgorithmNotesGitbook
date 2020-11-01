# Morris遍历\(Morris Traversal\)

### [Morris Traversal方法遍历二叉树（非递归，不用栈，O\(1\)空间）](https://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html)

本文主要解决一个问题，如何实现二叉树的前中后序遍历，并且保证：

1. **O\(1\)空间复杂度**，即只使用常数空间；

2. 二叉树的数据结构不会被改变（node中不用新添加额外指针，中间过程允许改变其形状）

**通常遍历的方法：**

* 递归\(recursive\)
* 栈实现的迭代版本\(stack+iterative\)

这两种方法都是O\(n\)的空间复杂度（递归本身占用stack空间或者用户自定义的stack），所以不满足要求1

* parent指针

这种需要添加额外指针

**核心思路**

该方法只需要O\(1\)空间，而且同样可以在O\(n\)时间内完成。

要使用O\(1\)空间进行遍历，**最大的难点**在于，**遍历到子节点的时候怎样重新返回到父节点**（假设节点中没有指向父节点的p指针），由于不能用栈作为辅助空间

**解决办法**

用到了[线索二叉树](http://en.wikipedia.org/wiki/Threaded_binary_tree#The_array_of_Inorder_traversal)（threaded binary tree）的概念。在Morris方法中不需要为每个节点额外分配指针指向其前驱（predecessor）和后继节点（successor），只需要利用叶子节点中的左右空指针指向某种顺序遍历下的前驱节点或后继节点就可以了。

