# std::string基本操作

初始化

```cpp
string s("aaaa");
string s(10,'a');
string s = "ssss";
```

切片：substr\(start\_pos, length\)

```cpp
string a = "hello"
string s = a.substr(3,2); // "lo"
```

添加字符串或者元素

```cpp
string s1 = "aaa";
string s2 = "bbb";
string s = s1 + s2;
//append可以整个字符串添加的
s.append("aaa");
s.append("a");
s.insert(0, "aaa");
//push_back必须是一个一个字符添加的
s.push_back('h')
s.push_back('e')
s.push_back('l')
s.push_back('l')
s.push_back('o')
```

**注意：string字符串处理添加操作的时候，不要使用"+"，使用push\_back和append**

**再注意！！有时leetcode使用append会报莫名其妙的错误，'+'就不会**

```cpp
pointer index expression with base 0xbebebebebebebebe overflowed to 0x7d7d7d7d7d7d7d7c (basic_string.h)
```

**push\_back与append的区别**

* push\_back一个一个字符的添加
* append是整个字符串添加

**find函数查找子串：**

```cpp
a.find(b, 3) # 表示从字符串的index = 3处开始查找，找为b的子串
```

