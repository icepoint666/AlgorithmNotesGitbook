# 笔试输入输出相关注意事项

string字符串如果输入中包含空格，那么要这样输入**（二进制安全字符串输入）**

```cpp
string s;
getline(cin, s);
```

C++循环输入数组

```cpp
int arr[10000];
int i = 0;
while(cin >> arr[i]){
    i++;
    if(cin.get() == '\n')break;
}
```

