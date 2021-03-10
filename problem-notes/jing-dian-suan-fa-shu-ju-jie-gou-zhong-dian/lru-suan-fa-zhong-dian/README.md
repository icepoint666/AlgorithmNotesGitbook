# LRU算法\(双向链表实现）

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

* **哈希表要存有双向链表的地址**
* **双向链表要存有key,value**
* **（互相存有找到对方的方式，以便删除时候的需要）**

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
struct DListNode{
    int key, val;
    DListNode* prev;
    DListNode* next;
    DListNode():key(0), val(0), prev(nullptr), next(nullptr){};
    DListNode(int key_, int val_):key(key_),val(val_), prev(nullptr), next(nullptr){};
};
class LRUCache {
private:
    int capacity;
    int size;
    DListNode* head;
    DListNode* tail;
    unordered_map<int, DListNode*>mp;
public:
    LRUCache(int capacity) {
        this->capacity = capacity;
        this->size = 0;
        mp.clear();
        head = new DListNode();
        tail = new DListNode();
        head->next = tail;
        tail->prev = head;
    }
    
    int get(int key) {
        if(mp.count(key) > 0){
            DListNode* node = mp[key];
            int value = node->val;
            moveToHead(node);
            return value;
        }
        return -1;
    }
    
    void put(int key, int value) {
        DListNode* node;
        if(mp.count(key) > 0){
            node = mp[key];
            node->val = value;
            moveToHead(node);
            return;
        }
        if(size == capacity){
            DListNode* tmp = removeTail();
            mp.erase(tmp->key);
            delete(tmp);
            node = new DListNode(key, value);
            addToHead(node);
            mp[key] = node;
        }else{
            node = new DListNode(key, value);
            addToHead(node);
            mp[key] = node;
            size++;
        }
    }

    void moveToHead(DListNode* node){
        removeNode(node);
        addToHead(node);
    }

    DListNode* removeTail(){
        DListNode* node = tail->prev;
        removeNode(node);
        return node;
    }
    void addToHead(DListNode* node){
        node->next = head->next;
        head->next->prev = node;
        head->next = node;
        node->prev = head;
    }
    void removeNode(DListNode* node){
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }
};
```

