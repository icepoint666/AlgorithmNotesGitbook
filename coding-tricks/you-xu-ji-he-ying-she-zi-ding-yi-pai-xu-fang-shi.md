# 有序集合/映射，自定义排序方式

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

