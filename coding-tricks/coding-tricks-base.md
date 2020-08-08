# Base

**数组arr\[\]长度：**

```cpp
int arr[10];
//确定长度大于0
int len = sizeof(arr) / sizeof(arr[0]);
```

**判断vector数组是否为空：**

```cpp
vector<int>nums;
//方法1
if(nums.size()==0){}
//方法2
if(nums.begin()==nums.end()){}
```

**swap指针**

```cpp
//剑指 Offer 27. 二叉树的镜像 的代码举例
TreeNode *temp = mirrorTree(root->left);
root->left = mirrorTree(root->right);
root->right = temp;
```

