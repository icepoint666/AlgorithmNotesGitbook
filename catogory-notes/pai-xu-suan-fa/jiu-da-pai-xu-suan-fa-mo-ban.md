# 九大排序算法模板

### 快速排序

```cpp
int partition(vector<int>&nums, int i, int j){
        int val = nums[i];
        while(i != j){
            while(i < j && val <= nums[j])j--; //即使i提前等于j了，交换两者也没有任何关系
            swap(nums[i], nums[j]);            //std::swap
            while(i < j && val >= nums[i])i++; //val = nums[i]相等这种情况就不需要交换了
            swap(nums[i], nums[j]);
        }
        return i;
    }
    void sort(vector<int>&nums, int left, int right){
        if(left >= right)return;
        int i = partition(nums, left, right);
        sort(nums, left, i-1);
        sort(nums, i+1, right);
    }
    vector<int> sortArray(vector<int>& nums) {
        sort(nums, 0, nums.size()-1);
        return nums; 
    }
```

