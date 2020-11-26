# 哈希表std::unordered\_map/std::unordered\_set

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 692 | 前K个高频单词 | 定义hashable + 自定义排序方式 | 教科书 |
| 1267 | 统计参与通信的服务器 | 用哈希表记录位置坐标\(i,j\)作为key | 中等 |

**692. 前K个高频单词（模板！！记忆！！）**

给一非空的单词列表，返回前 _k_ 个出现次数最多的单词

参照前面**哈希表hashtable key** 章节+ **STL map/set排序的自定义排序方式** 章节

**核心：**

* **如果map不是按照key排序的话，按照value排序就不需要std::map的有序性**
* **所以，std::unordered\_map的效率比std::map要好很多**
* **std::string需要hashable， map对应的pair需要自定义sort方式**

```cpp
class Solution {
public:
    typedef pair<string, int> map_pair;
    struct CmpByValue{ //排序规则
        bool operator () (const map_pair& lhs, const map_pair& rhs){
            if(lhs.second == rhs.second)return lhs.first < rhs.first;
            return lhs.second > rhs.second;
        }
    };
    struct pair_hash{ //hashable转换
        size_t operator() (const string& p) const{
            return hash<string>{}(p);
        }
    };
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<const string, int, pair_hash>mp;
        //如果使用map: map<const string, int>mp;
        for(auto&str: words){
            mp[str]++;
        }
        vector<map_pair> vec(mp.begin(), mp.end());
        sort(vec.begin(), vec.end(), CmpByValue());
        vector<string>res;
        res.reserve(k);
        for(int i = 0; i < k; ++i){
            res.push_back(vec[i].first);
        }
        return res;
    }
};
```

