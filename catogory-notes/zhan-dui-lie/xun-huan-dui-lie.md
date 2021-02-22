# 循环队列

Leetcode 622实现循环队列

循环队列有用过吗

循环队列的长度如何计算的

**关键：记录head，count,  capacity**

```cpp
class MyCircularQueue {
public:
    vector<int>cirque;
    int head, count, capacity;
    MyCircularQueue(int k) {
        cirque.resize(k);
        head = 0;
        count = 0;
        capacity = k;
    }
    
    bool enQueue(int value) {
        if(count == capacity)return false;
        cirque[(head + count) % capacity] = value; 
        count++;
        return true;
    }
    
    bool deQueue() {
        if(!count)return false;
        count--;
        head = (head + 1) % capacity;
        return true;
    }
    
    int Front() {
        if(!count)return -1;
        return cirque[head];
    }
    
    int Rear() {
        if(!count)return -1;
        return cirque[(head + count - 1) % capacity];
    }
    
    bool isEmpty() {
        return count == 0;
    }
    
    bool isFull() {
        return count == capacity;
    }
};
```

