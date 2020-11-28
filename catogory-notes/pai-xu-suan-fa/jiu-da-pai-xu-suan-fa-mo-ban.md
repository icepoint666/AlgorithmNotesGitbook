# 六大排序算法模板

### 快速排序 

**基本模板：4个关键点（注释上）**

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
    sort(nums, left, i-1);  //注意nums[i]就不用再放进去排了
    sort(nums, i+1, right);
}
vector<int> sortArray(vector<int>& nums) {
    sort(nums, 0, nums.size()-1);
    return nums; 
}
```

**更优的快速排序实现：**

* **①Median-of-3** 
* **②当 元素个数 &lt; threshold 使用insertion sort**
* **③尾递归优化**

**（关键在于median-of-3的实现）**

```cpp
int partition(vector<int>&nums, int i, int j){
    /*难点：median-of-three*/
    int mid = (i + j) /2;
    if(nums[i] > nums[mid])swap(nums[mid], nums[i]);
    if(nums[i] > nums[j])swap(nums[i], nums[j]);
    if(nums[mid] > nums[j])swap(nums[mid], nums[j]); //不符合left <= mid <= right的话交换
    swap(nums[i], nums[mid]); //把基准换到左边
    int val = nums[i];

    while(i != j){
        while(i < j && val <= nums[j])j--;
        swap(nums[i], nums[j]);
        while(i < j && val >= nums[i])i++; 
        swap(nums[i], nums[j]);
    }
    return i;
}
int insertionSort(vector<int>&nums, int i, int j);

void sort(vector<int>&nums, int left, int right){
    if(left >= right)return;
    if(left + 10 >= right){ //长度如果小于等于10，就直接插入排序
        insertionSort(nums, left, right);
        return;
    }
    int i = partition(nums, left, right);
    sort(nums, left, i-1);
    sort(nums, i+1, right);
}
vector<int> sortArray(vector<int>& nums) {
    sort(nums, 0, nums.size()-1);
    return nums; 
}
```

### 归并排序

