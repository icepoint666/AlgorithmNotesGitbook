# 前缀和

**前缀和：** 这个前缀和数组 `preSum` 的含义也很好理解，`preSum[i]` 就是 `nums[0..i-1]` 的和。那么如果我们想求 `nums[i..j]` 的和，只需要一步操作 `preSum[j+1]-preSum[i]` 即可，而不需要重新去遍历数组了。

典型的空间换时间

```cpp
int n = nums.size();
int preSum[n+1];
preSum[0] = 0;
for(int i = 0; i < n; ++i){
    preSum[i+1] = preSum[i] + nums[i];
}
//preSum[j+1] - preSum[i] 表示[i,j]的和
```

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 560 | 和为K的子数组 | 前缀和 | 简单 |
| 面试题16.17 | 连续序列 | 类似于买卖股票最大利润 | 几个小点要注意 |
| 牛客网6219/B | 疯狂过山车 | 前缀和记录的思想 | 中等（没想到这种方法） |

**560.和为K的子数组**

先计算前缀和

暴力的话可以用**二次循环遍历子数组的头和尾**，然后找结果**是否等于k**：复杂度**O\(N^2\)**

但是任意这种**遍历找固定值的问题**都可以通过**哈希表**来优化：复杂度**O\(N\*logN\)**

注意：

①边界处理：子数组不能为空，所以找到的preSum\[j\]需要 j &gt; i

②map的value需要是vector存放值为key的index，因为前缀和可能存在重复

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        if(nums.empty())return 0;
        int n = nums.size();
        int preSum[n+1];
        preSum[0] = 0;
        unordered_map<int,vector<int>> mp; //存放前缀和值为int的index数组
        for(int i = 0; i < n; ++i){ //不要把preSum[0]加入到哈希表中，加了也没用
            preSum[i+1] = preSum[i] + nums[i];
            if(mp.count(preSum[i+1])==0){
                mp[preSum[i+1]] = vector<int>{i+1};
            }else{
                mp[preSum[i+1]].push_back(i+1);
            }
        }
        int res = 0;
        for(int i = 0; i < n; i++){
            int target = k + preSum[i];
            if(mp.count(target)>0){
                for(auto &e: mp[target]){
                    if(e > i)res++; //一定要大于，不能大于等于
                }
            }
        }
        return res;
    }
};
```

**面试题 16.17. 连续数列**

给定一个整数数组，找出总和最大的连续数列，并返回总和。

示例：

输入： \[-2,1,-3,4,-1,2,1,-5,4\] 输出： 6 解释： 连续子数组 \[4,-1,2,1\] 的和最大，为 6。

```cpp
int maxSubArray(vector<int>& nums) {
    if(nums.empty())return 0;
    int sums[nums.size() + 1];
    sums[0] = 0;
    int min_ = 0, max_ = INT_MIN; //注意1：以后一定要用INT_MIN或者INT_MAX，不要用0x3f3f3f3f，这个不够大
    for(int i = 0; i < nums.size(); i++){
        sums[i+1] = sums[i] + nums[i];
        max_ = max(sums[i+1] - min_, max_);
        min_ = min(sums[i+1], min_);  //注意2：min_在后面可以防止 连续序列size为0的情况发生
    }
    return max_;
}
```

**牛客网竞赛6219/B：疯狂过山车**

题目链接：[https://ac.nowcoder.com/acm/contest/6219/B](https://ac.nowcoder.com/acm/contest/6219/B)

给定一个数组，找到最长的上升+下降的“金字塔”段，输出这个长度的最大值（必须既有包含上升，也有包含下降）

**解法**：遍历每个金字塔的中间节点，找一下从左下降的长度是多少，从右下降的长度是多少

length = l + r + 1

暴力的话，显然不可取O\(n^2\)

所以利用前缀和的思想，事先循环记录每个中间节点到左右两端额度下降长度，可以优化到O\(N\)

先从左向右循环，记录从左到右的连续递增长度，再从右到左循环，记录从右到左的连续递增长度

代码：

```cpp
class Solution {
public:
    int getMaxLength(int n, vector<int>& num) {
        if(n == 0)return 0;
        vector<int> l, r;
        l.resize(n);
        r.resize(n);
        l[0] = 0;
        r[n-1] = 0;
        for (int i = 1; i < n; ++i){
            if(num[i] > num[i-1]){
                l[i] = l[i-1] + 1;
            }else{
                l[i] = 0;
            }
        }
        for (int i = n - 2; i >= 0; i--){
            if(num[i+1] < num[i]){
                r[i] = r[i+1] + 1;
            }else{
                r[i] = 0;
            }
        }
        int res = 0;
        for (int i = 0; i < n; i++){
            res = max(res, l[i] + r[i] + 1);
        }
        return res;
    }
};
```



