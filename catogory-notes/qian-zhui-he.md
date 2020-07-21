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

