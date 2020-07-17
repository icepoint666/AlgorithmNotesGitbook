# 左右双指针

**题目特征**

* 左右指针在数组中是两个索引值，主要解决二分查找的问题

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 704 | 二分查找 | 模板 | 简单 |
| 1 | 两数之和 | 哈希表！绝妙的方法！ | 中等 |
|  |  | 排序+左右指针+遍历 | 笨办法 |

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

