# 题目

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指offer11 | 旋转数组的最小数字 | 普通做法O\(n\) | 简单 |
|  |  | O\(logn\): 另一个二分查找模板+去重 | 易晕 |
| 74 | 搜索二维矩阵 | 推荐做法：展开\[0,m\*n\)二分 | 简单 |
|  |  | 个人练习二分：找右边界模板+找目标模板 | 易晕 |
| 剑指offer53 | 在排序数组中查找元素个数 | 二分法左侧边界模板+从左侧统计个数 | 易错 |
| 剑指offer53 | 0~n-1缺失的数字 | 二分查找左边界 | 简单 |
| 1552 | 两球之间的磁力 | 对于求最大值的最小值问题（二分法） | 记忆 |
| 自定义 | 高效判定字符串子序列 | 利用二分查找优化（见字符串专题） | 记忆 |
| 4 | 寻找两个正序数组的中位数 | 时间复杂度O\(log\(m+n\)\)，需要二分查找 | 困难 |
| 34 | 在排序数组中查找元素的第一个和最后一个位置 | 二分查找左右边界模板题 | 记忆 |
| 162 | 寻找峰值 | 相当于找相邻左值大于右值的左边界，二分O\(logN\) | 记忆 |
| 自定义 | 先增后减数组二分查找 | 先二分查找峰值，然后左右分别二分查找，三次二分 | 记忆 |
| 69 | x的平方根 | 想到用二分，注意转long |  |
| 540 | 有序数组中的单一元素 | 二分偶数节点就好了 | 技巧 |
| 面试题10.05 | 稀疏数组搜索 | 关键：空字符串要移动left，并判断移动left的时候left是不是target | 细节 |

**剑指 Offer 11. 旋转数组的最小数字**

O\(logN\): 因为这里的二分查找有点不同，很容易去找比左边小的那个数，但是这种方法找下去可能很容易错

所以这道题是去**跟right指针的值比较，找最小值，要使用二分查找模板2！！**

* 如果比right指针的值大，说明在左边上升区，说明要找的最小值在mid右边（最差也是right指针的值），所以可以放心更新left = mid + 1
* 如果比right指针的值小，那么说明要找的最小值在mid左边，或者自己就是（因为自己可以代表下一阶段的右边），所以可以更新right = mid
* 如果等于right指针的值，这时候要去重，把right指针更新掉right--（因为不清楚该往哪一边）

```cpp
class Solution {
public:
    int minArray(vector<int>& nums) {
        int n = nums.size();
        int l = 0, r = n - 1;
        int mid;
        while(l < r){
            int mid = l + (r - l) / 2;
            if(nums[mid] < nums[r]){
                r = mid;
            }else if(nums[mid] > nums[r]){
                l = mid + 1;
            }else{
                r--;
            }
        }
        return nums[l];
    }
};
```

**剑指 Offer 53 - I. 在排序数组中查找数字 I**

寻找左侧边界模板+尾部替换成这个即可：一定要注意`l < n`不然会内存溢出

```cpp
int count = 0;
while(l < n && nums[l++]==target)count++; 
return count;
```

**1552. 两球之间的磁力**

思路：

1. 两球的最小距离的最小值，是1；最小距离的最大值是 \(最后位置的球坐标 - 最前位置的球坐标\) / \(球数-1\)，这里需要先对position数组排序，那么易得最小球间距离的最大值为 \(position\[position.length - 1\] - position\[0\]\) / \(m-1\) 
2.  有最小和最大，直觉想到二分法。 以二分的中间值，作为间距去摆放球。如果摆放的球数 &gt;=m, 可认为需要增加球间距 （同时保存中间值作为候选答案）； 否则需要减少球间距。 
3. 一点总结是， 当碰到求最大或最小值的时候，是否可转化为二分法。属于直觉和经验吧。

代码见leetcode

**1011. 在 D 天内送达包裹的能力**

**题解：**标准的二分查找左边界模板，但是有一个**坑**就是必须要**限制W\_MIN**，不然如果搜索的时候载重过小的话，永远没法装上，就会导致无限循环

```cpp
class Solution {
public:
    int Day(vector<int>weights, int W){
        int d = 0, i = 0, count = 0;
        while(i < weights.size()){
            count += weights[i];
            if(count > W){
                d++;
                count = 0;
            }else{
                i++;
            }
        }if(count)d++;
        return d;
    }
    int shipWithinDays(vector<int>& weights, int D) {
        int W_MAX = 500*50000;
        int W_MIN = 1;
        for(int i = 0; i < weights.size(); i++) //限制最小范围
            W_MIN = max(weights[i], W_MIN);
        while(W_MIN < W_MAX){
            int mid = (W_MIN + W_MAX) / 2;
            int w = Day(weights, mid);
            if(w <= D){
                W_MAX = mid;
            }else{
                W_MIN = mid + 1;
            }
        }
        return W_MIN;
    }
};
```

**4. 寻找两个正序数组的中位数**

给定两个大小为 m 和 n 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的中位数。

**要求时间复杂度是 O\(log\(m+n\)\)**

**题解：二分查找**

```cpp
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    int n = nums1.size(), m = nums2.size();
    if(n > m)return findMedianSortedArrays(nums2, nums1); // 1.保证数组1一定最短
    int mid1, mid2, low = 0, high = 2 * n; // midi 为第i个数组的割,比如mid1=2时表示第1个数组只有2个元素
    //2.我们目前是虚拟加了'#'所以数组1是2*n长度
    int l1, l2, r1, r2; //li为第i个数组割后的左元素。ri为第i个数组割后的右元素。
    while (low <= high){
        mid1 = (low + high) / 2;   //割出来mid1的左边+mid2的右边
        mid2 = m + n - mid1;       //割出来mid2的左边+mid1的右边
        l1 = (mid1 == 0) ? INT_MIN : nums1[(mid1 - 1) / 2];
        r1 = (mid1 == 2 * n) ? INT_MAX : nums1[mid1 / 2];
        l2 = (mid2 == 0) ? INT_MIN : nums2[(mid2 - 1) / 2];
        r2 = (mid2 == 2 * m) ? INT_MAX : nums2[mid2 / 2];
        
        if (l1 > r2)high = mid1 - 1;
        else if (l2 > r1)low = mid1 + 1;
        else break;
    }
    //最终的结果是l1 <= r2 && l2 <= r1，而且前提是mid1 + mid2 = m + n
		return (max(l1, l2) + min(r1, r2)) / 2.0; //无论是奇数个还是偶数个都满足题意
}
```

**162. 寻找峰值**

 给你一个输入数组 `nums`，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 **任何一个峰值** 所在位置即可。

**因为限定条件**nums\[-1\] = nums\[n\] = -∞问题通过二分就可以找到

**相当于找相邻左值大于右值的左边界**

```cpp
//按照左边界模板
int findPeakElement(vector<int>& nums) {
    int left = 0, right = nums.size();
    while(left < right){
        int mid = (left + right) / 2;
        if(mid < nums.size() - 1 && nums[mid] <= nums[mid+1])left = mid + 1;
        else right = mid;
    }
    return left == nums.size()? nums.size() - 1:left;
}

===================================================

int findPeakElement(vector<int>& nums) {
    int l = 0, r = nums.size() - 1;

    while (l < r) {
        int mid = (l + r) >> 1;
        if (nums[mid] > nums[mid + 1]) r = mid;
        else l = mid + 1;
    }
    return l;           //此时l等于r，返回任意一个即可
}
```

**540. 有序数组中的单一元素**

```cpp
int singleNonDuplicate(vector<int>& nums) {
    int left = 0, right = nums.size() / 2;
    while(left < right){
        int mid = (left + right) / 2;
        if(nums[mid*2+1]!=nums[mid*2+2])left = mid + 1;
        else right = mid;
    }
    return nums[left*2];
}
```

