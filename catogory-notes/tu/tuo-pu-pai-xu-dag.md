# 拓扑排序，DAG

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 207/210 | 课程表 |  判断课程安排图是否是 **有向无环图\(DAG\) bfs/dfs** | 中等 |

**207. 课程表**

你这个学期必须选修 `numCourse` 门课程，记为 `0` 到 `numCourse-1` 。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们：`[0,1]`

给定课程总量以及它们的先决条件，请你判断是否可能完成所有课程的学习？

```text
输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
```

**本题可约化为： 课程安排图是否是 有向无环图\(DAG\)。**

 通过课程前置条件列表 prerequisites 可以得到课程安排图的 邻接表 adjacency，以降低算法时间复杂度

**方法1：入度表（广度优先遍历）** 

算法流程： 

* 1.统计课程安排图中每个节点的入度，生成 入度表 indegrees。 
* 2.借助一个队列 queue，将所有入度为 0 的节点入队。 
* 3.当 queue 非空时将队首节点出队，在课程安排图中删除此节点 pre： 并不是真正从邻接表中删除此节点 pre，而是将此节点对应所有邻接节点 cur 的入度减1，即 `indegrees[cur] -= 1`。 之后减值的，邻接节点 cur 的入度也会被减为 0，说明 cur 所有的前驱节点已经被 “删除”，此时将 cur 入队。 
* 4.在每次 pre 出队时，执行 `numCourses--`； 若整个课程安排图是有向无环图（即可以安排），则所有节点一定都入队并出队过，即完成拓扑排序。

换个角度说，**若课程安排图中存在环，一定有节点的入度始终不为 0**。 因为拓扑排序出队次数等于课程个数，所以返回最终的 numCourses == 0 判断课程是否可以成功安排。 

复杂度分析：O\(M+N\)

```cpp
bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    vector<int>indegrees(numCourses, 0); //入度
    vector<vector<int>>graph(numCourses, vector<int>{}); //邻接表（节点可能多连一，一连多）
    for(int i = 0; i < prerequisites.size(); i++){
        graph[prerequisites[i][0]].push_back(prerequisites[i][1]);
        indegrees[prerequisites[i][1]]++;
    }
    queue<int>que;
    for(int i = 0; i < numCourses; i++)
        if(!indegrees[i])que.push(i);
    while(!que.empty()){
        int cur = que.front();
        que.pop();
        numCourses--;
        for(int i = 0; i < graph[cur].size(); i++){
            if(!--indegrees[graph[cur][i]])que.push(graph[cur][i]);
        }
    }
    return numCourses == 0;
}
```

**方法2：深度优先遍历** 原理是通过 DFS 判断图中是否有环。

算法流程： 借助一个标志列表 **flags**，用于判断每个节点 i （课程）的状态：

* 未被 DFS 访问：i == 0； 
* 已被其他节点启动的 DFS 访问：i == -1； 
* 已被当前节点启动的 DFS 访问：i == 1。 

对 numCourses 个节点依次执行 DFS，判断每个节点起步 DFS 是否存在环，若存在环直接返回 False。

DFS 终止条件： 

* 当 flag\[i\] == -1，说明当前访问节点已被其他节点启动的 DFS 访问，无需再重复搜索，直接返回 True。
* 当 flag\[i\] == 1，说明在本轮 DFS 搜索中节点 i 被第 22 次访问，即 课程安排图有环 ，直接返回 False。 
* 将当前访问节点 i 对应 flag\[i\] 置 1，即标记其被本轮 DFS 访问过； 
* 递归访问当前节点 i 的所有邻接节点 j，当发现环直接返回 False；
* 当前节点所有邻接节点已被遍历，并没有发现环，则将当前节点 flag 置为 -1 并返回 True。 

若整个图 DFS 结束并未发现环，返回 True。

作者：jyd 链接：[https://leetcode-cn.com/problems/course-schedule/solution/course-schedule-tuo-bu-pai-xu-bfsdfsliang-chong-fa/](https://leetcode-cn.com/problems/course-schedule/solution/course-schedule-tuo-bu-pai-xu-bfsdfsliang-chong-fa/) 来源：力扣（LeetCode） 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

