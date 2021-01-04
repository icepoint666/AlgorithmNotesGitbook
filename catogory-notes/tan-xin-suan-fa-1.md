# 贪心算法

贪心算法可以认为是动态规划算法的一个特例

相比动态规划，使用贪心算法需要满足更多的条件

贪心选择性质呢，简单说就是：**每一步都做出一个局部最优的选择，最终的结果就是全局最优。**注意哦，这是一种特殊性质，其实只有一部分问题拥有这个性质。

（虽然贪心，但也能全局最优）

**类似题目重叠子区间问题：**

#### 题目

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 435 | 无重叠区间 | 贪心 | 简单 |
| 452 | 用最少数量的箭引爆气球 | 贪心 | 简单 |
| 56 | 合并区间 | 重载排序函数 | 简单 |
| 621 | 任务调度器 | 模拟O\(N\*M\) | 自己做超时了 |
|  |  | 桶思想 | 技巧 \(推荐\) |
| 406 | 根据身高重建队列 | 重载排序函数 | 中等 |

**322. 零钱兑换**

 计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

**示例 ：**

```text
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```



**452. 用最少数量的箭引爆气球**

**经典贪心**

```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        int n = points.size();
        if(n == 0)return 0;
        auto cmp = [&](const auto &a, const auto &b){
            return a[1] < b[1];
        };
        sort(points.begin(), points.end(), cmp);
        int count = 1;
        int bd = points[0][1];
        for(int i = 0; i < n; i++){
            if(points[i][0] > bd){
                bd = points[i][1];
                count++;
            }
        }
        return count;
    }
};
```

**重载比较函数的方法：**

**方法1：**

```cpp
auto cmp = [&](const auto &a, const auto &b){
    return a[1] < b[1];
};
```

**方法2：**

```cpp
static bool cmp(vector<int> & v1, vector<int> &v2){
    return (v1[0] < v2[0]);
}
```

**621. 任务调度器**

```cpp
输入：tasks = ["A","A","A","B","B","B"], n = 2
输出：8
解释：A -> B -> (待命) -> A -> B -> (待命) -> A -> B
     在本示例中，两个相同类型任务之间必须间隔长度为 n = 2 的冷却时间，而执行一个任务只需要一个单位时间，所以中间出现了（待命）状态。 
```

求总时长

**做法1：模拟 + 贪心，但是超时**

* **维护map 字符'A' -&gt; 剩余任务数num**
* **维护另一个map 字符'A' -&gt; expire表示冷却时间**
* 每过一个timer
  * 从中抽取一个num最大的task\(需要按value排序map\)，将其num-1，expire重置
  * timer结束时，将所有expire都减一，直到0为止

```cpp
struct Cmp {
    bool operator()(const pair<char,int>& lhs, const pair<char,int>& rhs) {
        return lhs.second > rhs.second;
    }
};
int leastInterval(vector<char>& tasks, int n) {
    int timer = 0;
    map<char,int>expire;
    map<char,int>task_mp;
    for(int i = 0; i < tasks.size(); i++){
        task_mp[tasks[i]]++;
    }
    for(auto it=task_mp.begin(); it!=task_mp.end(); it++){
        expire[it->first] = 0;
    }
    while(!task_mp.empty()){
        //对map的value进行排序
        vector<pair<char,int>>vec(task_mp.begin(), task_mp.end());
        sort(vec.begin(), vec.end(), Cmp());
        int i = 0;
        while(i < vec.size() && expire[vec[i].first]){
            i++;
        }
        if(i!=vec.size()){
            char key = vec[i].first;
            expire[key] = n + 1;
            task_mp[key]--;
            if(!task_mp[key])
                task_mp.erase(key);
        }
        timer++;
        for(auto it=expire.begin(); it!=expire.end(); it++){
            if(it->second)it->second--;
        }  
    }
    return timer;
}
```

**做法2：桶思想**

**讲解参考：**[**https://leetcode-cn.com/problems/task-scheduler/solution/tong-zi-by-popopop/**](https://leetcode-cn.com/problems/task-scheduler/solution/tong-zi-by-popopop/)\*\*\*\*

**几个原则**

* 桶的个数为单个任务的最大个数num
* 装桶时同一个字符不能装在一个桶中
* delay值 n =2表示，如果桶的size超过n+1 = 3，多出来的部分\(例如：F\)只用加在总数上就ok
* 桶的空缺部分\(大小小于n+1的桶，就是代表有空缺\)就表示这个timer什么任务都没干
* 最后一个的空缺部分不用计算在内

![](../.gitbook/assets/893c01db5923889a865d7a4fe71de22b9519fc5a673473196ab58f26c1073ed2-image.png)

记录单个任务的最大数量num（桶的个数）

* 如果桶装不满
  * 桶的空缺部分\(例如大小小于n+1的桶，就是代表有空缺\)就表示这个timer什么任务都没干
  * 最后一个的空缺部分不用计算在内，单独计算最后一个桶子的任务数 X
* 如果桶能装满
  * 没有空缺，总时间就等于tasks.size\(\)
* NUM1=\(N-1\)\*\(n+1\)+x 与 NUM2=tasks.size\(\) 输出其中较大值即可 

```cpp
int leastInterval(vector<char>& tasks, int n) {
    int size = tasks.size();
    vector<int>vec(26);
    for(auto&e: tasks)
        vec[e-'A']++;
    sort(vec.begin(), vec.end(), [](int& a, int& b){return a > b;});
    int num = vec[0];
    int x = 0;
    for(int i = 0; i < vec.size(); i++){
        x = i;
        if(vec[i]!=num)break;
    }
    return max(size, (n+1)*(num-1) + x);
}
```

**406. 根据身高重建队列**

**题意：详情见leetcode**

```cpp
vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
    int n = people.size();
    vector<vector<int>>res (n, vector<int>(2, -1));
    sort(people.begin(), people.end(), [](vector<int>&a, vector<int>&b){
        return a[0] < b[0] || (a[0] == b[0] && a[1] < b[1]);
    });
    for(int i = 0; i < people.size(); i++){
        int h = people[i][0];
        int pos = people[i][1];
        for(int j = 0; j < res.size(); j++){
            if(res[j][0] == -1|| h == res[j][0]){
                if(!pos){
                    res[j][0] = people[i][0];
                    res[j][1] = people[i][1];
                    break;
                }else
                    pos--;
            }
        }
    }
    return res;
}   
```

