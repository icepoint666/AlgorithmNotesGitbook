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
        style="text-align:left">&#x4E0D;&#x4F1A;&#xFF0C;&#x503C;&#x5F97;&#x8BB0;&#x5FC6;</td>
    </tr>
    <tr>
      <td style="text-align:left">&#x5251;&#x6307;Offer 59-II</td>
      <td style="text-align:left">&#x961F;&#x5217;&#x7684;&#x6700;&#x5927;&#x503C;</td>
      <td style="text-align:left">&#x5355;&#x8C03;&#x6808;&#x6765;&#x7EF4;&#x62A4;&#x6700;&#x5927;&#x503C;</td>
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
                root = stk.top();
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

**关键：如何维护一个deque使它能返回队列的最大值**

**讨论两种情况：**

* **如果新加入的元素，大于deque的顶端值，那么pop这个顶端值，直到pop到比它大的值**
* **如果新加入的元素，小于deque的顶端值，直接加入**

**需要保证deque能记录，元素的index（插入顺序）**

**如果队列移除一个元素，这时队列开始点的index大于deque最底端元素的index，那么也要清楚这个deque的最底端元素**

```cpp
class MaxQueue {
public:
    queue<int>q;
    deque<pair<int,int>>dq;
    int start,end;
    MaxQueue() {
        start = 0;
        end = 0;
    }
    
    int max_value() {
        if(dq.empty())return -1;
        return dq.front().first;
    }
    
    void push_back(int value) {
        q.push(value);
        if(dq.empty()){
            dq.push_back(make_pair(value,end++));
        }else{
            while(!dq.empty()&&value >= dq.back().first){
                dq.pop_back();
            }
            dq.push_back(make_pair(value,end++));
        }
    }
    
    int pop_front() {
        if(q.empty())return -1;
        start++;
        if(start > dq.front().second)dq.pop_front();
        int tmp = q.front();
        q.pop();
        return tmp;
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



