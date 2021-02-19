# strcat实现

类似的写法可以写strcpy等其他,

**关键**是输入的数组a，数组b必须有固定的长度

不能声明为char\* a, char \* b，如果这样造成内存越界

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
  char a[6] = "hello"; //关键：一定要声明大小不然会直接默认为size为0，这样tmp++就会数组越界，而且尺寸必须要多一位
  char b[6] = "world";
  std::cout << strcat(a, b) <<std::endl;
}
```

