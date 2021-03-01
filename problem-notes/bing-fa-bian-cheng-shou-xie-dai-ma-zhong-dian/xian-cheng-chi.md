# 线程池

主要就是两个文件

threadpool.h文件

以及测试用的文件

### 线程池（3个函数，5个对象，其中一个函数涉及到泛型编程）

重点记住enqueue的泛型编程的实现

**函数**

* 构造函数 threadpool\(num\_threads） 创造线程
  * emplace\_back
  * 包含每个线程的run函数，用Lambda来写
    * RAII锁控制 + 条件变量
    * 判定stop
* 将任务加入到工作队列 enqueue
  * `std::make_shared` `std::package_task`  `std::bind`
  * RAII锁控制，判定stop，queue.emplace
  * 返回的是一个std::future
* 析构函数
  * 加锁设置flag
  * notify\_all
  * 回收每个线程 join
* （另外可以写四个单例类的函数定义=delete）

 **对象**

* 线程池停用flag: stop
* 工作线程
* 任务队列
* 互斥锁
* 条件变量

```cpp
#ifndef THREADPOOL_H
#define THREADPOOL_H

#include <vector>
#include <queue>
#include <functional>
#include <thread>
#include <memory>
#include <mutex>
#include <future>
#include <condition_variable>
#include <stdexcept>


class ThreadPool{ //3个函数（构造里面一个内置函数） 5个成员
public:
    ThreadPool(size_t num_thread);
    template<typename F, typename... Args>
    auto enqueue(F&& f, Args&&... args)->std::future<decltype(f(args...))>;
    ~ThreadPool();
private:
    ThreadPool(ThreadPool&&) = delete;
    ThreadPool& operator = (ThreadPool&&) = delete;
    ThreadPool(const ThreadPool&) = delete;
    ThreadPool& operator = (const ThreadPool&) = delete;
private:
    std::vector<std::thread> workers;        //工作线程
    std::queue<std::function<void()>> tasks; //任务队列

    std::mutex que_mtx;   
    std::condition_variable cond;
    bool stop;                               //主线程stop，线程池析构的时候，用来告知其他线程该停止了
};

ThreadPool::ThreadPool(size_t threads){
    for(size_t i = 0; i < threads; i++){
        workers.emplace_back(//in-place 不会拷贝复制
            [this]{
                for(;;){
                    std::function<void()>task;
                    {//RAII 自动锁会释放
                        std::unique_lock<std::mutex> lock(this->que_mtx);
                        this->cond.wait(lock, [this]{return this->stop || !this->tasks.empty();});
                        if(this->stop && this->tasks.empty())return;
                        task = std::move(this->tasks.front());
                        this->tasks.pop();
                    }
                    task();
                }
            }
        );
    }
}

template<typename F, typename... Args>
auto ThreadPool::enqueue(F&& f, Args&&...args) -> std::future<decltype(f(args...))>{
    auto taskPtr = std::make_shared<std::packaged_task<decltype(f(args...))()>>(
        std::bind(std::forward<F>(f), std::forward<Args>(args)...)
    );
    {
        std::unique_lock<std::mutex> ul(this->que_mtx);
        if(this->stop) { throw std::runtime_error("ThreadPool stopped"); }
        this->tasks.emplace([taskPtr]() { (*taskPtr)(); }); //emplace == push
    }
    this->cond.notify_one();
    return taskPtr->get_future();
}



ThreadPool::~ThreadPool(){
    {
        std::unique_lock<std::mutex>lock(this->que_mtx);
        this->stop = true;
    }
    this->cond.notify_all();
    for(auto &worker: workers)
        worker.join();
}

#endif
```

### 测试

* 构建线程池
* std::future结果数组
* 将任务都加入到线程池
* 输出结果

```cpp
#include "threadpool.h"
#include <vector>
#include <future>
#include <iostream>

int main(){
    ThreadPool pool(4);
    std::vector<std::future<int>> results;
    for(int i = 0; i < 8; ++i){
        results.emplace_back(pool.enqueue([i]{return i * i;}));
    }
    for(auto && result: results){
        std::cout << result.get() << std::endl;
    }
}
```

