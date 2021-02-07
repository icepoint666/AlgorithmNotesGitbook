# 左右双指针

**题目特征**

* 左右指针在数组中是两个索引值，主要解决二分查找的问题
* 有时：HashMap 或者 HashSet 也可以帮助我们处理无序数组相关的简单问题

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 704 | 二分查找 | 模板 | 简单 |
| 1 | 两数之和 | 哈希表！（返回的是索引，所以可以哈希表） | 中等 |
|  |  | 排序+左右指针+遍历 | 笨办法 |
| 剑指 Offer57 | 两数之和（裸） | 左右指针，从左右往中间扫 | 模板 |
| 42 | 接雨水 | 通过双指针可以实现时间空间最优 | 困难 |
| 15 | 三数之和 | 排序+双指针（返回的三数不重复，所以用不了哈希表） | 中等 |
| 16 | 最接近的三数之和 | 排序+双指针，不用返回每个数字，只用返回和，更简单 | 简单 |
| lint 58 | 四数之和 | 前两个数枚举+排序+双指针 | 中等 |
| 1726 | 同积元组 | 类似于四数之和，不过因为元素都不等 哈希表可以降低复杂度 | 想到 |
| 剑指 Offer 21 | 奇数位于偶数前 | 关键要想到不占空间的解法：双指针 |  |
| 75 | 颜色分类 | 对只存在0,1,2值的数组排序，双指针 | 理清关系 |
| 11 | 盛最多水的容器 | 关键思考这道题为什么可以，双指针 | 想到 |

#### 题目笔记

**1. 两数之和**

**方法1：排序+左右指针+遍历（不推荐）**

易错1：本身不是有序的，需要新建一个数组来保存排序的数组，然后找到两个值

易错2：这两个值可能是相等的，通过循环去找值对应的索引的时候，注意考虑两值相等这种特殊情况

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> sorted = nums;
        sort(sorted.begin(), sorted.end()); //排序
        vector<int> res;
        //左右指针查找两个值
        int left = 0, right = nums.size() - 1;
        int val1,val2;
        bool f1 = true, f2=true;
        while(left <= right){
            if(sorted[left] + sorted[right] == target){
                val1 = sorted[left];
                val2 = sorted[right];
                break;
            }else if(sorted[left] + sorted[right] < target){
                left++;
            }else{
                right--;
            }
        }
        //找两个值对应的索引
        int i = 0;
        while(f1||f2){
            if(val1 == nums[i] && f1){
                res.push_back(i);
                f1=false;
            }
            else if(val2 == nums[i] && f2){
                res.push_back(i);
                f2 = false;
            }
            i++;
        }
        return res;
    }
};
```

**方法2：哈希表（推荐）：一边检查一边添加，是绝妙的解法**

复杂度**O\(nlogn\)**: 遍历nums: 在哈希表中查找是否存在`val =  target - nums[i]`

**关键：**

如果第一次循环先建立哈希表，第二次循环再检查，是不对的

这样没法解决**输入\[3,3\] target是6**的这种情况，也就是解有**两个重复元素**

**一边检查一边添加，不仅可以减少hash表一半的复杂度，同时也可以解决两个重复元素的问题**

**把握好防止重复的关键:**

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> mp;
        int length = nums.size();
        vector<int>res;
        for (int i = 0; i < length; ++i){
            int other = target - nums[i];
            if(mp.count(other)>0){
                res = {mp[other], i};
                return res;
            }
            mp[nums[i]] = i;
        }
        return res;
    }
};
```

**42. 接雨水**

**题目：**就是柱状的形状，能存多少积水思路

**做题思路：刚开始可能想不到好办法，先暴力，然后对暴力进行时间上，空间上的优化，最后达到最优解**

\*\*\*\*[**https://www.zhihu.com/question/339135205/answer/913555025**](https://www.zhihu.com/question/339135205/answer/913555025)\*\*\*\*

**暴力：**

每一个柱子height\[i\]上可能积多少水，由左边最大值和右边最大值的最小值决定

暴力寻找最小值O\(N^2\)复杂度

**备忘录优化：**

先循环一遍，来记录每个位置左边最大值和右边最大值，这样时间复杂度O\(N\)但是空间复杂度也有O\(N\)

（注意特判）

```cpp
int trap(vector<int>& height) {
    int n = height.size();
    if(n == 0)return 0; // 特判
    int l_maxs[n];
    int l_max = 0, r_max = 0;
    int r_maxs[n];
    for(int i = 0; i < n; ++i){
        l_max = max(l_max, height[i]);
        l_maxs[i] = l_max;
        r_max = max(r_max, height[n - i - 1]);
        r_maxs[n - i - 1] = r_max;
    }//储存l_max, r_max
    int ans = 0;
    for(int i = 0; i < n; ++i){
        if (i == 0 || i == n - 1)continue; //特判
        ans += max(min(l_maxs[i-1], r_maxs[i+1]) - height[i], 0);
    }
    return ans;
}
```

**双指针：**

 双指针**边走边算O\(N\)**，节省下空间复杂度。

**核心思想**：因为只需要`min(l_max, r_max)`，

左值针left,右指针right，虽然l\_max记录的是 \[0,left\] 的最大值，r\_max记录的是\[right,n-1\]，中间\(left,right\)不知道最值

但是其实如果想计算height\[left+1\]处的积水，只要保证l\_max要小于r\_max（也就是\[right, n-1\]的最大值），就可以保证：`l_max = min(l_max, r_max)`如果无法保证，那么另一端height\[right-1\]就可以保证，然后更新它即可

```cpp
int trap(vector<int>& height) {
    if(height.empty())return 0;
    int n = height.size();
    int left = 0, right = n - 1;
    int l_max = 0, r_max = 0;
    int ans = 0;
    while(left <= right){
        l_max = max(l_max, height[left]);
        r_max = max(r_max, height[right]);
        if(l_max <= r_max){
            ans += max(l_max - height[left], 0);
            left++;
        }else{
            ans += max(r_max - height[right], 0);
            right--;
        }
    }
    return ans;
}
```

**15. 三数之和**

**双指针复杂度：O\(N^2\)** 

关键是不能重复，需要保证与上次枚举的不同，可以**用set**

（set可以存vector数组，但是unordered\_set不行，需要处理才可以）

* **并且为了防止极端的样例，例如全都是0（会超时）需要在加两个while循环，保证和上次枚举的数不同就行了**
* 通过循环确定第一个数first
* 然后左右双指针，这部分等价于**二数之和**的解法,但是不太一样

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        set<vector<int>> st; //set去重
        int n = nums.size();
        for(int i = 0; i < n - 2; i++){//第一个指针
            int left = i + 1;//第二个指针
            int right = n - 1;//第三个指针
            while(left < right){
                if(nums[i] + nums[left] + nums[right] == 0){
                    st.insert(vector<int>{nums[i], nums[left], nums[right]});
                    while(left + 1 < right && nums[left] == nums[left+1])left++;
                    left++;
                    while(right - 1 > left && nums[right] == nums[right-1])right--;
                    right--;
                    //因为不允许重复，左指针,右指针考虑去重的同时移动
                }else if(nums[i] + nums[left] + nums[right] > 0)right--;
                else left++;
            }
        }
        vector<vector<int>>res;
        for(auto it = st.begin(); it!=st.end(); ++it){
            res.push_back(*it);
        }
        return res;
    }
```

**16. 最接近的三数之和**

```cpp
int threeSumClosest(vector<int>& nums, int target) {
    int res = 100000;
    sort(nums.begin(), nums.end());
    for(int i = 0; i < nums.size() - 2; i++){
        int left = i + 1, right = nums.size() - 1;
        while(left < right){
            int sum = nums[i] + nums[left] + nums[right];
            if(abs(sum - target) < abs(res - target))res = sum;
            if(sum < target)left++;
            else if(sum > target)right--;
            else return target;
        }
    }
    return res;
}
```

**lint 58 四数之和**

**推荐解法：哈希表（O\(n^2logn\)\)**

```cpp

```

**普通解法：**将其转换为三数之和，在转换为二数之和 **（时间复杂度O\(n^3\) ）**

**不同于三数之和的核心代码：**

```cpp
for (int i = 0; i < size - 3; i++) {//第一个指针 
    for (int j = i + 1;j < size - 2;j++) {//第二个指针 
        if (j > i + 1 && numbers[j] == numbers[j-1]) { 
            continue; 
        }
        ...
```

**剑指 Offer 21. 调整数组顺序使奇数位于偶数前面**

不建额外的空间的解法：**双指针**

**左边指针扫，扫到偶数停止，右边指针扫到奇数停，然后让两者交换位置，再继续移动即可**

**11. 盛最多水的容器**

解题：关键要知道为什么这道题可以左右双指针，而不需要暴力O\(N^2）

**左右指针对应较小的数值的那个往内移动 是 合理的**

\*\*\*\*

