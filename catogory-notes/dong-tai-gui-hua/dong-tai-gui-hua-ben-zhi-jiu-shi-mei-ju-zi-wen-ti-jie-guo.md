# 动态规划本质就是枚举子问题结果

### 取扑克牌问题

  n张卡牌堆成一堆，每张卡牌上都有一个整数代表该牌得分。两人A,B交替从牌堆顶拿牌。第一次可以拿1~2张牌，后面每次拿牌，最多拿上个人拿牌数的两倍，最少拿1张，直到牌全部拿完。假设两个人都会采取最优策略让自己得分最大化，求先手拿牌人的得分。  
  
**解题：仔细思考一下这个过程**

枚举：第1个人,最开始取的最大利益 = max\(a\[0\] - 从第二个位置开始取的最大利益， a\[1\]-从第三个位置开始取的最大利益）

另外这个最大利益与刚取过的状态有关，默认第一次之前是取了1个，所以下一次可以取1-2个，如果下一次取了2个，那么再下一次就可以取1-4个

**①dp\[i\]\[j\]表示从第i个开始取，并可以取1-j个的最大收益, dp\[n+1\]\[n+1\]**

可以用sum数组的前缀和优化,sum\[i\]

②dp\[n\]\[i\] = 0

**③dp\[i\]\[j\] = max\(sum\[i+k\]-sum\[i\]-dp\[i+k\]\[min\(2\*k, n-i-k\)\], k = 0,1,2,...,n-i\)**

**求解：dp\[0\]\[2\]**

\*\*\*\*

