# 多叉树NestedInteger问题

**概念：本质就是多叉树**

```text
[[1,1],2,[1,1]]

       2
  /   / \  \ 
 1   1   1   1 
```

**数据结构**

```cpp
vector<NestedInteger>& nestedList

具备的操作:
NestedInteger ni;
isInteger()
getInteger()
ni.getList()  for (auto a : ni.getList())

/**
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
```

### **题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 339 | 嵌套链表权重和 | dfs | 简单 |
| 341 | 扁平化嵌套列表迭代器 | 有坑 | 中等 |

**339.嵌套链表权重和**

```text
Input: [[1,1],2,[1,1]]
Output: 10 
Explanation: 4个1的depth是2, 一个2的depth是1.
```

**dfs**

```cpp
class Solution {
public:
    int depthSum(vector<NestedInteger>& nestedList) {
        int res = 0;
        for (auto a : nestedList) {
            res += getSum(a, 1);
        }
        return res;
    }
    int getSum(NestedInteger ni, int level) {
        int res = 0;
        if (ni.isInteger()) return level * ni.getInteger();
        for (auto a : ni.getList()) {
            res += getSum(a, level + 1);
        }
        return res;
    }
};
```

**341. 扁平化嵌套列表迭代器**

题意：实现一个NestedList的迭代器

```cpp
NestedIterator i(nestedList);
while (i.hasNext()) cout << i.next();
```

**栈**

naive思路：

* 用一个栈，每解开一个nestedlist，然后就把其中的元素倒着push到栈中
* 这样可以保证从前往后取的顺序
* next的时候直接取栈顶，并且取完pop
* hasNext的时候，看一下栈还有没有，如果有，因为存在test case\[ \[ \] \]，需要看一下有没有真实的int，如果是list需要解开并且重新push到栈中

这样做明显缺陷：如果用户重复的调用hasNext\(\)，并不去调用next，那么就会破坏栈的结果，造成错误

**最好的做法：用一个internel HasNext\(\)代替原来的hasNext\(\)**

* internel HasNext\(\)在最开始的时候，和每次next执行完后执行，返回一个flag保存到全局变量中
* hasNext\(\)只用返回这个flag就好了

```cpp
class NestedIterator {
private:
    stack<NestedInteger> st;
    bool hasnxt = false;
public:
  NestedIterator(vector<NestedInteger> &nestedList) {
      for (auto iter = nestedList.rbegin(); iter != nestedList.rend(); iter++) {
          st.push(*iter);
      }
      hasnxt = inHasNext();
  }
  int inHasNext(){
      while (!st.empty()) {
          auto cur = st.top();
          if (cur.isInteger()) return true;
          st.pop();
          auto curList = cur.getList();
          for (auto iter = curList.rbegin(); iter != curList.rend(); iter++) {
              st.push(*iter);
          }
      }
      return false;
  }
  int next() {
      auto t = st.top();
      st.pop();
      hasnxt = inHasNext();
      return t.getInteger();
  }

  bool hasNext() {
      return hasnxt;
  }
};
```

