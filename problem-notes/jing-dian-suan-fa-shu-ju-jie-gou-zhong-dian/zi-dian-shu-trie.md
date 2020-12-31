# 字典树\(Trie\)

查单词需要所以叫字典树

**问题场景：**

字典中有n个单词，存在STL的map里，现在有m个查询，每次查询一个单词，需要回答出这个单词是否在之前输入的n单词中出现过

* 暴力实现：
* STL中map/set实现：红黑树
* STL中unordered\_map/unordered\_set实现：hash\_table
* 针对于字符：更高效的数据结构解决这个问题：Trie树

对cat、cash、apple、aply、ok建立一颗字典树

![](../../.gitbook/assets/v2-5e40295bfa6c4a1688b6b5888aef583d_b.jpg)

* 在这篇文章中，我们主要讨论小写的英文字母查询，因此每个节点最多有26个子节点
* 整棵树的根节点是空的（这里我们设置根节点为root=0），这便于查找和插入
* 每个节点结束的时候用一个特殊的标记来表示，这里我们用-1来表示结束

**初始化**

```cpp
vector<vector<int>>trie;  //表示trie树的数组
vector<int>leaf;          //是否为根节点
vector<int>sum;           //统计节点出现次数
int num;

/** Initialize your data structure here. */
Trie() {
    num = 0;
    trie.push_back(vector<int>(26, 0));
    leaf.push_back(false);
}
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        int root = 0;
        for(auto&c: word){
            char id = c - 'a';
            if(!trie_[root][id]){
                trie_[root][id] = ++num;
                trie_.push_back(vector<int>(26, 0));
                leaf.push_back(false);
            }
            root = trie_[root][id];
        }
        leaf[root] = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        int root = 0;
        for(auto&c: word){
            char id = c - 'a';
            if(!trie_[root][id])
                return false;
            root = trie_[root][id];
        }
        if(leaf[root])return true;
        return false;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        int root = 0;
        for(auto&c: prefix){
            char id = c - 'a';
            if(!trie_[root][id])
                return false;
            root = trie_[root][id];
        }
        return true;
    }
```

**插入**

申请数组：root\[NUM\_NODE\]\[26\] \(26表示只考虑小写字母）

cnt初始为0，表示节点ID，root是0

```cpp
void insert(string word) {
    int root = 0;
    for(auto&c: word){
        char id = c - 'a';
        if(!trie[root][id]){
            trie[root][id] = ++num;
            trie.push_back(vector<int>(26, 0));
            leaf.push_back(false);
            sum.push_back(0);
        }
        sum[trie_[root][id]]++;
        root = trie_[root][id];
    }
    leaf[root] = true;
}
```

**查找**

* 可以查找一个前缀
* 可以查找整个单词
* 可以统计一个前缀在单词表中出现的次数。

**从左往右扫描前缀单词中的每一个字母，然后从字典树的第一层开始找，能找到第一个字母就顺着字典树往下走，否则结束查找，即没有此前缀；若前缀单词扫完了到-1了，表示有这个前缀。**

```cpp
bool search(string word) {
    int root = 0;
    for(auto&c: word){
        char id = c - 'a';
        if(!trie[root][id])
            return false;
        root = trie[root][id];
    }
    if(leaf[root])return true;
    return false;
}
```

**删除**

```cpp
void delete(string word){ //假定必然存在+删除成功
    int root = 0;
    for(auto&c: word){ //s[i] = '\0'结束
        int id = c -'a';
        sum[trie[root][id]]--;
        root = trie[root][id];
        if(sum[trie[root][id]] == 0)
            trie[root][id]=-1;//这种实现会造成节点空洞
    }
}
```

