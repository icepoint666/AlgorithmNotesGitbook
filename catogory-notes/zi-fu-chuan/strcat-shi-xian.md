# strcat, strcmp, strlen, strcpy的实现

这四个函数也都属于&lt;string.h&gt;头文件中的函数，用于字符串操作

### strcat

**char \*strcat\(char \*dest, const char \*src\)** 把 **src** 所指向的字符串追加到 **dest** 所指向的字符串的结尾

**关键**是输入的数组a，数组b必须有固定的长度

不能声明为char\* a, char \* b，如果这样造成内存越界

**声明C风格字符串关键**：**一定要声明大小**不然会直接默认为size为0，**这样tmp++就会数组越界**，而且**尺寸必须要多一位**

```cpp
#include <iostream>
#include <assert.h>

char* strcat(char* dst, const char* src){ //牢记
    assert(dst!=NULL && dst!=NULL);
    char* tmp = dst;
    while(*tmp != '\0')
        tmp++;
    while((*tmp++=*src++)!='\0');
    return dst;
}

int main()
{
  char a[6] = "hello";
  char b[6] = "world";
  std::cout << strcat(a, b) <<std::endl;
}
```

### strcmp

操作方法类似于前面的memcmp，甚至还不需要指定大小

**关键：明确strcmp的比较规则，什么叫做更大，任何字符的数值越大就意味着更大，如果是空字符的话'\0'意味着最小**

```cpp
#include <iostream>
#include <assert.h>
#include <stdio.h>

int strcmp(const char* str1, const char* str2){
    while(*str1!='\0' && *str1 == *str2){
      ++str1;
      ++str2;
    }
    return *str1 - *str2;
}

int main()
{
  char a[6] = "hello"; //c字符串如果定义长度为6，那么最多只能包含5个字符，因为最后一个字符必然要留给'\0'，不然就会报错
  char b[6] = "world";
  char c[5] = "worl";
  std::cout << strcmp(a, b) <<std::endl;
  std::cout << strcmp(b, c) <<std::endl;
  std::cout << strcmp(a, c) <<std::endl;

}
```

### strlen

比较简单

```cpp
#include <iostream>
#include <assert.h>
#include <stdio.h>

unsigned int strlen(const char* src){ //牢记
    char* tmp = (char*)src; //临时变量，如果是用const char*转换的话，需要考虑用强制转换
    unsigned int count = 0;
    while(*tmp++ != '\0')
        count++;
    return count;
}

int main()
{
  char a[6] = "hello"; //一定要声明大小不然会直接默认为size为0，这样tmp++就会数组越界，而且尺寸必须要多一位
  char b[6] = "word";
  char c[32];
  std::cout << strlen(a) <<std::endl;
  std::cout << strlen(b) <<std::endl;
  std::cout << strlen(c) <<std::endl;
}
```

### strcpy

可以参考memcpy的实现，不过不需要转成char\*

参考strcat + memcpy的实现 就也很容易实现

