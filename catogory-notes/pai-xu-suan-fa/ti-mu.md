# 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 581 | 需要排序的最短子数组长度 | 理清思路 | 中等 |

**581. 最短无序连续子数组**

给定一个整数数组，你需要寻找一个**连续的子数组**，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是**最短**的，请输出它的长度。

**示例：**

```text
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```

**题解：**

思路：遍历过的最大值大于当前值，那么当前值肯定是无效的，那么排序时这个最大值在当前位置或者是更右的位置

没很快做出来的原因是，我考虑的是从左到右遍历，去更新最左范围

```cpp
int findUnsortedSubarray(vector<int>& nums) {
    int len = nums.size();
    if(len <= 1)return 0;
    int maxleft = nums[0];
    int maxright = nums[len - 1];
    int left = 0, right = -1; // 这两个指针分别记录左右两边无效的位置
    /*数组原本有序的时候 right - left + 1 = 0*/
    /*1.从左到右遍历：找出不合适数的最右范围*/
    for(int i = 1; i < len; i++){
        if(nums[i] < maxleft){
            right = i;
        }else{
            maxleft = max(nums[i], maxleft);
        }
    }
    /*2.从右到左遍历：找出不合适数的最左范围*/
    for(int i = len - 2; i >= 0; i--){
        if(nums[i] > maxright){
            left = i;
        }else{
            maxright = min(nums[i], maxright);
        }
    }
    return right - left + 1;
}
```

