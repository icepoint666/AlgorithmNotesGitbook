# STL容器

这些标准库STL容器，在正常C++代码需要，这样使用

```cpp
#include <map>
#include <vector>
std::vector<int> vec;
std::map<int,int> mp;
```

### vector

初始化

```cpp
vector<int> vec;
vector<int> vec(1，2，3);
vector<int> vec = {1,2,3};
```

vector初始化一个长度为n的空数组, 之后可以直接下标访问

```cpp
vector <int> vec;
vec.resize(n);
for(int i = 0; i < n; i++){
   vec[i] = 1;
}
```

如果不想声明+初始化，想直接返回vec

```cpp
return vector<int>{0,0,0}; //直接返回一个vec对象
```

添加元素

```cpp
vec.push_back(e);
```

删除元素（删除指定数值的元素）

```cpp
vector<int> vec = {1,2,3,4,5};
//想删除数值为3的元素
for(auto it=vec.begin(); it!=vec.end(); ++it){
    if(*it == 3){
        it = vec.erase(it);//因为删除元素迭代器会失效，所以让它指向下一个元素
    }
}
//删完后会得到{1，2，4，5}
```

删除元素（删除指定位置的元素）

```cpp
vector<int> vec = {1,2,3,4,5};
//想删除vec[3]
for(auto it=vec.begin(); it!=vec.end(); ++it, ++i){
    if(i == 3){
        it = vec.erase(it);//因为删除元素迭代器会失效，所以让它指向下一个元素
    }
}
//删完后会得到{1，2，3，5}
```

### stack

```cpp
stack<int> s1;
s.push(5); //入栈
s.pop();  //出栈
s.top(); //返回栈顶
s.empty(); //判断是否为空
s.size(); //长度
```

### queue

```cpp
queue<int> q1;
q.push(5); //入队
q.pop(); //出队
q.front(); //队首
q.back(); //队尾
q.empty();
q.size();
```

### string

初始化

```cpp
string s("aaaa");
string s(10,'a');
string s = "ssss";
```

添加字符串或者元素

```cpp
string s1 = "aaa";
string s2 = "bbb";
string s = s1 + s2;
s.append("aaa");
s.append("a");
s.insert(0, "aaa");
```

### pair,tuple

c++标准库实现的Pair与Tuple都不是hashable，里面也允许放置动态vector，放置string，允许随时更改

```cpp
pair<int,int> pr = make_pair(2,3);
pair<int,int> pr(2,3);
pr.first pr.second
```

存放3个元素

```cpp
pair<int, pair<int,int> > pr;
```

tuple

```cpp
#include <tuple> //std::tuple std::get std::make_tuple

std::tuple<int&,string&, int&> tp;
std::tuple<int, char> tp(3, 'x');
std::tuple<int, int, int> tp1 = make_tuple(3,4,54);

cout << std::get<2>(tp1) << endl;
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

### unordered\_multimap（不常用）

关键在于**可以存储多个重复key的元素**

初始化

```cpp
unordered_multimap<int, int>mp;
unordered_multimap<int, int>mp = {{1,10},{2,20},{3,30}};
```

插入元素（不能通过map的方法，mp\[key\]这种方式插入）

```cpp
mp.insert({1，10});
```

删除元素

```cpp
mp.erase(key);
```

查找元素是否存在

```cpp
if(mp.count(key)>0)
```

查找元素内容（比较不太一样）

```cpp
pair<unordered_multimap<int, int>::iterator, unordered_multimap<int, int>::iterator> myRange;
myRange = mp.equal_range(key);
//或者
auto myRange = mp.equal_range(key);

for (auto it = myRange.first; it != myRange.second; ++it) {
		cout << it->first << " -> " << it->second << endl;
}
```

