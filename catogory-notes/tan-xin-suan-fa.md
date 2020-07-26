# 贪心算法

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 56 | 合并区间 | 重载排序函数 | 简单 |
|  |  |  |  |

**56. 合并区间**

是注意在leetcode类内重载的比较函数，需要时static类型

```cpp
class Solution {
public:
    static bool cmp(vector<int> & v1, vector<int> &v2){
        return (v1[0] < v2[0]);
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> res;
        if(intervals.empty())return res;
        sort(intervals.begin(), intervals.end(), cmp);
        int length = intervals.size();
        vector <int> cur = intervals[0];
        for(int i = 1; i < length; ++i){
            if(cur[1] >= intervals[i][0]){
                vector<int>tmp = {cur[0], max(cur[1], intervals[i][1])};
                cur = tmp;
            }else{
                res.push_back(cur);
                cur = intervals[i];
            }
        }
        res.push_back(cur);
        return res;
    }
};
```

