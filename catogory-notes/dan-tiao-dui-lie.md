# 单调队列

一个队列，通过一些技巧实现了单调递增

应用在滑动窗口这类问题中

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 239 | 滑动窗口最大值 | 单调队列 | 困难 |

**239. 滑动窗口最大值**

单调处理的核心：新入队列的元素如果发现front比它小，直接把队列front取出，直到更新到比它大的

**原则：保留最新的尽可能大的值，在单调队列里**

循环时需要从back中取出过期元素（队列需要存index）

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<pair<int,int>> q;
    vector<int>res;
    for(int i = 0; i < nums.size(); i++){
        if(!q.empty() && q.back().second <= i - k)q.pop_back(); //注意这个小于等于
        while(!q.empty() && nums[i] > q.front().first){
            q.pop_front();
        }
        q.push_front(make_pair(nums[i], i));
        if(i < k - 1)continue;
        res.push_back(q.back().first);
    }
    return res;
}
```

