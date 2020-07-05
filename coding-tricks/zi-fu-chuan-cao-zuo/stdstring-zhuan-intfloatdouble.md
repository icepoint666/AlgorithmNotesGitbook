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

