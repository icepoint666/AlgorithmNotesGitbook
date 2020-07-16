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

### set, unordered\_set

链表中被逼无奈没有好的办法，采用unordered\_set把ListNode\*存放在set中可以记录是否访问

构造

```cpp
using namespace std;

unordered_set<ListNode*> c; //初始化容器
unordered_set<ListNode*> c{ "aaa", "bbb", "ccc" }; //初始化容器，并将"aaa", "bbb", "ccc"加入到容器中
```

添加元素

```cpp
c.insert(e);
```

查找元素

```cpp
c.find("eeee"); 
//查找元素"eeee"，返回结果为a.end()则表明没有找到，否则返回所对应元素
c.count("eeee"); 
//查找元素"eeee"在a中有几个（由于unordered_set中没有相同的元素，所以结果通常为0或1）
```

清除元素

```cpp
a.clear(); //清除a中所有元素
a.erase("aaa"); //清除元素"aaa"
```

容器大小

```cpp
a.size(); //返回元素个数
```

