# 如何泛化的将一个问题的递归解法转为迭代解法

**例题：给定 Binary Tree，print 所有 root - leaf 的 path. \(Leetcode 257\)**

```text
          A
         / \
        B   D
       /   / \
      C   E   F
```

* Input : Root A
* Output :
  * A,B,C
  * A,D,E
  * A,D,F

**递归：**维护一个记录路径的vector，并作为全局变量，到叶子节点输出一次当前vector的值

（经典DFS + backtrack）

**迭代：stack + 自定义stackframe对象**

**比较有意思并且很容易泛化到其他**递归解法中

StackFrame对象包含一个int status以及一个TreeNode\*指针

**对于二叉树**，我们可以这样定义 StackFrame 的三个状态：

* **刚入栈，未访问子树**
* **正在访问左子树，返回代表左子树访问完毕**
* **正在访问右子树，返回代表右子树访问完毕**

**解释：每次vector里存的依旧是当前路径，每次栈顶的 StackFrame 都记录了当前 node 的访问状态和下一步的动向**（这种 0/1/2 的状态表示方式看着有点像 Graph 里面做 DFS 的0,1,2,3对应上下左右的标注）

**对于多叉树，**这次 status 代表着 “下一个要访问的 child index”，当 index == children.size\(\) 的时候，我们就可以知道当前 node 的所有子节点都访问完了**。**

```java
class Solution {
public:
    struct StackFrame{
        int status;
        TreeNode* node;
        StackFrame(int x, TreeNode* n):status(x),node(n){}
    };
    vector<string> binaryTreePaths(TreeNode* root) {
        stack<StackFrame*>s;
        vector<int>path;
        vector<string>res;
        if(root!=NULL)s.push(new StackFrame(0,root));//不能创建object，而应该创建new Object
        
        while(!s.empty()){
            StackFrame* curframe = s.top();
            TreeNode* cur = curframe->node;
            if(cur == NULL){
                s.pop();
            }else if(curframe->status == 0){
                path.push_back(cur->val);
                curframe->status = 1; //需要注意如果是StackFrame curframe 而不是StackFrame* curframe是会有问题的
                s.push(new StackFrame(0, cur->left));
            }else if(curframe->status == 1){
                curframe->status = 2;
                s.push(new StackFrame(0, cur->right));
            }else{
                if(!cur->left && !cur->right){
                    string ss = to_string(path[0]);
                    for(int i = 1; i < path.size(); i++){
                        ss += "->" + to_string(path[i]);
                    }
                    res.push_back(ss);
                }
                s.pop();
                path.pop_back();
            }
        }
        return res;
    }
};
```



