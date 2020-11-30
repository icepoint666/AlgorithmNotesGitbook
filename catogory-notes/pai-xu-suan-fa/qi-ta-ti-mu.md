# 归位排序

**题型：**给定一个范围在 1 ≤ a\[i\] ≤ n \( n = 数组大小 \) 的 整型数组

其中有重复元素，如何将重复元素检测出来，而且一般这类题要求：**时间O\(N\), 空间O\(1\)**

**基本思路：交换！**

我们现在是\[4,3,2,7,8,2,3,1\]，没有缺失项的正确的顺序应该是\[1,2,3,4,5,6,7,8\]

我们需要把数字移动到正确的位置上去，**将nums\[i\]置换到其对应的位置nums\[nums\[i\]-1\]上去**

```cpp
while(nums[i] != nums[nums[i]-1])
    swap(nums[i], nums[nums[i]-1]);
```

最后得到的顺序应该是\[1,2,3,4,3,2,7,8\]，我们最后在对应位置检验，如果nums\[i\]和i+1不等，那么我们将nums\[i\]存入结果res中即可

### **题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 442 | 数组中的重复数据 | 快排变体：**聚集元素** | 经典 |
| 448 | 找到所有数组中消失的数字 | 归位排序\(与模板一样\) | 经典 |
| 41 | 缺少的第一个正数 | 归位排序 | 中等 |

**442. 数组中重复的数据**

```cpp
vector<int> findDuplicates(vector<int>& nums) {
    int n = nums.size();
    for(int i = 0; i < n; i++){
        while(nums[i] != nums[nums[i]-1])
            swap(nums[i], nums[nums[i]-1]);
    }
    vector<int>res;
    for(int i = 0; i < n; i++){
        if(nums[i] != i + 1)
            res.push_back(nums[i]);
    }
    return res;
}
```

41.**缺失的第一个正数**

给你一个未排序的整数数组，请你找出其中没有出现的最小的正整数。

```text
输入: [3,4,-1,1]
输出: 2
```

```text
输入: [7,8,9,11,12]
输出: 1
```

**解法：归位排序**

* 如果nums\[i\]在\[1,n\]范围，就放在对应位置
* 如果nums\[i\]不在这个范围就不用管了（也不用处理）
* 最后检查一遍不属于1,2,3..对应位置的第一个数就可以了

```cpp
int firstMissingPositive(vector<int>& nums) {
    int len = nums.size();
    if(len == 0)return 1;
    for(int i = 0; i < len; i++){ //如果在[1,n]范围，就放在对应位置
        while(nums[i] <= len && nums[i] >= 1 &&nums[i] != nums[nums[i]-1])
            swap(nums[i], nums[nums[i]-1]);
    }
    int val = 1;
    while(val <= len){//检查不属于对应1,2,3位置的第一个元素
        if(val!=nums[val-1])break;
        val++;
    }
    return val;
}
```

