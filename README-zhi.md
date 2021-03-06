### 指针
#### void*指针
+ `void*`的含义是指向未知类型对象的指针；除了函数指针和指向类成员的指针，指向其他任意类型对象的指针都能被赋给一个`void*`类型的变量，此外，一个`void*`能被赋给另一个`void*`，两个`void*`能比较是否相等，我们该能把`void*`显式的转换成其他类型，因此编译器事实上并不知道`void*`所指的对象到底是什么类型，所以对它执行其他操作可能不太安全并且会引发编译器错误，要想使用`void*`我们必须把它显式的转换为某一个特定类型的指针；

```
void f(int* pi){
       void* pv = pi;//ok发生了从int*到void*的隐式类型转换
       *pv;//错误，不允许解引用void*
       int* pi2 = static_cast<int*>(pv);//显式转换回int*
       double* pd1 = pv;//错误
       double* pd2 = pi;//错误
       double* pd3 = static_cast<double*>(pv);//不安全
}
```

+ 一般情况下，如果某个指针已经被转换成（强制类型转换）指向一种与实际所指对象类型完全不同的新类型，则使用转换后的指针是不安全的行为；`void*`最主要的用途是当我们无法假定对象的类型时，向函数传递指向该对象的指针，它还用于从函数返回未知类型的对象，要想使用这样的对象，必须先进行显式类型转换，用到`void*`指针的函数通常位于系统的最底层，函数指针和指向类成员的指针不能被赋给`void*`；在强制类型转换时，得到一个所需类型的中间数据，但原来变量的类型未发生变化；

#### 字面值常量nullptr
+ 字面值常量`nullptr`表示空指针，即不指向任何对象的指针，可以把`nullptr`赋给其他任意指针类型，但不能赋给其他内置类型；`nullptr`只有一个，它可以用于任意指针类型；
#### 指针与const
+ `constexpr`编译时求值，`const`在当前作用域内，值不发生变化；

```
const int model = 90;//model是一个const
const int v[] = {1 , 2 , 3 , 4 };//v是一个const
const int x;//错误，缺少初始化
```
+ 因为无法给`const`对象赋值，`所以它必须初始化`；一旦把某物声明成`const`，就确保它的值在其作用域内不会发生改变；
```
void f(char* p){
       char s[] = "Grom";
       const char* pc = s;//指向常量的指针，不能通过指针变量改变它指向的对象的值
       pc[3] = 'h';//错误，pc指向常量
       pc = p;//ok
       char* const cp = s;//常量指针，指向指针变量的值是常量，即指针变量的指向不能改变
       cp[3] = 'a';//ok
       cp = p;//错误，cp是一个常量
       const char* const cpc = s;//指向常量的常量指针，指向常量的常指针变量，即指针变量指向一个固定的对象，该对象的值不能通过指针变量改变
       cpc[3] = 'a';//错误，cpc是指向常量
       cpc = p;//错误，cpc本身是一个常量
}
char* const cp;//指向char的常量指针
char const* pc;//指向常量const的指针
const char* pc2;//指向常量char的指针
```
+ C++允许把`非const变量的地址`赋给指定`常量的指针`，这不会造成什么不可接受的后果，相反，常量的地址不能被赋给某个不受限的指针，因为如果这样的话，用户可能通过操作该指针修改对象的值，这显然是不被允许的；

#### 指针与所有权
+ 资源必须先分配后释放，使用`new`分配内存，用`delete`释放内存，用`fopen()`打开文件，用`fclose()`关闭文件；如果参数传递过来的指针也不要`delete`，应该由创建它的作用域`delete`；可以把表示某种所有权的指针全部置于`vector`、`string`和`unique_ptr`等资源句柄中；

#### 指针的数据类型小结

|变量定义|类型表示|含义|
|------|------|------|
|int i|int|定义整型变量i|
|int *p|int *|定义p为指向整型数据的指针变量|
|int a[5]|int[5]|定义整型数组a，它有5个元素|
|int *p[4]|int *[4]|定义指针数组p，它由4个指向整型数据的指针元素组成|
|int (*p)[4]|int(*)[4]|p为指向包含4个元素的一维数组的指针变量|
|int f()|int ()|f为返回整型函数值的函数|
|int *p()|int *()|p为返回一个指针的函数，该指针指向整型数据|
|int (*p)()|int (*)()|p为指向函数的指针，该函数返回一个整型值|
|int **p|int **|p是一个指针变量，它指向一个指向整数数据的指针变量|
|int const *p|int const *|p是常指针，其值是固定了，即其指向不能变|
|const int * p|const int *|p是指向常量的指针变量，不能通过p改变其指向的对象的值|
|const int * const p|const int * const|p是指向常量的常指针，其指向不能改变，且不能通过p改变其指向的变量的值|
|void *p|void *|p是一个指针变量，基类型为void，不指向具体的对象|
