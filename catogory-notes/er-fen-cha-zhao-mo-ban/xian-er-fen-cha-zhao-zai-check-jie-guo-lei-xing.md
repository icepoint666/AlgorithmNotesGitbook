# 先二分查找再check结果类型

**二分查找一类经典应用：**

往往是需要找到一个能完成什么工作的最小值，或者最大值

一般就是线性问题，二分查找一个中间值，计算出结果就能排除掉一般的情况，然后继续二分查找

**关键要想到这个解决办法**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 875 | 爱吃香蕉的珂珂 | 左边界二分查找模板，因为是找最小速度，但target值多个O\(n\*logn\) | 模板题 |
| 1011 | 在 D 天内送达包裹的能力 | 与上题一样做法，有一个坑：MIN\_BOUND开始不能取最小值 |  |
| lint 437 | 书籍复印 | 二分查找check结果 | 类型 |

 

**1011. 在 D 天内送达包裹的能力**

**题解：**标准的二分查找左边界模板，但是有一个**坑**就是必须要**限制W\_MIN**，不然如果搜索的时候载重过小的话，永远没法装上，就会导致无限循环

```cpp
class Solution {
public:
    int Day(vector<int>weights, int W){
        int d = 0, i = 0, count = 0;
        while(i < weights.size()){
            count += weights[i];
            if(count > W){
                d++;
                count = 0;
            }else{
                i++;
            }
        }if(count)d++;
        return d;
    }
    int shipWithinDays(vector<int>& weights, int D) {
        int W_MAX = 500*50000;
        int W_MIN = 1;
        for(int i = 0; i < weights.size(); i++) //限制最小范围
            W_MIN = max(weights[i], W_MIN);
        while(W_MIN < W_MAX){
            int mid = (W_MIN + W_MAX) / 2;
            int w = Day(weights, mid);
            if(w <= D){
                W_MAX = mid;
            }else{
                W_MIN = mid + 1;
            }
        }
        return W_MIN;
```

**书籍复印**

给定 n 本书, 第 i 本书的页数为 pages\[i\]. 现在有 k 个人来复印这些书籍, 而每个人只能复印编号连续的一段的书, 比如一个人可以复印 pages\[0\], pages\[1\], pages\[2\], 但是不可以只复印 pages\[0\], pages\[2\], pages\[3\] 而不复印 pages\[1\].

样例1：

```text
输入: pages = [3, 2, 4], k = 2 
输出: 5 
解释: 第一个人复印前两本书, 耗时 5 分钟. 第二个人复印第三本书, 耗时 4 分钟. 
```

样例2：

```text
输入: pages = [3, 2, 4], k = 3 
输出: 4 
解释: 三个人各复印一本书. 
```

**解法：**

答案的范围在max\(pages\)~sum\(page\), 利用二分查找搜索这个值，用贪心法从左到右扫描一下 pages，看看需要多少个人来完成抄袭。

如果这个值 &lt;= k，那么意味着大家花的时间可能可以再少一些，如果 &gt; k 则意味着人数不够，需要降低工作量。

