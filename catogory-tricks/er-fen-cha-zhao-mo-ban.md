# 二分查找模板

* **二分查找最关键的在于边界的逻辑很容易搞乱，所以如果能够熟悉+熟记模板会好一些！！**

### 普通二分搜索

前提需要保证数组是升序排列\[-1,2,3,5\] **这个版本模板搜索区间是左闭右闭**

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int length = nums.size(); //如果是int nums[]: nums.length
        int left = 0, right = length - 1;
        while(left <= right){
            int mid = (left + right) / 2; //这个mid是偏左的mid，一般使用这个mid会好一些
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] < target){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        return -1;
    }
};
```

**升序排序**

```cpp
sort(nums.begin(), nums.end());
```

**漏洞就是：无法处理指定边界的二分查找**

 给你有序数组 `nums = [1,2,2,2,3]`，`target` 为 2，此算法返回的索引是 2，没错。但是如果我想得到 `target` 的左侧边界，即索引 1，或者我想得到 `target` 的右侧边界，即索引 3，这样的话此算法是无法处理的。

 **也许会说，找到一个 target，然后向左或向右线性搜索不行吗？可以，但是不好，因为这样难以保证二分查找对数级的复杂度了**。

### 寻找左侧边界的二分搜索

```cpp
int left_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0;
    int right = nums.length; // 注意
    
    while (left < right) { // 注意
        int mid = (left + right) / 2;
        if (nums[mid] == target) {
            right = mid; //注意
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid; // 注意
        }
    }
    //终止条件是left == right，所以其实返回left与返回right都是一样的
    if(left >= nums.length) return -1;
    return nums[left] == target ? left : -1; 
    //本身是返回left，但是如果没有找到的话left可能为0或者nums.length，所以应该返回-1
}
```

**注意1：搜索区间左闭右开：**

因为 `right = nums.length` 而不是 `nums.length - 1`。因此每次循环的「搜索区间」是 `[left, right)` 左闭右开。

`while(left < right)` 终止的条件是 `left == right`，此时搜索区间 `[left, left)` 为空，所以可以正确终止。

一般左右侧边界的二分查找，这种半闭半开的写法比较普遍

左闭右开所以更新方式也变得有所不同： **`left = mid + 1`，`right = mid`**

**注意2：如果没有找到，return lef是不会返回-1的，而是返回`nums.length`**或者**`0`**

简单添加两行代码就能在正确的时候 return -1：

```cpp
// target 比所有数都大，left会越界
if (left == nums.length) return -1;
// target 比所有数都小，或者可能找不到，left = 0
return nums[left] == target ? left : -1;//简单添加两行代码就能在正确的时候 return -1：
```

**注意3：为什么该算法能够搜索左侧边界**？

答：关键在于对于 `nums[mid] == target` 这种情况的处理：

```cpp
if (nums[mid] == target)
    right = mid;
```

可见，找到 target 时不要立即返回，而是缩小「搜索区间」的上界 `right`，在区间 `[left, mid)` 中继续搜索，即不断向左收缩，达到锁定左侧边界的目的。

### 寻找**右**侧边界的二分搜索

依旧使用左闭又开来写右侧边界，循环终止条件依旧是 `left == right`

```cpp
int right_bound(int nums[], int target){
    if (nums.length == 0) return -1;
    int left = 0, right = nums.length;
    
    while (left < right) {
        int mid = (left + right) / 2;
        if (nums[mid] == target) {
            left = mid + 1; // 注意
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid;
        }
    }
    //如果一定可以找到就返回left-1就可以了
    if (left == 0) return -1;
    return nums[left-1] == target ? (left-1) : -1;
}
```

