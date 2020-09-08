# LRU算法（重点）

**Leetcode146 LRU算法实现**

**实现规范**

它应该支持以下操作： 

* 获取数据 get
* 写入数据 put 。

①获取数据 get\(key\) - 如果关键字 \(key\) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。

②写入数据 put\(key, value\) - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

要求获取数据与写入数据操作都要是O\(1\)复杂度



**做法**

通过**哈希表 + 双向链表**实现（把过程拆分成函数，清晰明了）

注意1：一般面试官可能会要求手动实现一个双向链表，最好不要使用已有的算法模板

哈希表存储key以及双向链表中数据的指针

注意2：新加入的元素应该add到头部

注意3：双向链表里面一定要存储key，因为之后从tail中删除的时候也要删除hashmap中的元素，要利用这个key去找到hashmap中的元素

注意4：自己new出来的数据结构，要记得delete，因为自己实现的双向链表不是智能指针不能自己删除

注意5：removeTail的时候，不是把tail移除了，其实是吧tail-&gt;prev的节点用removeNode同样的方法移除了，因为tail只是一个尾部标记，不同于head，tail是没有存储实际节点的，只是一个空的DLinkedNode

注意6：从removeTail返回所移出的节点，因为要正式从cache中删除了，所以要把内存delete掉

**代码\(需要背下来的模板\):**

注意模仿**双向链表的实现**，注意哪些要写在private，哪些要写在public，注意要区分开写，面试的时候代码规范其实也很重要

要记录一下LRU cache的size与 capacity

```cpp
struct DLinkedNode {
    int key, value;
    DLinkedNode* prev;
    DLinkedNode* next;
    DLinkedNode(): key(0), value(0), prev(nullptr), next(nullptr) {}
    DLinkedNode(int _key, int _value): key(_key), value(_value), prev(nullptr), next(nullptr) {}
};

class LRUCache {
private:
    unordered_map<int, DLinkedNode*> cache;
    DLinkedNode* head;
    DLinkedNode* tail;
    int size;
    int capacity;

public:
    LRUCache(int _capacity): capacity(_capacity), size(0) {
        // 使用伪头部和伪尾部节点
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head->next = tail;
        tail->prev = head;
    }
    
    int get(int key) {
        if (!cache.count(key)) {
            return -1;
        }
        // 如果 key 存在，先通过哈希表定位，再移到头部
        DLinkedNode* node = cache[key];
        moveToHead(node);
        return node->value;
    }
    
    void put(int key, int value) {
        if (!cache.count(key)) {
            // 如果 key 不存在，创建一个新的节点
            DLinkedNode* node = new DLinkedNode(key, value);
            // 添加进哈希表
            cache[key] = node;
            // 添加至双向链表的头部
            addToHead(node);
            ++size;
            if (size > capacity) {
                // 如果超出容量，删除双向链表的尾部节点
                DLinkedNode* removed = removeTail();
                // 删除哈希表中对应的项
                cache.erase(removed->key);
                // 防止内存泄漏
                delete removed;
                --size;
            }
        }
        else {
            // 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
            DLinkedNode* node = cache[key];
            node->value = value;
            moveToHead(node);
        }
    }

    void addToHead(DLinkedNode* node) {
        node->prev = head;
        node->next = head->next;
        head->next->prev = node;
        head->next = node;
    }
    
    void removeNode(DLinkedNode* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void moveToHead(DLinkedNode* node) {
        removeNode(node);
        addToHead(node);
    }

    DLinkedNode* removeTail() {
        DLinkedNode* node = tail->prev;
        removeNode(node);
        return node;
    }
};
```

