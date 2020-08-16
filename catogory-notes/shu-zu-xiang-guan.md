# 数组相关

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



