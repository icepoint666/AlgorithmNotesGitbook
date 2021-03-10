# 笔试输入输出相关注意事项

### 1.重复输入多组样例

```cpp
# input
1 5
10 20
# output
6
26

int a, b;
while(cin >> a >> b){
    cout << a + b << endl;
}

# input 
4 1 2 3 4
5 1 2 3 4 5
# output
10
15

while(cin >> num){
    int sum = 0, a;
    for(int i = 0; i < num; i++){
        cin >> a;
        sum += a;
    }
    cout << sum << endl;
}
```

### 2. 循环输入数组（每行不定有N个数 ，重点）

```cpp
# 单行单个数组
# input
1 2 3 4 5 6 7
# output

int arr[100];
int num = 0;
while(cin >> arr[i]){
    num++;
    if(cin.get() == '\n')break;
}

# 注意！！！ 上面这种方式处理单行单个数组就可以，下面这种方式处理单行就不行！
# 而且这种情况，是不能在结尾多输出一个空格的！记住！
int arr[100];
int num = 0;
while(cin.get() != '\n){
    cin >> arr[num++];
}//这种情况如果你连续输入8个数，num只会等于7

#多行多个数组
# input 
1 2 3
4 5
0 0 0 0 0
# output
6
9
0

int a[100];
int num = 0;  //初始化为零
while(cin >> a[num]){
    num++;    
    while(cin.get() != '\n'){
        cin >> a[num++];
    }
    
    //process
    
    num = 0;  //归零
}
```

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

