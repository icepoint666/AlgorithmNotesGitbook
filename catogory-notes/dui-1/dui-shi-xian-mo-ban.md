# 堆实现模板

**全部都依赖这个维护树的函数**

**heap通过vector实现，len记录heap的长度**

```cpp
void minHeap(vector<int>& heap, int parent, int len){
    int child = 2 * parent + 1;
    while(child < len){
        if(child + 1 < len && heap[child] > heap[child+1])child++;
        if(heap[parent] > heap[child]){
            swap(heap[parent], heap[child]);
            parent = child;
            child = 2 * parent + 1;
        }else break;
    }
}
```

### 创建堆

例子：0 - 12 - 3456最底层的3456不用考虑

```cpp
vector<int>heap;
heap.reserve(k);
for(int i = 0; i < k; i++){
    heap.push_back(nums[i]);
}
for(int i = k / 2 - 1; i >= 0; i--){ 
    minHeap(heap, i, k);
}
```

### 删除堆

最小的一定是heap\[0\]，拿最后一个位置的替换上去，然后维护最后一个node

```cpp
 heap[0] = heap[k-1];
 minHeap(heap, 0, k);
```

### 插入堆

```cpp

```





