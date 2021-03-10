# STL list实现LRU

把moveToHead, addToHead, removeTail，getTail这些操作都通过STL来代替了

* x.splice\(x.begin\(\), x, it\) == moveToHead;
* x.push\_front\(\) == addToHead
* x.pop\_back\(\) == removeTail
* x.back\(\) == getTail

```cpp
vector<int> LRU(vector<vector<int> >& operators, int k) {
    unordered_map<int, list<pair<int,int>>::iterator>mp;
    list<pair<int,int>>lst;
    vector<int>res;
    int capacity = k;
    int size = 0;
    for(auto&vec: operators){
        if(vec[0] == 1){
            if(size < capacity){
                lst.push_front({vec[1], vec[2]});
                auto it = lst.begin();
                mp[vec[1]] = it;
                size++;
            }else{
                if(mp.count(vec[1]) > 0){
                    auto it = mp[vec[1]];
                    it->second = vec[2];
                    lst.splice(lst.begin(), lst, it);
                }else{
                    int key = lst.back().first;
                    mp.erase(key);
                    lst.pop_back();
                    lst.push_front({vec[1], vec[2]});
                    auto it = lst.begin();
                    mp[vec[1]] = it;
                }
            }
        }else{
            if(mp.count(vec[1]) == 0)
                res.push_back(-1);
            else{
                res.push_back(mp[vec[1]]->second);
                lst.splice(lst.begin(), lst, mp[vec[1]]);
            }
        }
    }
    return res;
}
```

