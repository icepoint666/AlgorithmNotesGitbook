# 贪心算法

贪心算法可以认为是动态规划算法的一个特例

相比动态规划，使用贪心算法需要满足更多的条件

贪心选择性质呢，简单说就是：**每一步都做出一个局部最优的选择，最终的结果就是全局最优。**注意哦，这是一种特殊性质，其实只有一部分问题拥有这个性质。

（虽然贪心，但也能全局最优）

**类似题目重叠子区间问题：**

#### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 435 | 无重叠区间 | 贪心 | 简单 |
| 452 | 用最少数量的箭引爆气球 | 贪心 | 简单 |

**452. 用最少数量的箭引爆气球**

**经典贪心**

```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        int n = points.size();
        if(n == 0)return 0;
        auto cmp = [&](const auto &a, const auto &b){
            return a[1] < b[1];
        };
        sort(points.begin(), points.end(), cmp);
        int count = 1;
        int bd = points[0][1];
        for(int i = 0; i < n; i++){
            if(points[i][0] > bd){
                bd = points[i][1];
                count++;
            }
        }
        return count;
    }
};
```

