# 近似有序问题

### 近似类型1：近似有序序列（即除掉少数K个元素后是有序序列且K&lt;&lt;n）

试分析**插入排序**，**冒牌排序**和**简单选择排序**的时间复杂度

**分析：**

这种问题其实使用**快速排序，归并排序效率不一定高**

最好就是 **插入排序**

**示例：\[1,2,3,4,6,5,7,8,9,11,10,12\]**

**插入排序**的思路是**跟前面已经排好序的部分**比，例如4发现比3大直接就满足要求了，5发现没有6大通过交换来实现有序

* 比较/交换复杂度O\(k\*n\)，因为k&lt;&lt;n，近似认为**O\(n\)**

**冒泡排序**，其基本操作为元素间比较的次数，因此无论  
是否是近似有序，时间复杂度均为**O（n^2）**

**选择排序**，其基本操作也为元素间的比较，与待排序列状态无关，即第i次的  
比较次数为n-i,时间复杂度为**O（n^2）**

### **近似类型2：几乎有序的数组（每个元素的移动距离不超过k）**

#### **解决：堆排序（只用大小为k）**

* 建立一个**大小为k的小根堆**
* 并**取堆顶元素保存到原数组中，**加入数组中的后一个元素并对堆结构进行调整，**始终维持大小为k的小根堆**。
* 采用**普通堆排序**的方法将**最后k个元素**排序并保存到原数组中。

```cpp
void minHeap(vector<int>& nums, int parent, int len){ 
/*从上往下，与两个child比较，如果比child小，就要交换位置，因为是大顶堆*/
    int child = 2 * parent + 1; 
    while(child < len){
        if(child + 1 < len && nums[child] > nums[child + 1])child++; //
        if(nums[child] < nums[parent]){
            swap(nums[child], nums[parent]);
            parent = child;
            child = 2 * parent + 1;
        }else break;
    }
}
void sortAlmostArray(vector<int>& nums, int k){
    /*构成小顶堆*/
    int len = nums.size();
    vector<int>heap;
    heap.reserve(k);
    for(int i = 0; i < k; i++){
        heap.push_back(nums[i]);
    }
    for(int i = (k - 1) / 2; i >= 0; i--){
        minHeap(heap, i, k);
    }
    /*将堆顶元素赋给nums，维护大小为k的堆*/
    for(int i = k; i < len; i++){
        nums[i-k] = heap[0];
        heap[0] = nums[i];
        minHeap(heap, 0, k);
    }
    //后k个元素采用堆排序  
    for(int i = len - k; i < len; i++){
        nums[i] = heap[0];
        swap(heap[0], heap[k-1]);
        minHeap(heap, 0, --k);
    }
}
```

