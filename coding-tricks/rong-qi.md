# 容器

### vector

初始化

```cpp
vector<int> vec;
```

添加元素

```cpp
vec.push_back(e);
```

### map,unordered\_map

初始化

```cpp
map<char,int> mp;
unordered_map<char,int> mp; //哈希表容器：
```

遍历容器、遍历map：

```cpp
for(auto it=mp.begin();it!=end();it++){
    it->first it->second
}
```

map找寻元素是否存在：

```cpp
if(mp.count(key)>0)
```

map添加元素

```cpp
mp[key] = val;
```

**一个骚操作**

如果用map充当一个计数器，刚开始添加的时候直接`mp[key]++`; 就默认加入值为1了，不需要复杂的验证是否存在然后从0开始加。

```cpp
unordered_map<char, int> needs;
for (char c : t) needs[c]++;
```

map删除元素

```cpp
mp.erase(key); //需要先前判断key存在
```

