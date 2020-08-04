# 动态规划

本质：一种备忘录

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 14 | 剪绳子/分数字 | 动态规划来解决 | 有坑 |
| 300 | 最长递增子序列 | 动态规划O\(N^2\) / 二分查找O\(NlogN\) | O\(NlogN）解法记一下 |

**剑指 Offer 14- I. 剪绳子**

**分数字：**

不单纯是max的动态规划更新，注意一定要max\(dp\[j\],j\)，不加这个max，n = 4就会出问题

```cpp
class Solution {
public:
    int dp[59];
    int cuttingRope(int n) {
        memset(dp, 0, sizeof(dp));
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n; i++){
            for(int j = 1; j < n; j++){ // 保证不能j不是0，也不是n，所以确保m > 1
                dp[i] = max(dp[i], max(dp[j],j)*(i-j)); //第二个max一定要注意加
            }
        }
        return dp[n];
    }
};
```

**300. 最长上升子序列**

**解法1：动态规划（存在一个容易粗心的地方）**

```cpp
for(int i = 1; i <=n; ++i){
    dp[i] = 1;
    for(int j = 1; j < i; j++){
        dp[i] = max(dp[i], nums[i-1]>nums[j-1]?dp[j]+1:1);
    }
}
```

容易粗心的地方：注意返回所有dp\[i\]的最大值**（看清要返回什么）**

**解法2：**patience sorting（耐心排序）O\(NlogN\)

解法讲解：类似蜘蛛纸牌那种问题

[**https://github.com/labuladong/fucking-algorithm/blob/master/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E7%B3%BB%E5%88%97/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E8%AE%BE%E8%AE%A1%EF%BC%9A%E6%9C%80%E9%95%BF%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.md\#%E4%BA%8C%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE%E8%A7%A3%E6%B3%95**](https://github.com/labuladong/fucking-algorithm/blob/master/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E7%B3%BB%E5%88%97/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E8%AE%BE%E8%AE%A1%EF%BC%9A%E6%9C%80%E9%95%BF%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.md#%E4%BA%8C%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE%E8%A7%A3%E6%B3%95)\*\*\*\*

```cpp
int lengthOfLIS(vector<int>& nums) {
    vector<int> temp;
    int n = nums.size();
    for(int i = 0; i < n; ++i){
        bool flag = false;
        for(int j = 0; j < temp.size(); ++j){
            if(nums[i] <= temp[j]){ // 到底加不加等号，想一下极端情况全是5的话会怎么样
                temp[j] = nums[i];
                flag = true;
                break;
           }
       }
       if(!flag)temp.push_back(nums[i]);
    }
    return temp.size();
}
```

