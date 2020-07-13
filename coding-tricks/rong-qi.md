# 容器

哈希表容器：

```cpp
unordered_map<char, int> mp;
```

遍历容器、遍历map：

```cpp
map<char,int> mp;
...
for(auto it=mp.begin();it!=end();it++){
    it->first it->second
}
```

map找寻元素：

```cpp
if(mp.count(key)>0)
```

