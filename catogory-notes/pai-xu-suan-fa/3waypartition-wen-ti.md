# 快速排序3-way-partition变体

### Three-way Partition 

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

#### 代码模板

```cpp
void quickSort(int arr[],int low, int high)
{
	int first = low;
	int last = high;
 
	int left = low;
	int right = high;
 
	int leftLen = 0;
	int rightLen = 0;
 
	if (high - low + 1 < 10)
	{
		InsertSort(arr,low,high);
		return;
	}
 
	//一次分割
	int key =  NumberOfThree(arr,low,high);//使用三数取中选择枢轴
 
	while(low < high)
	{
		while(high > low && arr[high] >= key)
		{
			if (arr[high] == key)//处理相等元素
			{
				Swap(arr[right],arr[high]);
				right--;
				rightLen++;
			}
			high--;
		}
		arr[low] = arr[high];
		while(high > low && arr[low] <= key)
		{
			if (arr[low] == key)
			{
				Swap(arr[left],arr[low]);
				left++;
				leftLen++;
			}
			low++;
		}
		arr[high] = arr[low];
	}
	arr[low] = key;
 
	//一次快排结束，把与基准相等的元素移到基准周围
	int i = low - 1;
	int j = first;
	while(j < left && arr[i] != key)
	{
		Swap(arr[i],arr[j]);
		i--;
		j++;
	}
	i = low + 1;
	j = last;
	while(j > right && arr[i] != key)
	{
		Swap(arr[i],arr[j]);
		i++;
		j--;
	}
        quickSort(arr,first,low - 1 - leftLen);
        quickSort(arr,low + 1 + rightLen,last);
}

```

### **题目**

\*\*\*\*

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 75 | 颜色分类（荷兰国旗） | 对只存在0,1,2值的数组排序，双指针 | 理清关系 |
| 324 | 摆动排序-II | 关键：要求O\(n\)时间，O\(1\)空间 | 解题技巧 |

**75. 颜色分类**

**对一个只有0,1,2三种值的数组进行排序，要求只扫一遍**

**解法：双指针（处理左右指针时，很容易乱，理清思路）**

* left指针代表其左边都是0（初始left = 0，表示左边没有0）
* right指针代表其右边都是2（初始right = len - 1, 表示右边没有1）
* i表示当前处理节点，从0开始处理到right
  * 如果是2的话，右边其实0,1,2都有可能，将nums\[i\]与nums\[right\]交换，将right--
    * 如果nums\[right\]是2，其实不用交换，只用right--
    * 如果nums\[right\]是1，不用处理
    * 如果nums\[right\]是0，还要再给它交换到left，left++
  * 如果是0的话，交换到left，left++
  * 如果是1的话，不用处理

```cpp
void sortColors(vector<int>& nums) {
    int len = nums.size();
    if(len == 0)return;
    int left = 0, right = len - 1;
    for(int i = 0; i < len && i <= right; i++){
        if(nums[i] == 2){
            while(nums[right] == 2 && i < right)right--;
            swap(nums[i], nums[right--]);
        }
        if(nums[i] == 0)swap(nums[i], nums[left++]);
    }
}
```

\*\*\*\*

