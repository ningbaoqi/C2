### 链接非C++代码
```
extern "C" char* strcpy(char* , const char*);//指定它采用C链接规范，主要作用就是为了能够正确实现C++代码调用其他C语言代码
extern "C"{//C++提供了一种机制为一组声明制定链接规范；这种构造通常称为链接块，甚至可用来封装完整的C头文件，从而使之适用于C++程序
       char* strcpy(char* , const char*);
       int strcmp(const char* , const char* );
}
extern "C"{//程序员常常用这种技术从C头文件生成C++头文件
       #include <string.h>
}
```
### 代码编写实例
#### Vector.h
```
class vector{
       public:
              vector(int s);
              double& operator[](int i);
              int size();
       private:
              double* elem;
              int size;
}
```
+ 这段声明被放置于文件Vector.h文件中，我们称为这种文件为头文件，用户将其包含进程序以访问接口；
#### user.cpp
```
#include "vector.h"
#include <cmath>
using namespace std;
double sqrt_sum(vector& v){
       double sum = 0 ;
       for(int i = 0 ; i != v.size ; ++i){
              sum += sqrt(v[i]);
       }
       return sum;
}
```
#### Vector.cpp
+ 为了帮助编译器确保一致性，负责提供Vector实现部分的.cpp文件同样应该包含提供接口的.h文件；
```
#include "vector.h"
vector::vector(int s) : elem {new double[s]} , sz {s}{
   
}
double& vector::operator[](int i){
       return elem[i];
}
int vector::size(){
       return sz;
}

```
### 预定义宏

|编译器预定义了一些宏|说明|
|------|------|
|`_cplusplus`|在C++编译器中有定义，在C++11它的值是201103L|
|`_DATE_`|`yyyy:mm:dd`格式的时间|
|`_TIEM_`|`hh:mm:ss`格式的时间|
|`_FILE_`|当前源文件的名字|
|`_LINE_`|当前源文件的代码行数|
|`_FUNC_`|是一个由具体实现定义的C风格字符串，表示当前函数的名字|
|`_STDC_HOSTED_`|如果当前实现是宿主式的则为1，否则为0，此外，还有一些宏是根据具体条件定义的|
|`_STDC_`|在C语言编译器中有定义|
|`_STDC_MB_MIGHT_NEQ_WC_`|在wchar_t的编码体系中，如果基本字符集的成员的值与它作为普通字符字面值常量的值可能不同|
|`_STDCPP_STRICT_POINTER_SAFETY_`|如果当前实现有严格的指针安全机制则为1，否则是未定义的|
|`_STDCPP_THREADS_`|如果程序可以有几个执行线程则为1，否则是未定义的|
