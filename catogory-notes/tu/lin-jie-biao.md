# 邻接表

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 127 | 单词接龙 |  建立邻接表 + bfs | 困难 |

**127. 单词接龙**

输入：beginWord = "hit", endWord = "cog", wordList = \["hot","dot","dog","lot","log","cog"\] 输出：5 解释：一个最短转换序列是 "hit" -&gt; "hot" -&gt; "dot" -&gt; "dog" -&gt; "cog", 返回它的长度 5。

**关键：**

* **不要建立string的邻接表，更不要邻接矩阵，开销太高了**
* **给string加一个int索引，建立索引的邻接表**
* 如果距离diff = 1，认为可以建立一个edge

```cpp
bool diff(string& a, string& b){
    int n = a.size();
    int m = b.size();
    if(n != m)return false;
    int i = 0, cnt = 0;
    while(i < n){
        if(a[i] != b[i])cnt++;
        i++;
    }
    return cnt == 1;
}
int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
    unordered_map<string, int>word2idx; //关键
    vector<bool>vis;
    vector<vector<int>>conn;
    word2idx[beginWord] = 0;
    vis.push_back(true);
    conn.resize(wordList.size() + 1, vector<int>());
    for(int i = 0; i < wordList.size(); i++){
        word2idx[wordList[i]] = i+1;
        vis.push_back(false);
        if(diff(beginWord, wordList[i]))conn[0].push_back(i+1);
    }
    if(!word2idx.count(endWord))
        return 0;
    for(int i = 0; i < wordList.size(); i++){
        for(int j = i + 1; j < wordList.size(); j++){
            if(diff(wordList[i], wordList[j])){
                conn[i+1].push_back(j+1);
                conn[j+1].push_back(i+1);
            }
        }
    }
    queue<pair<int, int>>q;
    q.push({0, 1});
    while(!q.empty()){
        int cur = q.front().first;
        int dep = q.front().second;
        q.pop();
        if(word2idx[endWord] == cur)
            return dep;
        for(auto idx: conn[cur]){
            if(!vis[idx]){
                q.push({idx, dep+1});
                vis[idx] = true;
            }
        }
    }
    return 0;
}
```

