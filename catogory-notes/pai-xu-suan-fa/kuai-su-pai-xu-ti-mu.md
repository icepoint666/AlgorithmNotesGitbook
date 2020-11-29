# 快速排序题目

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指Offer 40 | 最小的K个数（TopK） | 多种做法 | 重点 |

### **剑指Offer 40.  TopK**

**做法1：快速排序变体（O\(N\)）**

经过快速排序算法中的一次划分后，基点左边的所有数小于基点，右边的所有数大于基点，基点位置pivot有三种情况：

* pivot == k 说明基点就是第k+1个小的元素，其左边的子数组就是最小的k个数。此时的子数组\[0, k\) 就是答案 
* pivot &gt; k 说明基点"偏大"了，对其左子数组继续进行划分 
* pivot &lt; k 说明基点"偏小"了，对其右子数组继续进行划分

```cpp
class Solution {
   public:
    int partition(vector<int>& arr, int left, int right) {
        int pivot = left;
        int lt = left + 1;
        int gt = right;
        while (true) {
            while (lt <= right && arr[lt] < arr[pivot]) lt++;
            while (gt >= left && arr[gt] > arr[pivot]) gt--;
            if (lt > gt) break;
            swap(arr[lt], arr[gt]);
            lt++;
            gt--;
        }
        swap(arr[pivot], arr[gt]);
        return gt;
    }

    vector<int> smallestK(vector<int>& arr, int k) {
        // 边界情况特殊处理
        if(k >= arr.size()) {
            return arr;
        } else if (k <= 0) {
            return {};
        } else { 
            int left = 0, right = arr.size() -1;
            do {
                int pivot = partition(arr, left, right);
                if(pivot == k){
                    break;
                }else if(pivot > k){
                    right = pivot - 1;
                }else {
                    left = pivot + 1;
                }
            }while(true);        

            vector<int> result(arr.begin(), arr.begin() + k);
            return result;
        }
        
    }
};
```

**做法2：最大堆，最小堆（堆排序）（O\(NlogK\)）**

