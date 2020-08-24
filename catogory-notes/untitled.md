# 滑动窗口（Sliding Window）

滑动窗口类型的题目经常是用来执行数组或是链表上某个区间（窗口）上的操作。比如找最长的全为1的子数组长度。滑动窗口一般从第一个元素开始，一直往右边一个一个元素挪动。

当然了，根据题目要求，我们可能有固定窗口大小的情况，也有窗口的大小变化的情况。

**题目特征**

* 这个问题的输入是一些线性结构：比如链表呀，数组啊，字符串啊之类的
* 让你去求最长/最短子字符串或是某些特定的长度要求

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 239（剑指offer59） | 滑动窗口最大值 | 单调栈\(deque实现）+ 窗口判定 | 中等 |
| 76 | 最小覆盖子串 | 双指针（right右扫描，left左优化） | 中等 |
| 438 | 找到字符串中字母异位词 | 与上题类似滑窗+双指针+双计数map | 简单 |
| 3（剑指offer48\) | 无重复字符的最长子串 | 一样的双指针+滑窗 | 中等 |
| 剑指offer57-II | 和为s的连续正数序列 | 滑窗，但是注意细节，容易出现一些小瑕疵 | 易错 |

#### 题目笔记

**239. 滑动窗口最大值**

长度k在长度n数组滑窗，找最大值

①暴力法：时间复杂度O\(nk\)

②双端队列+单调栈：时间复杂度O\(n\)

**本题关键点：\[3,4,1,2\]如果最新加入的4比之前的3要大，那么之前的就绝对不可能是最大值，就可以删了**

递减栈：如果新加入元素比尾部元素大的，删除掉尾部元素，然后继续检查删除

```cpp
while(!deq.empty() && nums[i] > nums[deq.back()]){
                deq.pop_back();
            }
```

双端队列，每次滑窗都要维护队头是否已经出区间

```cpp
if (!deq.empty() && deq.front() < i - k + 1) deq.pop_front()
```

**76. 最小覆盖子串**

易错：有可能字符串T有多个重复字符"AAA"，那么就不是单纯寻找最近字符位置那么简单（第一次做错了）

这个易错点决定了，不是单一记录字符是否存在的map那么简单，而是要有计数器的map

暴力法：远大于O\(N^2\)不适合

更好解法： O\(M+N\) **滑动窗口+左右指针的技巧**

1、我们在字符串 S 中使用双指针中的左右指针技巧，初始化 left = right = 0，把索引闭区间 \[left, right\] 称为一个「窗口」。

2、我们先不断地增加 right 指针扩大窗口 \[left, right\]，直到窗口中的字符串符合要求（包含了 T 中的所有字符）。

3、此时，我们停止增加 right，转而不断增加 left 指针缩小窗口 \[left, right\]，直到窗口中的字符串不再符合要求（不包含 T 中的所有字符了）。同时，每次增加 left，我们都要更新一轮结果。

4、重复第 2 和第 3 步，直到 right 到达字符串 S 的尽头。

第 2 步相当于在寻找一个「可行解」，然后第 3 步在优化这个「可行解」，最终找到最优解。左右指针轮流前进，窗口大小增增减减，窗口不断向右滑动。

**针对于前面的易错：需要两个计数器：needs 和 window 相当于计数器，分别记录 T 中字符出现次数和窗口中的相应字符的出现次数。**

**实现：**用两个哈希表当作计数器解决。用一个哈希表 needs 记录字符串 t 中包含的字符及出现次数，用另一个哈希表 window 记录当前「窗口」中包含的字符及出现的次数，如果 window 包含所有 needs 中的键，且这些键对应的值都大于等于 needs 中的值，那么就可以知道当前「窗口」符合要求了，可以开始移动 left 指针了。

```cpp
unordered_map<char, int> window;
unordered_map<char, int> needs;
for (char c : t) needs[c]++;
```

更新：

```cpp
while (match == needs.size()) {
    if (right - left < minLen) {
        // 更新最小子串的位置和长度
        start = left;
        minLen = right - left;
    }
    ...
}
return minLen == INT_MAX ?
                "" : s.substr(start, minLen);
```

**438. 找到字符串中所有字母异位词**

**个人做法（偏暴力）：**

**窗口是固定的，没有用双指针，单纯处理边界然后记录**

与上题类似，滑动窗口，维护两个哈希表充当计数器，一个来计算并维护滑窗内的情况，一个计算窗口字符串

\*\*\*\*

**Trick1:双指针**

双指针初始化，关键在于两个指针动态的移动：

**right指针先满足条件，left指针再缩小窗口，然后不行的话right再继续增大窗口凑条件**

```cpp
int left = 0, right = 0;
```

**Trick2:一个骚操作**

如果用map充当一个计数器，刚开始添加的时候直接`mp[key]++`; 就默认加入值为1了，不需要复杂的验证是否存在然后从0开始加。

```cpp
unordered_map<char, int> needs;
for (char c : t) needs[c]++;
```

**为了避免单独对窗口没有填满的部分进行特殊判断，双指针设定：**

**Trick3:三步:**

* 维护right指针相关的计数判断，右移right++
* 如果匹配条件，但是window大小不太合适，就处理left指针
* 维护left指针相关的计数判断，右移left++

**Trick4:验证两个计数器相等的方法，不需要O\(mp.size\(\) \* n\)的复杂度**

用一个match来记录当前匹配两个计数器的匹配个数，每次更新的时候只处理元素更改的前后两个key即可

```cpp
unordered_map<char, int> window;

while (right < s.size()) {
        //维护right指针相关的计数判断，添加
        char c1 = s[right];
        if (needs.count(c1)) {
            window[c1]++;
            //Trick4
            if (window[c1] == needs[c1])
                match++;
        }
        right++;

        while (match == needs.size()) {
            // 如果 window 的大小合适
            // 就把起始索引 left 加入结果
            if (right - left == t.size()) {
                res.push_back(left);
            }
            //维护left指针相关的计数判断，移去
            char c2 = s[left];
            if (needs.count(c2)) {
                window[c2]--;
                //Trick4
                if (window[c2] < needs[c2])
                    match--;
            }
            left++;
        }
    }
```

**3. 无重复字符的最长子串**

**直接贴代码：**

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int left = 0, right = 0;
        unordered_map<char, int> window;
        int n = s.size();
        int maxLength = 0;
        while(right < n){
            window[s[right]]++;
            while(window[s[right]]>1){
                window[s[left]]--;
                left++;
            }
            right++;
            maxLength = max(maxLength, right - left);
        }
        return maxLength;
    }
};
```

