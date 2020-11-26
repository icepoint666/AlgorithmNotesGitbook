# 二分查找

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指offer11 | 旋转数组的最小数字 | 普通做法O\(n\) | 简单 |
|  |  | O\(logn\): 另一个二分查找模板+去重 | 易晕 |
| 剑指offer53 | 在排序数组中查找元素个数 | 二分法左侧边界模板+从左侧统计个数 | 易错 |
| 剑指offer53 | 0~n-1缺失的数字 | 另一个二分查找模板的练习 | 简单 |
| 1552 | 两球之间的磁力 | 对于求最大值的最小值问题（二分法） | 记忆 |
| 自定义 | 高效判定字符串子序列 | 利用二分查找优化（见字符串专题） | 记忆 |

**二分查找一类经典应用：**

往往是需要找到一个能完成什么工作的最小值，或者最大值

一般就是线性问题，二分查找一个中间值，计算出结果就能排除掉一般的情况，然后继续二分查找

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 875 | 爱吃香蕉的珂珂 | 左边界二分查找模板，因为是找最小速度，但target值多个O\(n\*logn\) | 模板题 |
| 1011 | 在 D 天内送达包裹的能力 | 与上题一样做法，有一个坑：MIN\_BOUND开始不能取最小值 |  |

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

