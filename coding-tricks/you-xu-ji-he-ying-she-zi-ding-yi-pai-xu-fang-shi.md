# STL map/set排序，自定义排序方式

### **STL中Map的按Key排序**

```cpp
map<string, int, greater<string> > name_score_map; //从z到A从大到小

```

### **STL中Map自定义key的排序**

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



