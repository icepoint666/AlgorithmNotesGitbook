# 阻塞队列，无锁队列

### 阻塞队列（循环队列,multi-consumer, multi-producer）

设计都是整个队列被一个互斥锁所控制

* boundedqueue （循环队列，数组实现）
* blockqueue （双向链表list实现）

#### bounded\_queue循环队列（5个函数，8个成员）

5个函数

* 构造函数（初始化8个成员）
* 析构函数 （delete T\[\]\)
* push
* pop
* clear（可以不写这个）

8个成员对象

* 3个存量：beg, size, capacity（尽量用size\)
* 1个互斥锁
* 2个条件变量
* 1个队列数组

**学会RAII锁 + 条件变量**

**学会如何测试阻塞队列 std::thread配合std::ref来使用**

```cpp
#include <mutex>
#include <condition_variable>
#include <iostream>

template<typename T>
class BoundedQueue{
public:
    BoundedQueue(int capacity);
    ~BoundedQueue();
    void Push(T&);
    T Pop();
    void Clear();
private:
    bool clear;
    int beg;
    int size;
    int capacity;
    std::mutex mtx;
    std::condition_variable cond_full;
    std::condition_variable cond_empty;
    T* queue;
};

template<typename T>
BoundedQueue<T>::BoundedQueue(int capacity_){
    clear = false;
    beg = 0;
    size = 0;
    capacity = capacity_;
	queue = new T[capacity];
}

template<typename T>
BoundedQueue<T>::~BoundedQueue(){
	delete [] queue;
}

template<typename T>
void BoundedQueue<T>::Clear(){
    {
	    std::unique_lock<std::mutex> lk(mtx);
        if(!clear){
            clear = true;
            cond_empty.notify_all();
        }
    }
}

template<typename T>
void BoundedQueue<T>::Push(T& t){
    {
        std::unique_lock<std::mutex> lk(mtx);
        cond_full.wait(lk, [this]{return this->size != this->capacity;});
        queue[(beg+size)%capacity] = t;
        ++size;
    }   
    cond_empty.notify_one();
}

template<typename T>
T BoundedQueue<T>::Pop(){
    T t = T();
    {
        std::unique_lock<std::mutex> lk(mtx);
        cond_empty.wait(lk, [this]{return this->size != 0 || this->clear;});
        if(clear)return t;
        t = std::move(queue[beg]);
        if(++beg == capacity)beg = 0;
        --size;
    }
    cond_full.notify_one();
    return t;
}

#include <thread>
#include <vector>
#include <chrono>
#include <functional> //std::ref

void produce(BoundedQueue<int>& queue){
    for(int i = 0; i < 9; i++){
        queue.Push(i);
    }
}

void consume(BoundedQueue<int>& queue){
    for(;;){
        int tmp = queue.Pop();
        std::cout << tmp << std::endl;
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }
}

int main(){
    BoundedQueue<int> q(2);
    std::vector<std::thread> threads;
    for(int i = 0; i < 3; i++){
        threads.emplace_back(produce, std::ref(q)); 
        //本质是一个std::bind(std::functional<void()>, Args...args),所以第二个参数一定要用std::ref
    }
    std::thread t(consume, std::ref(q));
    t.join();
    for(int i = 0; i < 3; i++){
        threads[i].join();
    }
}
```

`$ g++ boundedqueue.cpp -o bounded_queue -l pthread`

### 无锁队列\(multi-producer, multi-consumer\)

采用的是moodycamel的实现方法：[https://github.com/cameron314/concurrentqueue](https://github.com/cameron314/concurrentqueue)

**这个做法实现的原理是什么？**

\*\*\*\*



\*\*\*\*

