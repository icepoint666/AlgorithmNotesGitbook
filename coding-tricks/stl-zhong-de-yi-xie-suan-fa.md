# STL中的一些算法

> \#include &lt;algorithm&gt;

### find, remove, remove\_if（虽然会把元素移到末尾，但是会打乱相对顺序）

```cpp
v.erase(remove(v.begin(), v.end(), 5), v.end());
v.erase(remove_if(v.begin(), v.end(), [](int i){i>5;}), v.end());
```

### stable\_partition（可以稳定的Partition两类条件的元素）

```cpp
stable_partition(s.begin(),s.end(),[](char c){return c >= 'a'));

//可以把小写字母分到左边，大写字母分到右边，而且不打乱原来的相对顺序：
//aaaabbCCCAA
```

