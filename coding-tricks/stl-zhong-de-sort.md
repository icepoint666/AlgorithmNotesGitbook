# STL中的sort\(\), pair,tuple,string的默认比较规则

### 模板

```cpp
template <class RandomAccessIterator, class Compare>
  void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);
```

**使用方式:**

```cpp
sort(vec.begin(), vec.end());             //内置比较方式
sort(vec.begin(), vec.end(), Compare());
sort(vec.begin(), vec.end(), greater<int>{});
```

### **默认sort的比较方式：（每个容器都有内置重载的 operater &lt;\)**

**每个容器内置的operator &lt; 规则** 

**string:**  

bool operator &lt;  return std::string& lhs &lt; std::string& rhs

* 小于 lhs的字母比rhs字母小，或者lhs与rhs匹配，但是lhs比rhs更短（也就是字典序更小）
* 大于 lhs的字母比rhs字母大，或者lhs与rhs匹配，但是lhs比rhs更长（也就是字典序更大）
* 等于 两个字符串相等

**pair:**

bool operator &lt;  return std::pair& lhs &lt; std::pair& rhs

* 小于 lhs.first &lt; rhs.first，如果lhs.first == rhs.first则比较lhs.second &lt; rhs.second 

### 自定义sort的比较方式：定义Lambda函数，定义比较类

一般STL容器最好不要直接重载operator &lt; ，可能会报错

方法1：

```cpp
auto cmp = [&](const auto &a, const auto &b){
    return a[1] < b[1];
};
sort(points.begin(), points.end(), cmp);
```

方法2：

```cpp
struct CmpByValue {
  bool operator()(const map_pair& lhs, const map_pair& rhs) {
    return lhs.second < rhs.second;
  }
};
sort(vec.begin(), vec.end(), CmpByValue());
```

