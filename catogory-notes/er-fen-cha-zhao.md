# 二分查找（容易晕）

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指offer11 | 旋转数组的最小数字 | 另一个二分查找模板+去重 | 重点（完全晕） |

**剑指 Offer 11. 旋转数组的最小数字**

因为这里的二分查找有点不同，很容易去找比左边小的那个数，但是这种方法找下去可能很容易错

所以这道题是去**跟right指针的值比较，找最小值，要使用二分查找模板2！！**

* 如果比right指针的值大，说明在左边上升区，说明要找的最小值在mid右边（最差也是right指针的值），所以可以放心更新left = mid + 1
* 如果比right指针的值小，那么说明要找的最小值在mid左边，或者自己就是（因为自己可以代表下一阶段的右边），所以可以更新right = mid
* 如果等于right指针的值，这时候要去重，把right指针更新掉right--

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

