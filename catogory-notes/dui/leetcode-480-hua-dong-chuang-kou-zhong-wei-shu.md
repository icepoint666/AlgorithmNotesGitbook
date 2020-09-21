# Leetcode 480 滑动窗口中位数

### 题目

给你一个数组 nums，有一个大小为 k 的窗口从最左端滑动到最右端。窗口中有 k 个数，每次窗口向右移动 1 位。你的任务是找出每次窗口移动后得到的新窗口中元素的中位数，并输出由它们组成的数组。

示例：

给出 nums = \[1,3,-1,-3,5,3,6,7\]，以及 k = 3。

```text
窗口位置                      中位数
---------------               -----
[1  3  -1] -3  5  3  6  7       1
 1 [3  -1  -3] 5  3  6  7      -1
 1  3 [-1  -3  5] 3  6  7      -1
 1  3  -1 [-3  5  3] 6  7       3
 1  3  -1  -3 [5  3  6] 7       5
 1  3  -1  -3  5 [3  6  7]      6
```

 因此，返回该滑动窗口的中位数数组 \[1,-1,-1,3,5,6\]

### 整体思路

类似于之前一道**数据流中的中位数\(**剑指 Offer 41\)，但是之前并不需要从中删除元素

这道题的难度更大，需要维护一个滑动窗口的中位数，也就是需要删除元素

* 1.双堆：**最大堆\(lo堆\)+最小堆\(hi堆\)**来维护中位数
  * 如果是奇数，中位数就是最大堆的max
  * 如果是偶数，中位数就是\(最大堆的max + 最小堆的min\)/2.0
* 2.延迟删除技术：对于滑窗过期元素，现在**hash\_table**标记上该元素，直到这个元素出现在堆顶top再真正从堆中删除
  * 用两个hash\_table分别记录lo堆的删除情况，与hi堆的删除情况（这里易错：不能只用一个hash\_table来做，原因后面会提到）
  * 记录一个**balance标记**，虽然延迟移除了，但是平衡性是需要保证的，来动态记录平衡性

### 具体实现细节 + 坑 + 易错样例

**1.代码风格：**

整体实现最好能够把操作封装在函数里，避免过多的代码复用，也有助于调试的时候理解

**2. 初始化前K个元素**的时候，有一个小技巧可以更加简单一点

先都加在lo堆上，然后再把k/2个top元素移动到hi堆上

**3. 注意int + int返回double的时候要防止爆Int**

会有一个特殊样例： \[2147483647,2147483647\],1 如果不注意这点，就会WA

所以最好有一个良好的编程习惯

**4. 要给各自堆维护一个hash\_table的问题**

最开始只使用了一个hash\_table，会导致的问题就是在下面情况下：

lo:\[3,7,8\] hi:\[8,8\] 

hash\_table: \[7,8\]

根据单个hashtable来删除很容易导致，本身开始移除的是lo堆，但是因为lo堆hi堆都有这个元素，元素相同，所以可能到延迟移除的时候移除成了hi堆（这个bug调试了很久）

所以对于这种延迟移除技术，有几个堆就维护几个hash\_table

**5. checkTop的放置位置问题**

有些时候已经平衡了balance值，例如这种：

lo:\[3,7,8\] hi:\[8,8\] 

hash\_table: \[7,8\]

但是这时候还是要checkTop，所以要放在前面

```cpp
void incBalance(int target){
    checkHighTop(); 
    while(balance < target){
        lo.push(hi.top());
        hi.pop();
        balance += 2;
        checkHighTop();
    }
}
//下面是错误的
void incBalance(int target){
    while(balance < target){
        checkHighTop(); 
        lo.push(hi.top());
        hi.pop();
        balance += 2;
    }
    checkHighTop();
}
```

**6**. **其他细节问题**

* 调用top\(\)的时候都要判断一下可能不可能为空，如果可能要在前面加!lo.empty\(\)
* 判断Hashtable元素值之前也要加hash\_table.count\(e\)&gt;0判定

### 代码

```cpp
class Solution {
private:
    int i; 
    int balance;
    unordered_map<int, int>hash_table_lo;
    unordered_map<int, int>hash_table_hi;//4.两个哈希表很关键！！
    vector<double>medians;
    priority_queue<int>lo;
    priority_queue<int, vector<int>, greater<int> >hi;
public:
    void initializePrevK(vector<int>& nums, int k){
        while(i < k){
            lo.push(nums[i++]);
        }
        for(int j = 0; j < k / 2; j++){
            hi.push(lo.top());
            lo.pop();
        }//注意这个初始小技巧
        if(k % 2){
            balance = 1;
        }
    }

    void returnMedian(int k){
        if(k % 2){
            medians.push_back(lo.top());
        }else{
            medians.push_back(((double)lo.top() + (double)hi.top()) / 2.0); 
            //3.一个坑，防止overflow
        }
    }
    //延迟移除原则：保证栈顶最终结果不能有hash_table标记的元素
    void delayRemove(vector<int>& nums, int k){
        if(!hi.empty() && nums[i-k] >= hi.top()){
            balance += 1;
            hash_table_hi[nums[i-k]]++;
        }else if(!lo.empty() && nums[i-k] <= lo.top()){
            balance -= 1;
            hash_table_lo[nums[i-k]]++;
        }
    }
    void add(int num){
        if(!hi.empty() && num >= hi.top()){
            balance -= 1;
            hi.push(num);
        }else{
            balance += 1;
            lo.push(num);
        }
    }
    void checkHighTop(){
        while(!hi.empty() && hash_table_hi.count(hi.top()) > 0 && hash_table_hi[hi.top()] > 0){
            hash_table_hi[hi.top()]--;
            hi.pop();
        }
}
    void incBalance(int target){
        checkHighTop(); //5.这个check函数放在循环的位置很关键，刚开始放的位置有问题debug了很久
        while(balance < target){
            lo.push(hi.top());
            hi.pop();
            balance += 2;
            checkHighTop();
        }
    }
    void checkLowTop(){
        while(!lo.empty() && hash_table_lo.count(lo.top()) > 0 && hash_table_lo[lo.top()] > 0){
            hash_table_lo[lo.top()]--;
            lo.pop();
        }
    }
    void decBalance(int target){
        checkLowTop();
        while(balance > target){
            hi.push(lo.top());
            lo.pop();
            balance -= 2;
            checkLowTop();
        }
    }
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        i = 0, balance = 0;
        initializePrevK(nums, k);
        while(true){
            returnMedian(k);
            if(i == nums.size())break;
            //先移除，再添加，再维护平衡
            delayRemove(nums,k);
            add(nums[i]);
            //维护最终平衡
            if(k % 2){
                decBalance(1);
                incBalance(1);
            }else{
                decBalance(0);
                incBalance(0);
            } 
            i++;
        }
        return medians;
    }
};
```

