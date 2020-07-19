# 哈希表中使用pair或者tuple作为key

如果想让自己的pair是hashable从而作为**unordered\_map**或者**unordered\_set**的key

这样实现:\(这里一定要operator\(\) const\)

```cpp
#include <utility>
#include <unordered_map>
#include <unordered_set>
#include <functional>
#include <pair>

template <class T, class U> 
struct PairHash{ 
    size_t operator()(const std::pair<T, U> &key) const{ 
        return std::hash<T>()(key.first) ^ std::hash<U>()(key.second);
    }
};

//使用：这里哈希表实现允许加入一个hash<key>的参数
unordered_map<std::pair<int, int>, int, PairHash<int,int> > mp;
unordered_set<std::pair<int, int>, PairHash<int,int> >s;
```

```cpp
template < class Key,                        // unordered_set::key_type/value_type
           class Hash = hash<Key>,           // unordered_set::hasher
           class Pred = equal_to<Key>,       // unordered_set::key_equal
           class Alloc = allocator<Key>      // unordered_set::allocator_type
           > class unordered_set;

template < class Key,                                    // unordered_map::key_type
           class T,                                      // unordered_map::mapped_type
           class Hash = hash<Key>,                       // unordered_map::hasher
           class Pred = equal_to<Key>,                   // unordered_map::key_equal
           class Alloc = allocator< pair<const Key,T> >  // unordered_map::allocator_type
           > class unordered_map;
```

