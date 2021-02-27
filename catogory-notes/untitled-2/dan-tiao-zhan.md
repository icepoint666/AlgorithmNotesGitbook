# 单调栈

**单调栈模板**

```cpp
for(int i = nums.size() - 1; i >= 0; i--){ //可以从后往前，或者从前往后
    while(!stk.empty() && nums[i] >= stk.top()){
        stk.pop();
    }
    //处理
    stk.push(nums[i]);
}
```

**题目：**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5E8F;&#x53F7;/&#x96BE;&#x5EA6;</th>
      <th style="text-align:left">&#x540D;&#x5B57;</th>
      <th style="text-align:left">&#x5907;&#x6CE8;</th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#x5251;&#x6307;Offer 33</td>
      <td style="text-align:left">&#x4E8C;&#x53C9;&#x641C;&#x7D22;&#x6811;&#x540E;&#x5E8F;&#x904D;&#x5386;&#x5E8F;&#x5217;</td>
      <td
      style="text-align:left">&#x9006;&#x5E8F;&#x540E;&#x5E8F;&#x904D;&#x5386;&#xFF0C;&#x5355;&#x8C03;&#x6808;</td>
        <td
        style="text-align:left">&#x4E0D;&#x4F1A;&#xFF0C;&#x91CD;&#x70B9;&#x8BB0;&#x5FC6;</td>
    </tr>
    <tr>
      <td style="text-align:left">739</td>
      <td style="text-align:left">&#x6BCF;&#x65E5;&#x6E29;&#x5EA6;</td>
      <td style="text-align:left">&#x5176;&#x5B9E;&#x5C31;&#x662F;&#x627E;&#x4E0B;&#x4E00;&#x4E2A;&#x66F4;&#x5927;&#x7684;&#x5143;&#x7D20;</td>
      <td
      style="text-align:left">&#x88F8;&#x5355;&#x8C03;&#x6808;</td>
    </tr>
    <tr>
      <td style="text-align:left">&#x5251;&#x6307;Offer 59-II</td>
      <td style="text-align:left">&#x961F;&#x5217;&#x7684;&#x6700;&#x5927;&#x503C;</td>
      <td style="text-align:left">&#x5355;&#x8C03;deque&#x6765;&#x7EF4;&#x62A4;&#x6700;&#x5927;&#x503C;</td>
      <td
      style="text-align:left">&#x4E2D;&#x7B49;</td>
    </tr>
    <tr>
      <td style="text-align:left">496</td>
      <td style="text-align:left">&#x4E0B;&#x4E00;&#x4E2A;&#x66F4;&#x5927;&#x7684;&#x5143;&#x7D20;-i</td>
      <td
      style="text-align:left">&#x5355;&#x8C03;&#x6808;&#x6765;&#x5904;&#x7406;: O(NlogN) &#x66B4;&#x529B;:O(N^2)</td>
        <td
        style="text-align:left">&#x9EBB;&#x70E6;</td>
    </tr>
    <tr>
      <td style="text-align:left">503</td>
      <td style="text-align:left">
        <p>&#x4E0B;&#x4E00;&#x4E2A;&#x66F4;&#x5927;&#x7684;&#x5143;&#x7D20;-ii</p>
        <p>(&#x5FAA;&#x73AF;&#x6570;&#x7EC4;&#x7684;&#x5904;&#x7406;&#xFF09;</p>
      </td>
      <td style="text-align:left">&#x5FAA;&#x73AF;&#x6570;&#x7EC4;&#x5904;&#x7406;&#xFF1A;&#x518D;copy&#x4E00;&#x4EFD;&#x63A5;&#x5728;&#x540E;&#x9762;</td>
      <td
      style="text-align:left">&#x6280;&#x5DE7;</td>
    </tr>
    <tr>
      <td style="text-align:left">556</td>
      <td style="text-align:left">
        <p>&#x4E0B;&#x4E00;&#x4E2A;&#x66F4;&#x5927;&#x7684;&#x5143;&#x7D20;-iii</p>
        <p>(&#x5176;&#x5B9E;&#x662F;next permutation)</p>
      </td>
      <td style="text-align:left">&#x6CE8;&#x610F;int&#x7684;out-of-range&#x5904;&#x7406;</td>
      <td style="text-align:left">&#x4E2D;&#x7B49;</td>
    </tr>
    <tr>
      <td style="text-align:left">155</td>
      <td style="text-align:left">&#x6700;&#x5C0F;&#x6808;</td>
      <td style="text-align:left">&#x53CC;&#x6808;&#xFF1A;&#x8F85;&#x52A9;&#x6808; + &#x9012;&#x589E;id&#x6807;&#x8BB0;</td>
      <td
      style="text-align:left">&#x81EA;&#x5DF1;idea</td>
    </tr>
    <tr>
      <td style="text-align:left">84, 85</td>
      <td style="text-align:left">&#x67F1;&#x72B6;&#x56FE;&#x4E2D;&#x6700;&#x5927;&#x7684;&#x77E9;&#x5F62;</td>
      <td
      style="text-align:left">&#x7EF4;&#x62A4;&#x5355;&#x8C03;&#x6808;</td>
        <td style="text-align:left">&#x56F0;&#x96BE;&#xFF08;&#x8BB0;&#x5FC6;&#xFF09;</td>
    </tr>
  </tbody>
</table>

**剑指 Offer 33. 二叉搜索树的后序遍历序列**

题意：给你一个整数序列，让你判断是不是二叉搜索树的后序遍历序列

转换问题：如果把后序遍历的序列倒序的话，那么就是\[中，右，左\]，这个顺序其实很类似于判断一个序列是不是二叉树的正序遍历序列

基本相同的问题：**判断一个序列是不是二叉搜索树的正序遍历序列**

**怎么使用栈来实现判断？**

倒序遍历序列，关键点在于发现如果**当前节点posterior\[i\]**比它root的值大时，表明矛盾

这个root的维护是关键，你怎么知道这个节点是root呢

我们把每个元素都加入到栈中，一旦**当前元素posterior\[i\]**小于栈中top的元素，那么就开始找root

**root就是栈中比posterior\[i\]大的最后一个元素**，（这个是一个关键特性，用循环实现）然后更新root

再把posterior\[i\]加入到栈中

**关键：这个循环，意思也就是说，如果遇到比当前top小的，就默认为已经是一个左子树了，因为是后续遍历的倒叙，这时候右子树的元素必然已经询问过了，之后如果出现比root大的就是矛盾所在**

```cpp
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        stack<int> stk;
        int root = 0x3f3f3f3f;
        int n = postorder.size();
        if(n == 0)return true;
        for(int i = n - 1; i >= 0; i--){
            if(postorder[i] > root)return false;
            while(!stk.empty() && postorder[i] < stk.top()){ //这个循环是太关键了！理解这个循环的意思
                root = stk.top(); //这个循环就是说，如果遇到比当前top小的，就默认为已经是一个左子树了，然后直接找到该左子树的对应的root
                stk.pop();
            }
            stk.push(postorder[i]);
        }
        return true;
    }
};
```

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

**496. 下一个更大元素 I**

**单调栈：见代码**

**503. 下一个更大元素 II**

**重点：单调栈模板 + 循环数组的处理技巧（\[1,2,3,-1\] =&gt; \[1,2,3,-1,1,2,3,-1\]）**

double一下然后单调栈维护处理所有，计算结果只返回前半部

**556. 下一个更大元素 III**

**题解：本质题意就是next permutation，可以利用单调栈来解决next permutation问题**

**示例：12443111**

①对于降序的443111已经是这六位里面最大排列了，所以接下来一定是要改2了

②2肯定会跟比它大的换，因为本身就是降序，所以从后往前，找到降序中比它大的第一个就是要交换的两个数，也就是3

所以是12443111

注意：如果是1999999999会爆int，所以先转成long，然后判定是否大于INT\_MAX，然后再转成int

```cpp
int nextGreaterElement(int n) {
    string number = to_string(n);
    deque<pair<char,int>>nums;
    int find_idx = -1;
    for(int i = number.size() - 1; i >= 0; i--){
        if(!nums.empty() && number[i] >= nums.front().first || nums.empty())nums.push_front(make_pair(number[i], i));
        else {
            while(number[i] >= nums.back().first){
                nums.pop_back();
            }
            number[nums.back().second] = number[i];
            number[i] = nums.back().first;
            find_idx = i;
            break;
        }
    }
    if(find_idx < 0)return -1;
    auto it = number.begin();
    while(find_idx--){
       it++;
    }
    it++;
    sort(it, number.end());
    long long temp = stol(number);
    if(temp > INT_MAX)return -1;
    return (int)temp;
}
```

**84. 柱状图中最大的矩形**

**85.最大矩形**

给定 _n_ 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

**题解**

**首先：暴力解法O\(N^2\)**

**是循环每一个，计算以当前高度height\[i\]所在柱子为矩形高，所能构成的最大的面积**

**之后优化O\(N\)**

**stage1**

* 在开头安放个高度为0，坐标为-1的哨兵，进入单调栈
* 将比top\(\)高（或者一样高）的加入栈中，如果出现低的就将top\(\)移除，直到移除到单调
* 移除栈的时候，意味着**当前高度height\[i\]所在柱子为矩形高的最大面积计算出来了**

第一遍循环后，目前还有一个单调栈，接下来需要将单调栈清除

**stage2**

* 参考代码的策略

```cpp
int largestRectangleArea(vector<int>& heights) {
    int res = 0;
    stack <pair<int,int>> h;
    h.push({-1,0});
    //stage 1
    for(int i = 0; i < heights.size(); i++){
        if(heights[i] >= h.top().second)
            h.push({i,heights[i]});
        else{
            while(h.top().second > heights[i]){
                int idx = h.top().first;
                h.pop();
                int last_idx = h.top().first;
                res = max(heights[idx] * (i - last_idx - 1), res);
            }
            h.push({i, heights[i]});
        }
    }
    //stage 2
    if(h.size() <= 1)return res;
    int last_heights = h.top().second;
    h.pop();
    while(!h.empty()){
        int idx = h.top().first;
        res = max((int)(heights.size() - idx - 1) * last_heights, res);
        last_heights = h.top().second;
        h.pop();
    }
    return res;
}
```

