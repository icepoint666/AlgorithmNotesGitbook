# 翻转字符串——多步翻转法

### 翻转模板

```cpp
void reverse(string& s, int left, int right){
    while(left < right){
        char tmp = s[left];
        s[left] = s[right];
        s[right] = tmp;
        left++;
        right--;
    }
}

reverse(str, left, right);
reverse(str.begin()+left, str.begin()+right-1); //STL
```

**题目：**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 151/186 | 翻转字符串里的单词（151需要处理空字符，186是简化版） | 两次翻转法 | 经典 |

**151. 翻转字符串里的单词**

**要求：inplace完成 + 不能在string中直接删字符**

```text
输入："the sky is blue"
输出："blue is sky the"

输入："  hello world!  "
输出："world! hello"
解释：输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。

输入：s = "  Bob    Loves  Alice   "
输出："Alice Loves Bob"
```

**解法**：

**in-place做法：先翻转整个字符串，再把每个单词翻转了**

* 处理多余空格时，**不能直接删空字符**（vector中删字符的复杂度太高）
  * 往后处理时，记录一个**readindex: i**,  一个**storeindex：index**
  * 处理完resize字符串就好了
* 可以使用C++ STL自带的reverse\(it.begin\(\)+i, it.begin\(\)+j\)，也可以自己写reverse

```cpp
string reverseWords(string s) {
    if(s.empty())return s;
    reverse(s.begin(), s.end());
    int index = 0;
    for(int i = 0; i < s.size(); i++){
        if(s[i] != ' '){
            if(index!=0)s[index++] = ' '; //最开始的单词是不需要前面开头空格的
            int j = i;
            while(j < s.size() && s[j] != ' ')s[index++] = s[j++];
            reverse(s.begin()+index-(j-i), s.begin()+index);
            i = j - 1;
        }
    }
    s.resize(index); //注意resize
    return s;
}
```

