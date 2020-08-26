# 其他题

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 61 | 扑克牌中的顺子 |  | 中等 |

**剑指 Offer 61. 扑克牌中的顺子**

判断是不是一个顺子**，**0可以代表任何牌（**把握好两个关键判定**）

```cpp
bool isStraight(vector<int>& nums) {
    int joker = 0;
    sort(nums.begin(), nums.end());
    for(int i = 0; i < 5; i++){
        if(nums[i] == 0)joker++;
        else if(i > 0 && nums[i-1] == nums[i])return false; // 关键1：相同的提前报错
    }
    return nums[4] - nums[joker] < 5; // 关键2：最大值最小值之差
}
```

