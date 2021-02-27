# 单调队列

一个队列，通过一些技巧实现了单调递增

应用在滑动窗口这类问题中**（注意更规范化的实现代码）**

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 239 | 滑动窗口最大值 | 单调deque（更规范的实现代码，推荐！） | 模板 |
|  |  | 动态规划实现 | 困难 |

**239. 滑动窗口最大值**

单调处理的核心：新入队列的元素如果发现front比它小，直接把队列front取出，直到更新到比它大的

**原则：保留最新的尽可能大的值，在单调队列里**

循环时需要从back中取出过期元素（队列需要存index）

**自己实现的代码（不推荐）**

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

**更加规范化的实现代码（推荐,规范）**

```cpp
class MonotonicQueue {
private:
    deque<int> data;
public:
    void push(int n) {
        while (!data.empty() && data.back() < n) //如果相等也要加入，这是关键！
            data.pop_back();
        data.push_back(n);
    }
    
    int max() { return data.front(); }
    
    void pop(int n) {
        if (!data.empty() && data.front() == n)
            data.pop_front();
    }
};

vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    MonotonicQueue window;
    vector<int> res;
    for (int i = 0; i < nums.size(); i++) {
        if (i < k - 1) { //先填满窗口的前 k - 1
            window.push(nums[i]);
        } else { // 窗口向前滑动
            window.push(nums[i]);
            res.push_back(window.max());
            window.pop(nums[i - k + 1]);
        }
    }
    return res;
}
```

**这道题也可以动态规划来做**

详见：[**https://leetcode-cn.com/problems/sliding-window-maximum/solution/hua-dong-chuang-kou-zui-da-zhi-by-leetcode-3/**](https://leetcode-cn.com/problems/sliding-window-maximum/solution/hua-dong-chuang-kou-zui-da-zhi-by-leetcode-3/)\*\*\*\*

时间复杂度：O\(N\)，我们对长度为 N 的数组处理了 3次。

空间复杂度：O\(N\)，用于存储长度为 N 的 left 和 right 数组，以及长度为 N - k + 1的输出数组。

