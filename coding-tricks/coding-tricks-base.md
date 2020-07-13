# Base

**数组arr\[\]长度：**

```text
int arr[10];
//确定长度大于0
int len = sizeof(arr) / sizeof(arr[0]);
```

**判断vector数组是否为空：**

```text
vector<int>nums;
//方法1
if(nums.size()==0){}
//方法2
if(nums.begin()==nums.end()){}
```

