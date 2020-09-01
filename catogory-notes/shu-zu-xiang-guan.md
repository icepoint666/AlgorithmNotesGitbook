# 数组相关

题目：

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 面试题17.10 | 主要元素 | 摩尔投票法：时间O\(N\)空间O\(1\) | 简单 |
| 1535 | 找出数组游戏的赢家 | 看清题： 当一个整数赢得 k 个回合时 | 简单 |
|  |  |  |  |

一个结论：**寻找数组中两个元素的最大差值**，要求最小值要在最大值右边是可以**O\(n\)**的实现

```cpp
int min_ = 0x3f3f3f3f;
int max_ = -0x3f3f3f3f;
for(int i = 1; i < nums.size(); i++){
    min_ = min(min_, nums[i-1]);
    max_ = max(nums[i] - min_, max_);
}
return max_;
```

**面试题 17.10. 主要元素**

数组中占比超过一半的元素称之为主要元素

**摩尔投票法**的基本原理是，维护一个众数major和一个频数count，如果出现不同的数count加1，如果出现相同的数，count-1。最终会发现，如果存在主要元素，那么最终count一定大于0，否则一定不存在主要元素。

但仅大于0也不一定能判断确实存在主要元素，因为如果数组为\[4,3,3,2,2,2\]，会发现count为2但是，2并不是主要元素，所以还要添加验证环节

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        if(nums.empty())return -1;
        int count = 0;
        int major;
        for(int i = 0; i < nums.size(); i++){
            if(count == 0){
                major = nums[i];
                count = 1;
            }else{
                if(nums[i] == major)count++;
                else count--;
            }
        }
        if(count <= 0)return -1;
        count = 0;
        for(auto&e: nums){
            if(major == e)count++;
        }
        if(count > nums.size() / 2)return major;
        else return -1;
    }
};
```

