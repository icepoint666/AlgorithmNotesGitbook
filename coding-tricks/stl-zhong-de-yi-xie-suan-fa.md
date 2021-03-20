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

### find（返回找到的第一个目标的idx位置坐标）

```cpp
str.find('0');
str.find('1', 2); //指定从2作为开始位置寻找

str.find('1', str.find('0')) //从出现零之后开始寻找第一个出现1的位置

//没找到
str.find('1') == -1
str.find('1') == string::npos  //表示没找到，也可以写成string::npos
//string::npos就表示坐标中的str.end(), str.end()是对应的迭代器类型
```



