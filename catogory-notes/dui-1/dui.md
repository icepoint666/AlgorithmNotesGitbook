# 题目

#### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 41 | 数据流中的中位数 | 最大堆/最小堆来做 | 思考/困难 |
| 剑指 Offer 49/264 | 丑数 | 注意去重+long 处理 | 简单 |
| 480 | 滑动窗口中位数 | 专题（缜密的考虑各种情况） | 困难 |

**剑指 Offer 41. 数据流中的中位数**

要求添加元素进数据结构的复杂度O\(logN\)，查询中位数的复杂度也是O\(logN\)

**解法**：因为中位数最多跟中间两个数有关，所以以中间为分界线

小的一边维护一个最大堆，大的一边维护一个最小堆

**剑指 Offer 49. 丑数**

只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。1是丑数

偏慢的解法：快的解法可以看动态规划

把丑数\*2,\*3,\*5的数都添加到最小堆里，优先队列来维护，**注意要判断重复的丑数！注意会爆int！**

```cpp
int nthUglyNumber(int n) {
    priority_queue<long, vector<long>, greater<long>>q; //小顶堆是greater
    q.push(1);
    long cur; //overflow long去处理
    while(n--){
        cur = q.top();
        while(!q.empty() && q.top() == cur)q.pop(); //一定要去重，可能上一次push(6 = 3 * 2)这一次push(6 = 2 * 3)
        q.push(cur*2);
        q.push(cur*3);
        q.push(cur*5);
    }
    return (int)cur;
}
```

**480. 滑动窗口中位数**

**见下一页**

