# 概率题



**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 剑指 Offer 60 | n个骰子的点数 | dp储存概率 | 简单 |
| 382 | 链表随机节点 | 蓄水池采样算法 | 技巧 |
| 470 | 用rand7\(\)实现rand10\(\) | 乘法，拒绝采样，整除 | 技巧 |

**剑指 Offer 60. n个骰子的点数**

把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。  
****输入: 2 

输出: \[0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778\]

```cpp
class Solution {
public:
    vector<double> twoSum(int n) {
        double dp[n][n * 6 + 1];
        memset(dp, 0, sizeof(dp));
        for(int i = 1; i <= 6; i++)dp[0][i] = 1.0/6;
        double one[7] = {0, 1.0/6, 1.0/6, 1.0/6, 1.0/6, 1.0/6, 1.0/6};
        for(int k = 1; k < n; k++){
           for(int i = 1; i <= 6; i++){
               for(int j = 1; j <= k * 6; j++){
                    dp[k][i + j] += dp[k-1][j] * one[i];
               }
           }
        }
        vector<double>res;
        for(int i = n; i <= n*6; i++){
            res.push_back(dp[n-1][i]);
        }
        return res;
    }
};
```

**382. 链表随机节点**

给定一个单链表，随机选择链表的一个节点，并返回相应的节点值。保证每个节点被选的概率一样。

进阶: 如果链表十分大且长度未知，如何解决这个问题？你能否使用常数级空间复杂度实现？

**大数据流中的随机抽样、蓄水池抽样**

面试中经常出现的在大数据流中的随机抽样问题，

即：当内存无法加载全部数据时，如何从包含未知大小的数据流中随机选取k个数据，并且要保证每个数据被抽取到的概率相等。本题为k = 1 时的情况。 

假设数据流含有N个数，我们知道如果要保证所有的数被抽到的概率相等，那么每个数抽到的概率应该为 1/N。那如何保证呢？ 

解决方案：每次只保留一个数，当遇到第 i 个数时，以 1/i的概率保留它，\(i-1\)/i的概率保留原来的数。 

举例说明： 1 ，2，3， 4

* 遇到1，概率为1的保留第一个数（作为选取的数据对象）。 
* 遇到2，概率为1/2的选取这个数作为那个被选的数据对象，这个时候，1和2各1/2的概率被作为选取对象 
* 遇到3，3被保留的概率为1/3，\(之前剩下的数假设1被保留\)，2/3的概率 1 被保留，\(此时1被保留的总概率为 2/3  _1/2 = 1/3\)_ 
* _遇到4，_ 以此类推，每个数被保留的概率都是1/N。

```cpp
ListNode* h;
Solution(ListNode* head) {
    h = head;
}

/** Returns a random node's value. */
int getRandom() {
    ListNode* node = h;
    int ret = 0;
    int cnt = 0;
    while(node){
        cnt++;
        int i = rand() % cnt + 1;
        if(i == cnt){
            ret = node->val;
        }
        node = node->next;
    }
    return ret;
}
```

**470. 用 Rand7\(\) 实现 Rand10\(\)**

**参考：**[**https://leetcode-cn.com/problems/implement-rand10-using-rand7/solution/cong-zui-ji-chu-de-jiang-qi-ru-he-zuo-dao-jun-yun-/**](https://leetcode-cn.com/problems/implement-rand10-using-rand7/solution/cong-zui-ji-chu-de-jiang-qi-ru-he-zuo-dao-jun-yun-/)\*\*\*\*

```cpp
int rand10() {
    while(true){
        int num = (rand7() - 1) * 7 + rand7() - 1; //返回[0, 48]
        if(num < 40)return num % 10 + 1;
    }
    return 0;
}
```

**变体：尝试用rand35\(\)实现rand42\(\) 不同于Leetcode题中的意思这里的rand35是说从\[0,34\]**

```cpp
while(true){
    num = (rand() % 35) * 35 + rand() % 35; 
    if(num <= 1217)return num % 42; //核心在于拒绝采样 
    //42 * 29 = 1218 < 35 * 35
}
```



