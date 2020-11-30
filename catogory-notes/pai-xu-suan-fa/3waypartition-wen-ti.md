# 快速排序变体：聚集元素, Kth

Three-way Partition （聚集元素）

在快速排序中，有一个**变种**是**将选择的pivot都放到中间**。 

例如 \[5,1,5,7,5,8,3,5\]，选择最末尾的 5 作为pivot，partition以后可以变成 \[3,1,5,5,5,5,8,7\]。

之后再partition时，把比5小的放在左边，把比5大的放在右边

也就是处理\[3,1\]与\[8,7\]，**对于重复元素比较多的聚集情况，这是一种很有效的优化**

**步骤**

* **选基准**：可以按照median-of-3选出一个
* ①在划分过程中将与基准值相等的元素放入数组两端
* ②划分结束后，再将两端的元素移到基准值周围。

**第①步：**

![](../../.gitbook/assets/20180925183624457.png)

![](../../.gitbook/assets/20180925183643177.png)

**第②步：**

![](../../.gitbook/assets/20180925183736304.png)

### **快排变体：聚集元素模板**

```cpp
//目标：将pivot的数聚集起来
//nums[left, p) < nums[pivot]
//nums[p, q) = nums[pivot]
//nums[q, right] > nums[pivot]
void quickSort(vector<int>& nums, int left, int right){
{
    //threeWayPartition
    int pivot = nums[right], p = left, q = p;
    for(int i = left; i < right; i++){
        if(nums[i] <= nums[pivot]){//仔细看一些发现，类似于threeColor的思路，维护p,q
            swap(nums[q++], nums[i]);//如果小于等于，先换到q的右边
            if(nums[q-1] < nums[pivot])swap(nums[p++],nums[q-1]);//如果是小于，再换到p的右边
    }
    swap(nums[q++], nums[right]); //处理最右边的pivot
    quickSort(nums, left, p-1);
    quickSort(nums, q, right);
}

```

### 快速变体：kth模板

```cpp

```

### 聚集元素 + kth结合版

```cpp
int threeWayPartition_Kth(vector<int>nums, int k){
    //确认K在[0,len)范围之内
    int len = nums.size();
    int left = 0, right = len - 1;
    while(true){
        //目标：将pivot的数聚集起来，并找到第k小的pivot
        //nums[left, p) < nums[pivot]
        //nums[p, q) = nums[pivot]
        //nums[q, right] > nums[pivot]
        //threeWayPartition
        int pivot = nums[right], p = left, q = p;
        for(int i = left; i < right; i++){
            if(nums[i] <= nums[pivot]){//仔细看一些发现，类似于threeColor的思路，维护p,q
                swap(nums[q++], nums[i]);//如果小于等于，先换到q的右边
                if(nums[q-1] < nums[pivot])swap(nums[p++],nums[q-1]);//如果是小于，再换到p的右边
        }
        swap(nums[q++], nums[right]);
        //find Kth
        if (k < p)//k在[left,p)
            right = p - 1;
        else if (k < q)//k在[p,q)
            return pivot;
        else {//k在[q,right]
            left = q;
        }
    }
}
```

### **题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 75 | 颜色分类（荷兰国旗） | 快排变体：**聚集元素** | 模板 |
| 剑指Offer 40 / 215 | 最小的k个数（TopK, kth） | 快排变体：**kth** | 解题技巧 |
| 324 | 摆动排序-II | 快排变体：**聚集元素** + **kth\(这里kth就是中位数\)** + **虚拟映射** | 解题技巧 |
|  |  |  |  |
|  |  |  |  |

**75. 颜色分类**

**对一个只有0,1,2三种值的数组进行排序，要求只扫一遍**

**解法：按照聚集元素模板来做,**

* **甚至不用递归更新left,right，只维护p，q**
* **最右边也不是pivot，pivot数值确定了=1**
  * nums\[left, p\) &lt; nums\[pivot\]
  * nums\[p, q\) = nums\[pivot\]
  * nums\[q, right\] &gt; nums\[pivot\]

```cpp
void sortColors(vector<int>& nums) {
   int len = nums.size();
   if(len == 0)return;
   int p = 0, q = p;
   int pivot_val = 1;
   for(int i = 0; i < len; i++){
      if(nums[i] <= pivot_val){
           swap(nums[q++], nums[i]);//如果小于等于，先换到q的右边
           if(nums[q-1] < pivot_val)swap(nums[p++],nums[q-1]);//如果是小于，再换到p的右边
      }
   }
}
```

**剑指Offer 40 / 215  TopK**

**快速排序变体（O\(N\)）**

经过**快速排序算法中的一次普通划分后**，基点左边的所有数小于基点，右边的所有数大于基点，基点位置pivot有三种情况：

* pivot = k 说明基点就是第k+1个小的元素，其左边的子数组就是最小的k个数。此时的子数组\[0, k\) 就是答案 
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

**另外做法：最大堆，最小堆（堆排序）O\(NlogK）**

\*\*\*\*

**324. 摆动排序 II**

 给定一个无序的数组 `nums`，将它重新排列成 `nums[0] < nums[1] > nums[2] < nums[3]...` 的顺序。

**题解：快排变体kth + 插空的时候虚拟映射**

**快排变体kth**

经过**快速排序算法中的一次划分后**，基点左边的所有数小于基点，右边的所有数大于基

 例如 \[5,1,5,7,5,8,3,5\]，选择末尾作为pivot，partition以后可以变成 \[3,1,5,5,5,5,8,7\]。 **用此方法可以将nums数组中的pivot都相邻在一起**。

**pivot不一定是中位数**

* pivot == mid 说明基点就是中位数，直接ok
* **pivot &lt; mid 那么把pivot左边数组部分，重新按照上述再进行一次快速排序划分**
* **pivot &gt; mid也是类似**

**直到pivot是中位数**

所以最终我们可以将数组**按中位数partition**，然后左边的数放0,2,4的位置，右边的放1,3,5的位置，**这里可以用非递归的实现达到 空间复杂度O\(1\)**。 （递归实现的平均空间复杂度为O\(logn\)\)

**当中位数的个数超过半数时，不存在可行解**

\*\*\*\*

**将数组的两段插空，一般需要额外的空间，但通过巧妙的下标映射可以实现自动插空**

对于所有数组元素的访问，我们可以实现一个虚拟的映射

例如，当长度为 8时， \[a, b, c, d, e, f, g, h\] -&gt; \[a, e, b, f, c, g, d, h\]。 当长度为 9时， \[a, b, c, d, e, f, g, h, i\] -&gt; \[a, f, b, g, c, h, d, i, e\]。 具体的下标映射公式是： 

```cpp
i <= (n - 1) / 2 ? i * 2 : (i - (n + 1) / 2) * 2 + 1
```

i 其中 n 为数组长度， i 为虚拟下标。 

例如，当数组长度为 9 切访问的虚拟下标为 2 时，实际访问的下标为 5

```cpp

```

