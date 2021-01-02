# 题目

### 题目

|  |  |  |  |
| :--- | :--- | :--- | :--- |
| 130 | 被围绕的区域 |  |  |
| 399 | 除法求值 | 能够想到并查集 | 好题 |

**130. 被围绕的区域**

**传统方法：**先用 for 循环遍历棋盘的**四边**，用 DFS 算法把那些与边界相连的 `O` 换成一个特殊字符，比如 `#`；然后再遍历整个棋盘，把剩下的 `O` 换成 `X`，把 `#` 恢复成 `O`。这样就能完成题目的要求，时间复杂度 O\(MN\)

**Union-Find 算法解决：**

 **把那些不需要被替换的 `O` 连成一个共同祖先 `dummy`上，这些 `O` 和 `dummy` 互相连通，而那些需要被替换的 `O` 与 `dummy` 不连通**。

![](../../.gitbook/assets/3.jpg)

**399. 除法求值**

```text
输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
给定：a / b = 2.0, b / c = 3.0
问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
返回：[6.0, 0.5, -1.0, 1.0, -1.0 ]
```

**解决：并查集**

```cpp
class Solution {
public:
    unordered_map<string, string> parents;
    unordered_map<string, double>val;
    string find(string a){
        if(parents[a]!=a){
            string temp = find(parents[a]);
            val[a] = val[a] * val[parents[a]];// a->c  =  a->b  *  b->c 
            parents[a] = temp;// a->c 连接
        }
        return parents[a]; 
    }

    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        //initialize
        for(int i = 0; i < equations.size(); i++){
            parents[equations[i][0]] = equations[i][0];
            val[equations[i][0]] = 1.0;
            parents[equations[i][1]] = equations[i][1];
            val[equations[i][1]] = 1.0;
        }
        //update
        for(int i = 0; i < equations.size(); i++){
            string root_a = find(equations[i][0]);   
            parents[root_a] = equations[i][1];   
            val[root_a] = values[i] / val[equations[i][0]];
        }
        //query
        vector<double>res;
        for(int i = 0; i < queries.size(); i++){
            if(parents.count(queries[i][0])==0||parents.count(queries[i][1])==0
                ||find(queries[i][0])!=find(queries[i][1])){
                res.push_back(-1.0);
            }else{
                res.push_back(val[queries[i][0]] / val[queries[i][1]]);
            }
        }
        return res;
    }
};
```

