### 选择语句
+ 如果`x`是一个整数，`if(x)`等价于`if(x != 0)`；对于指针`p`，`if(p)`等价于`if(p != nullptr)`；`case`标签中出现的表达式必须是整形或枚举类型的常量表达式；`if(double d = prim(true))`首先声明了`d`并给它赋值，然后把初始化后`d`的值作为条件的值进行检查；
### goto语句

```
void do_something(int i , int j){
       for(i = 0 ; i != n ; i++){
              for(j = 0 ; j != m ; j++){
                     if(nm[i][j] == a){
                            goto found;
                     }
                     //do something
                     .......
              }
       }
       found:
              //do something
}
```

### 声明多个名字
+ `int *p , y`准确的含义是`int* p；int y`；`int x , *q`含义为`int x；int *q`；`int v[10]，*pv`的含义是`int v[10]；int* pv`；
### 声明const常量
+ 在定义变量时，如果加上`const`关键字，则变量的值在程序运行期间不能改变，这种变量称为`常变量`；`const int a = 3;`用`const`来声明这个变量的值不能改变，指定其值始终是`3`；在定义常变量时必须同时对它初始化，以后它的值不能再改变；`const`主要用于说明接口，这样在把变量传入函数时就不必担心变量会在函数内被改变了；`constexpr`主要用于说明常量，作用是允许将数据置于只读内存中，不太可能被破坏，以及提升性能；

```
const int dmv = 17; //dmv是一个命名的常量
int var = 17; //var不是常量
constexpr double max1 = 1.4 * square(dmv); //如果square(17)是常量表达式则正确
constexpr double max2 = 1.4 * square(var); //错误，var不是常量表达式
const double max3 = 1.4 * square(var); //正确，可以在运行时求值
double sum(const vector<double>&); //sum不会变更它的参数的值
vector<double> v {1.2 , 3.4 , 5.6};//v不是常量
const double s1 = sum(v); //在运行时求值
constexpr double s2 = sum(v); //错误sum(v)不是常量表达式
```

+ 如果某个函数用在常量表达式中，即该表达式在编译时求值，则函数必须定义成`constexpr`：`constexpr double square(double x) {return x* x};`；`constexpr`函数可以接受非常量实参，但此时结果将不会是一个常量表达式，当程序的上下文不需要常量表达式时，可以使用非常量表达式实参来调用`constexpr`函数，这样就不用把同一个函数定义两次了，其中一个用于常量表达式，另一个用于变量；

### 作用域

```
int x ;
void f2(){
       int x = 1;//隐藏全局变量x
       ::x = 2;//为全局变量x赋值
       x = 2;//为局部变量x赋值
}
```
### 初始化
+ 初始化器有四种可能的形式：`Ｘ a1 {v};`、`X a2 = {v};`、`X a3 = v;`、`X a4(v);`；当使用`auto`关键字从初始化器推断变量的类型时，没必要采用列表初始化的方式，而且如果初始化器是`{}`列表，则推断得到的数据类型肯定不是我们想要的结果，`auto z1 {99};//z1的类型是initializer_list<int>`、`auto z2 = 99;//z2的类型是int`；因此使用`auto`的时候应该选择`=`的初始化形式；`vector<int> v1{99}//v1包含一个元素，该元素的值是99`、`vector<int> v2(99)//v2包含99个元素，每个元素都取值默认值0`；

```
double d1 = 2.3;
double d2 {2.3};
complex<double> z = 1;//数值为双精度浮点数的复数
complex<double> z2 {d1 , d2};
complex<double> z3 = {1 , 2};//当使用{......}的时候，符号=是可选的

int i1 = 7.2;//i1变成了7
int i2 {7.2};//错误，试图执行浮点数向整数的类型转换

auto b = true; //变量的类型是bool
auto ch = 'x'; //变量的类型是char
auto i = 123; //变量的类型是int
auto d = 1.2; //变量的类型是double
auto z = sqrt(y); //z的类型是sqrt(y)的返回类型
```
