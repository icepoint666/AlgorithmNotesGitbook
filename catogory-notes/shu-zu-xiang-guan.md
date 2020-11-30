# 数组相关

题目：

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 面试题17.10 | 主要元素 | 摩尔投票法：时间O\(N\)空间O\(1\) | 简单 |
| 1535 | 找出数组游戏的赢家 | 看清题： 当一个整数赢得 k 个回合时 | 简单 |
| 42 | 接雨水 | 暴力法 -&gt; 备忘录法 -&gt;双指针法 | 三种方法 |
| 442 | 数组中的重复元素 | \(1\) 原地hash标记数组 \(2\)归位排序 | 有难度 |
| 26/80 | 去除有序数组重复元素 | 数组删除原则，swap到一边，最后全删 | 技巧 |
|  |  |  |  |

一个结论：**寻找数组中两个元素的最大差值**，要求最小值要在最大值右边是可以**O\(n\)**的实现

```cpp
int min_ = 0x3f3f3f3f; //INT_MIN
int max_ = -0x3f3f3f3f; //INT_MAX
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

**42. 接雨水**

* 暴力法：O\(N^2\)复杂度
* 备忘录法：O\(N\)时间，O\(N\)额外空间
* 双指针法：O\(N\)时间，无额外空间 
  * 参考：[https://github.com/labuladong/fucking-algorithm/blob/master/%E9%AB%98%E9%A2%91%E9%9D%A2%E8%AF%95%E7%B3%BB%E5%88%97/%E6%8E%A5%E9%9B%A8%E6%B0%B4.md](https://github.com/labuladong/fucking-algorithm/blob/master/%E9%AB%98%E9%A2%91%E9%9D%A2%E8%AF%95%E7%B3%BB%E5%88%97/%E6%8E%A5%E9%9B%A8%E6%B0%B4.md)

**442.数组中的重复元素**

**解法1：原地hash标记数组:**

```cpp
vector<int> findDuplicates(vector<int>& nums) {
    int n = nums.size();
    for(int i = 0; i < n; i++){
        nums[(nums[i]-1)%n] += n; // 要整除n，这个很关键
    }
    vector<int>res;
    for(int i = 0; i < n; i++){
        if(nums[i] > 2 * n)
            res.push_back(i+1);
    }
    return res;
}
```

**解法2：归位排序**

将nums\[i\]置换**swap**到其对应的位置nums\[nums\[i\]-1\]上去

比如对于没有缺失项的正确的顺序应该是\[1, 2, 3, 4, 5, 6, 7, 8\]

最后**在对应位置检验，如果nums\[i\]和i+1不等，那么我们将nums\[i\]存入结果res中即可**

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

**80. 删除排序数组中的重复项 II**

给定一个排序数组，你需要在[**原地**](http://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)删除重复出现的元素，使得每个元素最多出现两次，返回移除后数组的新长度。

**原则: 对于数组相关的算法问题，有一个通用的技巧：要尽量避免在中间删除元素，那我就先想办法把这个元素换到最后去**。这样的话，最终待删除的元素都拖在数组尾部，一个一个 pop 掉就行了，每次操作的时间复杂度也就降低到 O\(1\) 了。

**技巧：slow指针，fast指针, 用slow fast解决，把不重复的swap到slow左边**

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if(n == 0 || n == 1)return n;
        bool count = true;
        int slow = 0;
        for(int i = 1; i < n; i++){
            if(nums[i] != nums[slow]){
                count = true;
                slow++;
                if(nums[i]!=nums[slow])swap(nums[i], nums[slow]);
            }else if(nums[i] == nums[slow] && count){
                count = false;
                slow++;
                if(nums[i]!=nums[slow])swap(nums[i], nums[slow]);
            }
        }
        return slow+1;
    }
```



