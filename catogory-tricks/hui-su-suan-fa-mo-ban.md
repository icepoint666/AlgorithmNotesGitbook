# 回溯算法模板

**排列：**

```cpp
//给定一个 没有重复 数字的序列，返回其所有可能的全排列。
class Solution {
public:
    vector<vector<int>> res; //全局变量记录结果
    void backtrack(vector<int>&nums, vector<int>&track){
        if(track.size() == nums.size()){
            res.push_back(track);
            return;
        }
        int n = nums.size();
        for(int i = 0; i < n; ++i){
            bool repeat = false;
            for(auto &e: track){
                if(e == nums[i]){
                    repeat = true;
                    break;
                }
            }
            if(repeat)continue;
            track.push_back(nums[i]); 
            backtrack(nums, track);
            track.pop_back();
        }
        return;
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> track; 
        backtrack(nums, track);
        return res;
    }
};
```

