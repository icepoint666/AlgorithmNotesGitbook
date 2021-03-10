# 二分查找

**二分查找公式化**

**二分的特征很明显：有序数组皆二分**

要思考的问题（非常容易乱）

* ①left = 0, right取size还是size-1
* ②**注意一点**，mid = \(left + right\) / 2, **一定不能设置left = mid**不然会无限循环
* ③终止条件是left &lt; right还是left &lt;= right
  * **注意 left &lt; right对应找左右边界的时候，right = size**
* ④判断的时候nums\[mid\]跟谁比
  * 大部分查找值或者找边界的时候是跟target比: num\[mid\] &lt; target
  * 例如旋转数组这种题目会跟num\[right\]比: num\[mid\] &gt; num\[right\]
  * 还有一些找峰值num\[mid\] &lt; num\[mid+1\] && mid + 1 &lt; right
* ⑤比几种情况
  * 三种情况：大于，等于，小于（找target, 找旋转数组最小值）
  * 两种情况：大于等于，小于（找target左侧边界）
* ⑥到底结束条件的时候要找的目标是left还是right还是left-1
  * 返回left-1要注意边界判定，是不是为负数
  * 通过left+1的方式返回left，也要注意边界判定，注意是不是大于size
* ⑦**注意一点：**如果需要当前位置前移或者后移，不要控制mid++，而是控制left++或者right--



