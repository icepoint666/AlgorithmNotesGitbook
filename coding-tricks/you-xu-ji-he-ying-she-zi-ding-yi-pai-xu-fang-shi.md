# STL map/set排序，自定义排序方式

### **STL中Map的按Key排序**

```cpp
map<string, int, greater<string> > mp; //从大到小，从Z到A

map<string, int> mp; //从小到大
map<string, int, less<string> >mp; //等价于上面，从小到大

```

### **STL中Map自定义key的排序**

```cpp
struct CmpByKeyLength {
  bool operator()(const string& k1, const string& k2) {
    return k1.length() < k2.length();
  }
};
map<string, int, CmpByKeyLength> mp;
```

### STL中Set自定义key的排序（类似方式）

```cpp
class PairComp{
public:
    bool operator () (const pair<int,int> &a, const pair<int,int> &b){
        int dist_a = distance(a);
        int dist_b = distance(b);
        if(dist_a == dist_b)
            return a.first < b.first;
        return dist_a > dist_b;
    };
};

set<pair<int,int>, PairComp> s;
```

###  **STL中Map的按Value排序**

STL中的sort算法只能对**序列容器进行排序**（如vector，list，deque）

map也是一个**集合容器**，它里面存储的元素是pair，通过红黑树进行排序

**做法：把map中的元素放到序列容器（如vector）中，然后再对这些元素进行排序**

**map是以pair存储的，**我们需要知道pair重载的&lt;运算符是怎么样的：

pair类重载了&lt;符，但是它并不是按照value进行比较的，而是先对key进行比较，key相等时候才对value进行比较。显然不能满足我们按value进行排序的要求

