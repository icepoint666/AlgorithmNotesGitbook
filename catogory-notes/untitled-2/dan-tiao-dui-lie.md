# 单调队列

一个队列，通过一些技巧实现了单调递增

应用在滑动窗口这类问题中**（注意更规范化的实现代码）**

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 239 | 滑动窗口最大值 | 单调deque（更规范的实现代码，推荐！） | 模板 |
|  |  | 动态规划实现 | 困难 |
| 剑指Offer 59-II | 队列的最大值 | 单调deque来维护最大值 | 中等 |

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

**剑指 Offer 59 - II. 队列的最大值**

 函数`max_value`、`push_back` 和 `pop_front` 的**均摊**时间复杂度都是O\(1\)

**关键：使用deque**

**讨论两种情况：**

* 如果新加入的元素，大于deque的顶端值，那么pop这个顶端值，直到pop到比它大的值
* 如果新加入的元素，小于deque的顶端值，直接加入

**关键：需要保证deque能记录，元素的index吗（插入顺序）**

**\(不需要记录index）这里有一个trick：加入的时候如果相等的元素也加入单调deque，删除的时候通过相等来判断是否删除即可**

```cpp
class MaxQueue {
public:
    deque<int>dq;
    queue<int>q;
    MaxQueue() {
        q = queue<int>();
        dq = deque<int>();
    }
    
    int max_value() {
        if(q.empty())return -1;
        return dq.front();
    }
    
    void push_back(int value) {
        while(!dq.empty() && dq.back() < value){
            dq.pop_back();
        }//注意相等的也要push
        dq.push_back(value);
        q.push(value);
    }
    
    int pop_front() {
        if(q.empty())return -1;
        int val = q.front();
        if(val == dq.front())dq.pop_front();
        q.pop();
        return val;
    }
};
```

