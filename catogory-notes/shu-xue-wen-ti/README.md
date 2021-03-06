# 数学问题

leetcode内置了`math.h`库，所以不需要实现一些数学运算的函数

### 一些常见操作：

* pow\(a, n\)
* sqrt\(n\)
* ceil\(n\) 天花板
* floor\(n\) 地板
* int quotient = n / d;
* int reminder = n % d;

### 分解质因子\(O\(sqrt\(N\)\)

关键：除到根号既可以停下来了

```cpp
int main(){
    long a;
    cin >> a;
    long cur = 2;
    long s = sqrt(a);
    while(a > 1 && cur <= s){
        if(a % cur == 0){
            a /= cur;
            cout << cur << " ";
        }else{
            cur++;
        }
    }
    if(a > 1)cout << a << " ";
    return 0;
}
```

### 验证素数 \(O\(sqrt\(N\)\)

```cpp
bool isPrime(int n){
    if(n <= 1)return false;
    bool res = true;
    for(int i = 2; i * i <= n ; ++i){
        if(n % i == 0)return false;
    }
    return true;
}
```

### 统计素数

**思路：**判断到i\*i&lt;N就可以了O\(sqrt\(N\)\)，不需要单独判断素数的函数

在每次根据标记判断完素数后

* 如果是素数，还要把之后所有它的倍数都标记成非素数
* 如果不是素数，就不用标记了，因为说明前面已经标记过了

**时间复杂度**：其操作数应该是：

n/2 + n/3 + n/5 + n/7 + ... = n × \(1/2 + 1/3 + 1/5 + 1/7...\)

最终结果是 O\(N \* loglogN\)

**空间复杂度**：O\(N\)

```cpp
int countPrimes(int n) {
    boolean[] isPrim = new boolean[n];
    Arrays.fill(isPrim, true);
    for (int i = 2; i * i < n; i++) 
        if (isPrim[i]) 
            for (int j = i * i; j < n; j += i) 
                isPrim[j] = false;
    
    int count = 0;
    for (int i = 2; i < n; i++)
        if (isPrim[i]) count++;
    
    return count;
}
```

### **MOD的处理**

有时候数字过大，可能会overflow，为了防止这种情况，需要将结果模MOD

但是如果模的数很接近1e9+7，这时候再乘3，很容易会overflow，因为int overflow范围也就比2e9多一点

这时候需要转换为**long long类型**

**long long类型 + MOD版本的普通pow：**

```cpp
long long MOD = 1e9+7;
int mod_pow(int a, int b){
    long long res = 1;
    while(b--){
        res = (res * a) % MOD;
    }
    return (int)res;
}
```

### **快速幂**

```cpp
int qpow(int a, int n){
    int ans = 1;
    while(n){
        if(n&1)        //如果n的当前末位为1
            ans *= a;  //ans乘上当前的a
        a *= a;        //a自乘
        n >>= 1;       //n往右移一位
    }
    return ans;
}
```

快速乘

```cpp
int mul(int a, int k){
    int ans = 0;
    while(k){
        if(k & 1)ans += a;
        k >>= 1;
        a += a;
    }
    return ans;
}
```

### 阶乘的计算

计算 C\_{m+n}^m

```cpp
for (int x = n, y = 1; y < m; ++x, ++y) {
    ans = ans * x / y;
}
```

### **负数位运算 与 overflow的坑**

注意负数的位运算，并不会考虑前面的“负位”

例如：-32&gt;&gt;1 = -16

但是注意**-1是不能在右移** -1&gt;&gt;1 = -1

注意：MIN\_INT = -\(1&lt;&lt;31\)，如果它取反会overflow，因为范围是 -\(1&lt;&lt;31\) ~\(1 &lt;&lt;31-1\)，注意取反的话转成**long类型**就可以了





