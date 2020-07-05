# std::string转int,float,double

### 利用stringstream可以完成向任何类型的转换

```cpp
#include <string> //std::string
#include <sstream> //stringstream

...

int stringToInt(const string &s) {
    int v;
    stringstream ss;
    ss << s;
    ss >> v;
    return v;
}
double stringToDouble(const string &s){
    double v;
    stringstream ss;
    ss << s;
    ss >> v;
    return v;
}
...
int main() {
    int i = stringToInt("2.3");
    cout<<i<<endl;
}
```

### C风格：利用atoi与strtol

```cpp
#include <cstdlib> //atoi,strtoi

string str = "16s";
int a = atoi(str.c_str());
int b = strtol(str.c_str(), nullptr, 10);
```

### stoi

```cpp
#include <string> //stoi

string str1 = "asq,";
//    int c = stoi(str1);    // 报异常
string str2 = "12312";
int c = stoi(str2);     // ok
```

