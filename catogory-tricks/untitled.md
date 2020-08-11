# 单调栈

> 最大值最小值会随着加入/删除元素动态更新
>
> 单调栈就是有记录早期加入的最值元素，在被移除时，单调栈能记忆并更新这个最值的变化

**为什么是栈？**压入的是尾部，删除的也是尾部

**哪里单调？**因为压入新元素之前，都会检查尾部的是不是比新元素小，小的话就删了，直到撞到比新元素大，再把新元素压入，这样可以保证永远是一个单调的栈

**注意单调栈如果相等元素也要入栈**

**优点：**单调栈可以时刻从头部front弹出当前（访问过数组）的最大元素值

（也就是只保留现阶段最大值的信息）

**实现：**

**deque&lt;T&gt;deq;  
deq.empty\(\) deq.back\(\) deq.pop\_back\(\) deq.push\_back\(\) deq.front\(\)** 

```cpp
vector<int>& nums
deque<int> deq; //单调栈

int n = nums.size();
for (int i = 0; i < n; i++){
    while(!deq.empty() && nums[i] > num[deq.back()]){
        deq.pop_back();
    } //维护单调栈
    deq.push_back(i); //弹入新元素
    ...
    nums[deq.front()]作为可以返回的最大值
}
```

题目：**Leetcode239滑动窗口最大值** 以及 **剑指offer30 最小栈**

