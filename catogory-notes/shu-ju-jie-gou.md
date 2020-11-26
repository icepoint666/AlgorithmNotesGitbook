# 数据结构

**关于数据结构的考察**

**题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 855 | 考场就座 | 有序集合/映射 std::set | 复杂 |

**855. 考场就座**

假设有一个考场，考场有一排共 `N` 个座位，索引分别是 `[0..N-1]`，考生会**陆续**进入考场考试，并且可能在**任何时候**离开考场。

你作为考官，要安排考生们的座位，满足：**每当一个学生进入时，你需要最大化他和最近其他人的距离；如果有多个这样的座位，安排到他到索引最小的那个座位**

每两个相邻的考生看做线段的两端点，新安排考生就是找最长的线段，然后让该考生在中间把这个线段「二分」，中点就是给他分配的座位。`leave(p)` 其实就是去除端点 `p`，使得相邻两个线段合并为一个。

**解决：主要是数据结构的考察**

使用平衡二叉树：可以取最值，也可以修改、删除任意一个值，而且时间复杂度都是 O\(logN\)。

常见的就是红黑树（一种平衡二叉搜索树），特性是自动维护其中元素的顺序，操作效率是 O\(logN\)。这种一般称为「有序集合/映射」。

使用一个「虚拟线段」让算法正确启动

**注意：要对0，N-1端点情况特殊讨论**

**注意：要重载有序集合std::set的比较方式**

**注意：线段距离的计算方法，也要考虑端点，同时只计算端点距离中点的距离（这也是考虑到与端点统一的缘故，而不能计算线段总长）**

**代码：（leetcode并没有编译通过）**

```cpp
#include <bits/stdc++.h>

using namespace std;

int Num;
static int distance (pair<int,int> intv){
    int x = intv.first;
    int y = intv.second;
    if(x == -1)return y;
    if(y == Num)return Num - 1 - x;
    return (y - x) /2;
    //根据distance的语义，应该是计算中点与端点之间的长度，这个实际真正seat距离两边的长度，根据原则肯定是取小的一个
}
class PairComp{
public:
    bool operator () (const pair<int,int> &a, const pair<int,int> &b){
        int dist_a = distance(a);
        int dist_b = distance(b);
        if(dist_a == dist_b)
            return a.first < b.first;
        return dist_a > dist_b;
    };
};

class ExamRoom {
public:
    map<int, pair<int,int>>startmap; //用于leave
    map<int, pair<int,int>>endmap;   //用于leave
    typedef set<pair<int,int>, PairComp> SeatSet; 
    SeatSet s;                       //用于seat
    int Num;
    int distance (pair<int,int> intv){
        int x = intv.first;
        int y = intv.second;
        if(x == -1)return y;
        if(y == Num)return Num - 1 - x;
        return (y - x) /2;
        //根据distance的语义，应该是计算中点与端点之间的长度，这个实际真正seat距离两边的长度，根据原则肯定是取小的一个
    }

    void addInterval(pair<int,int> intv){
        s.insert(intv);
        startmap[intv.first] = intv;
        endmap[intv.second] = intv;
    }
    void removeInterval(pair<int,int> intv){
        s.erase(intv);
        startmap.erase(intv.first);
        endmap.erase(intv.second);
    }
    ExamRoom(int N) {
        Num = N;
        startmap.clear();
        endmap.clear();
        s.clear();
        addInterval(make_pair(-1, Num));
    }
    
    int seat() {
        pair<int,int> intv = *s.begin();
        int seat;
        int x = intv.first;
        int y = intv.second;
        //这个部分对于端点的判定是核心：只要取到的最长线段包含左右端点的都seat在端点上，而且优先小的之后再大的
        if(x == -1){
            seat = 0; //端点处理
        }else if(y == Num){
            seat = Num - 1; // 端点处理
        }else{
            seat = (y + x) / 2; //位置尽量偏小的中点
        }
        removeInterval(make_pair(x,y));
        addInterval(make_pair(x, seat));
        addInterval(make_pair(seat, y));
        return seat;
    }
    
    void leave(int p) {
        //将p左右的线段找出来
        pair<int,int>right = startmap[p];
        pair<int,int>left = endmap[p];
        pair<int,int>merge = make_pair(left.first, right.second);
        removeInterval(left);
        removeInterval(right);
        addInterval(merge);
    }
};
//测试
int main(int argc, char* argv[]){
    ExamRoom* obj = new ExamRoom(10);
    cout << obj->seat() <<endl;
    cout << obj->seat() <<endl;
    cout << obj->seat() <<endl;
    cout << obj->seat() <<endl;
    obj->leave(4);
    cout << obj->seat() <<endl;
}

```

\*\*\*\*

