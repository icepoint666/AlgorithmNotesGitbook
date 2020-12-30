# trie树相关

trie树复习可以参考：经典算法数据结构：trie树

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 139 | 单词拆分 | trie树（高频题） | 中等，复杂 |

**139.单词拆分**

**题意：**

 判定 _s_ 是否可以被空格拆分为一个或多个在字典中出现的单词

```text
s = "leetcode", wordDict = ["leet", "code"]
输出: true
```

**题解：**

1.建立trie树：

* trie树的size刚开始设置为\[400\]\[26\],\[4000\]\[26\]都会报错，设置为\[40000\]\[26\]通过
* 需要设置一个叶子节点标记数组leaf\[40000\]
  * 当前node节点如果是根节点，node = leaf\[node\]\[ch\],将新node leaf\[node\]设置为true

2.如果一个单词查出来之后，有两个选择：

* ①从root开始识别
* ②接着单词继续识别看看有没有更长的单词

所以说有多个分支，在trie树中，所以要**存储当前查找的trie节点**

起初是准备用set存储，但是根据特性维护操作只有两个：

* ①erase元素，且不改变迭代器（list适合）it = lst.erase\(it\)
* ②加入一个元素在其中，lst.push\_front\(\)

显然选用list比较适合

```cpp
bool wordBreak(string s, vector<string>& wordDict) {
    int trie[40000][26];
    bool leaf[40000];
    memset(leaf, false, sizeof(leaf));
    memset(trie, false, sizeof(trie));
    int cnt = 0;
    for(auto&word: wordDict){
        int root = 0;
        for(int i = 0; i < word.size(); i++){
            int ch = word[i] - 'a';
            if(!trie[root][ch])trie[root][ch] = ++cnt;
            root = trie[root][ch];
        }
        leaf[root] = true;
    }
    list<int>lst;
    lst.push_front(0);
    int i = 0;
    while(i < s.size()){
        bool flag = false;
        for(auto it=lst.begin(); it!=lst.end();){
            if(trie[*it][s[i]-'a']){
                *it = trie[*it][s[i]-'a'];
                if(leaf[*it])flag = true;
                ++it;
            }else{
                it = lst.erase(it);
            }
        }
        if(flag)lst.push_front(0);
        i++;
    }
    for(auto it=lst.begin(); it!=lst.end(); it++){
        if(leaf[*it])return true;
    }
    return false;
}
```

