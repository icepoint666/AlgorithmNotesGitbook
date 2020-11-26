# 归并排序

### **归并排序（分治算法）基本模板**

```cpp
void mergesort(vector<int>&nums, int l, int r){
    if(l + 1 == r)return;
    int mid = (l + r) / 2;
    mergesort(nums, l, mid);
    mergesort(nums, mid, r);
    int tmp[r - l]; //占用空间,用int[]会比vector快很多
    int i = l, j = mid, idx = 0;
    while(i < mid && j < r){
        if(nums[i] <= nums[j])tmp[idx++] = nums[i++];
        else tmp[idx++] = nums[j++];
    }
    while(i < mid){
        tmp[idx++] = nums[i++];
    }
    while(j < r){
        tmp[idx++] = nums[j++];
    }
    for(int i = 0; i < r - l; i++)
        nums[l+i] = tmp[i];
    return;
}
```

### 归并排序：inplace版的mergesort

用时间换空间 [https://www.cnblogs.com/Philip-Tell-Truth/p/5006910.html](https://www.cnblogs.com/Philip-Tell-Truth/p/5006910.html)

