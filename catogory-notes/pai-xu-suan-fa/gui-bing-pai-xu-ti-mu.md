# 归并排序题目

**题目**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5E8F;&#x53F7;/&#x96BE;&#x5EA6;</th>
      <th style="text-align:left">&#x540D;&#x5B57;</th>
      <th style="text-align:left">&#x5907;&#x6CE8;</th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#x5251;&#x6307; Offer 51</td>
      <td style="text-align:left">&#x6570;&#x7EC4;&#x4E2D;&#x7684;&#x9006;&#x6570;&#x5BF9;</td>
      <td style="text-align:left">&#x5F52;&#x5E76;&#x6392;&#x5E8F;</td>
      <td style="text-align:left">&#x4E2D;&#x7B49;</td>
    </tr>
    <tr>
      <td style="text-align:left">148</td>
      <td style="text-align:left">&#x6392;&#x5E8F;&#x94FE;&#x8868;</td>
      <td style="text-align:left">
        <p>&#x8981;&#x6C42;&#x65F6;&#x95F4;&#xFF1A;O(NlogN)&#xFF0C;&#x7A7A;&#x95F4;&#xFF1A;&#x5E38;&#x6570;</p>
        <p>&#xFF08;&#x5176;&#x5B9E;&#x8FD9;&#x91CC;&#x5B9E;&#x73B0;&#x7684;&#x5F52;&#x5E76;&#xFF0C;&#x6CA1;&#x6709;&#x505A;&#x5230;&#x7A7A;&#x95F4;&#x4E3A;&#x5E38;&#x6570;&#x8FD9;&#x4E00;&#x70B9;&#xFF09;</p>
      </td>
      <td style="text-align:left">&#x4E2D;&#x7B49;</td>
    </tr>
    <tr>
      <td style="text-align:left">378</td>
      <td style="text-align:left">&#x6709;&#x5E8F;&#x77E9;&#x9635;&#x4E2D;&#x7B2C;k&#x5C0F;&#x7684;&#x5143;&#x7D20;</td>
      <td
      style="text-align:left">&#x5F52;&#x5E76;&#x6392;&#x5E8F;</td>
        <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

**剑指 Offer 51. 数组中的逆序对**

归并排序merge的时候，后半段的数如果要往前排时

计算前半段剩余比它大的数有几个，然后这个个数记在count上

**有一个坑：见代码注释，一旦写的是小于号，那么等序对也会被count进去**

```cpp
int count;
void mergeSort(vector<int>&nums, int left, int right){
    if(left >= right)return;
    int mid = (left + right) / 2;
    mergeSort(nums, left, mid);
    mergeSort(nums, mid+1, right);
    int temp[right-left+1];
    int i = left, j = mid+1, x = 0;
    while(i <= mid && j <=right){
        if(nums[i] <= nums[j])//这里的判断条件一定是小于等于，不然会多算上等序对
            temp[x++] = nums[i++]; 
        else{
            temp[x++] = nums[j++];
            count += mid + 1 - i;
        }
    }
    while(i <= mid)temp[x++] = nums[i++];
    while(j <= right)temp[x++] = nums[j++];
    for(int i = left; i <= right; i++){
        nums[i] = temp[i-left];
    }
}
int reversePairs(vector<int>& nums) {
    int len = nums.size();
    if(len <= 1)return 0;
    count = 0;
    mergeSort(nums, 0, len-1);
    return count;
}
```

