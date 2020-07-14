# 双指针滑窗模板

> 参考**leetcode438**题目

**Trick1:双指针**

双指针初始化，关键在于两个指针动态的移动：

**right指针先满足条件，left指针再缩小窗口，然后不行的话right再继续增大窗口凑条件**

```cpp
int left = 0, right = 0;
```

**Trick2:一个骚操作**

如果用map充当一个计数器，刚开始添加的时候直接`mp[key]++`; 就默认加入值为1了，不需要复杂的验证是否存在然后从0开始加。

```cpp
unordered_map<char, int> needs;
for (char c : t) needs[c]++;
```

**为了避免单独对窗口没有填满的部分进行特殊判断，双指针设定：**

**Trick3:三步:**

* 维护right指针相关的计数判断，右移right++
* 如果匹配条件，但是window大小不太合适，就处理left指针
* 维护left指针相关的计数判断，右移left++

**Trick4:验证两个计数器相等的方法，不需要O\(mp.size\(\) \* n\)的复杂度**

用一个match来记录当前匹配两个计数器的匹配个数，每次更新的时候只处理元素更改的前后两个key即可

```cpp
unordered_map<char, int> window;

while (right < s.size()) {
        //维护right指针相关的计数判断，添加
        char c1 = s[right];
        if (needs.count(c1)) {
            window[c1]++;
            //Trick4
            if (window[c1] == needs[c1])
                match++;
        }
        right++;

        while (match == needs.size()) {
            // 如果 window 的大小合适
            // 就把起始索引 left 加入结果
            if (right - left == t.size()) {
                res.push_back(left);
            }
            //维护left指针相关的计数判断，移去
            char c2 = s[left];
            if (needs.count(c2)) {
                window[c2]--;
                //Trick4
                if (window[c2] < needs[c2])
                    match--;
            }
            left++;
        }
    }
```

