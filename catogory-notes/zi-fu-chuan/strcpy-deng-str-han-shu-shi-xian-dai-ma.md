# memcpy, memcmp, memmove的实现

首先这三个函数都属于&lt;string.h&gt;头文件中的函数，用于拷贝内存，比较内存

### memcmp

**记忆关键点（3个）：**

* cmp无论是memcmp还是strcmp都是**不需要临时变量的，**不涉及修改
* 中间循环中止条件就是是否相等，是否到头
* 返回不相等字符的差值即可（可以转为unsigned char来比较，更为妥当）

```cpp
#include <iostream>
#include <assert.h>
#include <string.h>

int my_memcmp(const void* buf1, const void* buf2, unsigned int count){//count表示bytes数目
    if(!count)return 0;
    while(--count && *(char*)buf1 == *(char*)buf2){
        buf1 = (char*)buf1 + 1; //如果换成void* + 1会变成什么样呢
        buf2 = (char*)buf2 + 1;
    }
    return (*((unsigned char*)buf1)-*((unsigned char*)buf2));
}

int main()
{
  char a[6] = "hello"; //一定要声明大小不然会直接默认为size为0，这样tmp++就会数组越界，而且尺寸必须要多一位
  char b[6] = "helld";
  int c = 254;
  int d = 128;
  std::cout << my_memcmp(a, b, 4) <<std::endl;
  std::cout << my_memcmp(&c, &d, 1) << std::endl;
  std::cout << memcmp(a, b, 4)<<std::endl;
  std::cout << memcmp(&c, &d, 1) << std::endl;
}
```

### memcpy

**记忆关键点，几个原则：**

1. 不要破坏传进来的形参，**定义新的临时变量**来操作
2. 转换为char\* 考虑指针的类型，不同类型的指针不能直接++赋值
3. 注意：**overlap情况**

   * 需要从高地址处向前copy
   * memcpy对内存空间有要求的,dest和src所指向的内存空间不能重叠,否则复制的数据是错误的

   但是注意：原本的memcpy实现也没有考虑从覆盖的情况，就是单纯的从前到后来复制，所以说**这里实现的memcpy也算是一种改进版的memcpy了**

**另外要注意：**

* 注意sizeof操作字符串指针的话，大小会等于8或者4, sizeof\(\*src\) = 1，所以不要sizeof指针来判定大小

```cpp
#include <iostream>
#include <stdio.h>
#include <assert.h>
#include <string.h>


void* my_memcpy(void* dest, const void* src, unsigned int count){ 
    char *d;  //临时变量
    const char *s;  //临时变量
    if ((dest > (src + count)) || (dest < src)){
        d = (char*)dest;
        s = (char*)src;
        while (count--)
            *d++ = *s++;        
    }
    else{//意味着dst指针与src指针区域有所覆盖
        d = (char *)(dest + count - 1); /* offset of pointer is from 0 */
        s = (char *)(src + count -1);
        while (count --)
            *d-- = *s--;
    }
    return dest;
}



int main()
{
  char src[50] = "hello world how are you!";
  char dst[50];
  memcpy(dst, src, 50);  
  std::cout << dst << std::endl;
  
  //第二个测试用例会出现问题，但是本身我用string.h的memcpy测试也是会出现问题的，所以无所谓
  char dst_1[23];
  //my_memcpy(dst_1, src, 16);
  memcpy(dst_1, src, 16);
  std::cout << dst_1 <<std::endl;
}
```

### memmove

void _memmove\(void_ str1, const void \*str2, size\_t n\) 从 str2 复制 n 个字符到 str1， 

注意：但是在重叠内存块这方面，memmove\(\) 是比 memcpy\(\) 更安全的方法。 

* 如果目标区域和源区域有重叠的话，memmove\(\) 能够保证源串在被覆盖之前将重叠区域的字节拷贝到目标区域中，复制后源区域的内容会被更改。 
* 如果目标区域与源区域没有重叠，则和 memcpy\(\) 函数功能相同。memcpy效率更高

注意：临时变量 + 临时拷贝字符串数组tmp

```cpp
#include <iostream>
#include <assert.h>
 
void* memmove(void* dest, const void* src, int count){
    char temp[count];  //开辟新空间拷贝，防止覆盖
    char* d = (char*)dest;  //临时变量
    const char* s = (const char*)src;   //临时变量
    for(int i = 0; i < count; ++i){
        temp[i] = s[i];
    }
    for(int i = 0; i < count; ++i){
        d[i] = temp[i];
    }
    return dest;
}

int main()
{
  char src[50] = "hello world how are you!";
  char dst[50];
  memmove(dst, src, 50); 
  std::cout << dst << std::endl;
  
  char dst_1[23];
  memmove(dst_1, src, 16);
  std::cout << dst_1 <<std::endl;
}
```

