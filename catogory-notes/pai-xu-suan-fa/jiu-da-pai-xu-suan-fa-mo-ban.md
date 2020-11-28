# 六大排序算法模板

### 快速排序 \(普通版 + 优化版）

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
    if(left >= right)return; // 如果insertionSort有判边界，就可以省去这一句
    if(left + 10 >= right){ //长度如果小于等于10，就直接插入排序
        insertionSort(nums, left, right);
        return;
    }
    while(left < right){  //尾递归优化
        int pivot = partition(nums, left, right);
        sort(nums, left, pivot-1);
        left = pivot + 1;
    }
}
vector<int> sortArray(vector<int>& nums) {
    sort(nums, 0, nums.size()-1);
    return nums; 
}
```

### 归并排序 \(普通版 + inplace版）

**普通版**

```cpp
void mergeSort(vector<int>& nums, int left, int right){
    if(left >= right)return;
    vector<int>temp; //尽量用int[] 
    temp.reserve(right-left+1);
    int mid = (left + right) / 2;
    mergeSort(nums, left, mid);
    mergeSort(nums, mid+1, right);
    int i = left, j = mid + 1;
    while(i <= mid && j <= right){
        if(nums[i] < nums[j])temp.push_back(nums[i++]);
        else temp.push_back(nums[j++]);
    }
    while(i <= mid)temp.push_back(nums[i++]);
    while(j <= right)temp.push_back(nums[j++]);
    for(int i = left; i <= right; i++){
        nums[i] = temp[i-left];
    }
}
vector<int> sortArray(vector<int>& nums) {
    mergeSort(nums, 0, nums.size()-1);
    return nums;
}
```

#### inplace版**（时间开销很大）**：

#### 在归并排序上改进，以时间复杂度换空间复杂度，利用元素反转完成排序

 在原地归并排序中最主要用到了**内存反转**，即交换相邻的两块内存

**内存反转**是给定序列a1,a2,...,an,b1,b2,...,bn,反转成 b1,b2,...,bn,a1,a2,...,an

**merge的原理**

![](../../.gitbook/assets/wu-biao-ti-%20%286%29.png)

![](../../.gitbook/assets/wu-biao-ti-%20%287%29.png)

（**核心：主要是reverse要反转三次**）

```cpp
void reverse(vector<int>&nums, int left, int right){
    int i = left, j = right;
    while(i < j){
        swap(nums[i++], nums[j--]);
    }
}
void convert(vector<int>&nums, int left, int mid, int right){//内存反转
    reverse(nums, left, mid);
    reverse(nums, mid+1, right);
    reverse(nums, left, right);
}
void merge(vector<int>& nums, int left, int mid, int right){
    int i = left, j = mid + 1;
    while (i < j && j <= right){
       while (i < j && nums[i] <= nums[j])i++;
       int index = j;
       while (j <= right && nums[j] < nums[i])j++;
       convert(nums, i, index - 1, j - 1);
       i += j - index;
    }
}
void mergeSort(vector<int>& nums, int left, int right){
    if(left >= right)return;
    int mid = (left + right) / 2;
    mergeSort(nums, left, mid);
    mergeSort(nums, mid+1, right);
    merge(nums, left, mid, right);
}
vector<int> sortArray(vector<int>& nums) {
    mergeSort(nums, 0, nums.size()-1);
    return nums;
}
```

### 堆排序

 一种**选择排序，**每趟从待排序的记录中选出关键字最小的记录，顺序放在已排序的记录序列末尾，直到全部排序结束为止。

① **构造初始堆。将给定无序序列构造成一个大顶堆（一般升序采用大顶堆）**

**这一点有点点反直觉，也就是升序小数在前应该是对应最大堆**

* 初始给定无序序列结构如下

![](../../.gitbook/assets/1024555-20161217192038651-934327647.png)

* 我们从最后一个非叶子结点开始（叶结点自然不用调整，第一个非叶子结点 arr.length/2-1=5/2-1=1，也就是下面的6结点），从左至右，从下至上进行调整。

![](../../.gitbook/assets/1024555-20161217192209433-270379236.png)

![](../../.gitbook/assets/1024555-20161217192854636-1823585260.png)

![](../../.gitbook/assets/1024555-20161217193347886-1142194411.png)

* 最终形成一个大顶堆

2. **将堆顶元素与末尾元素进行交换，使末尾元素最大。（相当于把最大元素从堆中移除，放在最尾端，所以就已经排好了序）然后继续调整堆，再将堆顶元素与末尾元素交换，得到第二大元素。如此反复进行交换、重建、交换。**

![](../../.gitbook/assets/1024555-20161217194207620-1455153342.png)

```cpp
void maxHeap(vector<int>& nums, int parent, int len){ 
/*从上往下，与两个child比较，如果比child小，就要交换位置，因为是大顶堆*/
    int child = 2 * parent + 1; 
    while(child < len){
        if(child + 1 < len && nums[child] < nums[child + 1])child++; //
        if(nums[child] > nums[parent]){
            swap(nums[child], nums[parent]);
            parent = child;
            child = 2 * parent + 1;
        }else break;
    }
}
void heapSort(vector<int>& nums){
    int len = nums.size();
    for(int i = (len - 1) / 2; i >= 0; i--){ //调整非叶子节点
        maxHeap(nums, i, len);
    }/*构成大顶堆*/
    
    for(int j = len; j > 0; j--){
        maxHeap(nums, 0, j); //每次相当于移除一个最大元素到堆尾
        swap(nums[0], nums[j-1]);
    }
}
vector<int> sortArray(vector<int>& nums) {
    heapSort(nums);
    return nums;
}
```

### 桶排序

### 基数排序

### 插入排序

认定元素i左边已经是从小到大排好序的部分  
从**nums\[i\]**往左边找\(j--\)，找到刚好比nums\[j\]大的插入到nums\[j+1\]的位置

```cpp
 vector<int> insertionSort()(vector<int>& nums) {
    for(int i = 1; i < nums.size(); i++){
        int key = nums[i];
        int j = i - 1;
        while(j >= 0 && nums[j] > key){
            nums[j+1] = nums[j];
            j--;
        }
        nums[j+1] = key;
    }
    return nums;
}
```

### 冒泡排序

```cpp
vector<int> bubbleSort(vector<int>& nums){
    for(int i = 0; i < nums.size() - 1; i++){ //注意边界[0,n-1)
        for(int j = 0; j < nums.size() - 1 - i; j++){ //注意边界[0,n-1-i)
             if(nums[j] > nums[j+1])swap(nums[j], nums[j+1]);
        }
    }
    return nums;
}
```

