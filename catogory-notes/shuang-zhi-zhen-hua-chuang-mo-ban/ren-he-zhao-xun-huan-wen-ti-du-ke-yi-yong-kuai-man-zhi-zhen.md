# 任何找循环问题都可以用快慢指针

示例问题：**快乐数202**，也是一个比较近似于找循环的问题

```cpp
int cal(int n){
    int res = 0;
    while(n){
        int cnt = n % 10;
        res += cnt*cnt;
        n /=10;
    }
    return res;
}
bool isHappy(int n) {
    int fast = n;
    int slow = n;
    while(1){
        fast = cal(cal(fast));
        slow = cal(slow);
        if(fast == slow){
            if(fast != 1)return false;
            return true;
        }
    }
    return true;
}
```

