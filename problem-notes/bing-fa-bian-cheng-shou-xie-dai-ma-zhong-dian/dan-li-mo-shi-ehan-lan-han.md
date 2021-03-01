# 单例模式（饿汉，懒汉）

### 饿汉模式

创建一个全局静态变量就好了

### 懒汉模式

#### 实现1：c++11 静态成员函数静态局部变量

```cpp
#include <iostream>

class Singleton{
public:
    static Singleton& getInstance(){ 
        static Singleton value; //这里的类型也可以替换成模板T typename T
        return value;
    }
private:
    Singleton() = default;
    Singleton(const Singleton&) = delete;
    Singleton(const Singleton&&) = delete;
    Singleton operator = (const Singleton&) = delete;
    Singleton operator = (const Singleton&&) = delete;
};

int main(){
    Singleton& s1 = Singleton::getInstance();
    Singleton& s2 = Singleton::getInstance();
    std::cout << &s1 << " " << &s2 << std::endl;
    return 0;
}
```

**实现2：双检查锁**

```cpp
#include <mutex>
#include <iostream>

template<typename T>
class SingletonT{
public:
    static T& getInstance(){
        static T* instance;
        static std::mutex m;
        if(!instance){
            std::lock_guard<std::mutex> lk(m);
            if(!instance){
                instance = new T();
            }
        }
        return *instance;
    }
private:
    SingletonT() = default;
    SingletonT(const SingletonT&) = delete;
    SingletonT(const SingletonT&&) = delete;
    SingletonT& operator = (const SingletonT&&) = delete;
    SingletonT& operator = (const SingletonT&) = delete;
};

int main(){
    int& s3 = SingletonT<int>::getInstance();
    int& s4 = SingletonT<int>::getInstance();
    std::cout << &s3 << " " << &s4 << std::endl;
    return 0;
}
```

