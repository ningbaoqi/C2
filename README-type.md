### 类型
#### 布尔值
+ 在算术逻辑表达式和位逻辑表达式中，`bool`被自动转换为`int`，编译器在转换后的值上执行整数算术运算以及逻辑运算，最终的计算结果需要转换为`bool`；如有必要，指针也能被隐式的转换为bool，其中，`非空`指针对应`true`，值为`nullptr`的指针对应`false`；
#### 字符类型

|C++提供了一系列字符类型|说明|
|------|------|
|`wchar_t`|用于存放unicode等更大的字符集，wchar_t的尺寸依赖于实现，确保能够支持实现环境中用到的最大字符集|
|`char16_t`|该类型存放UTF-16等16位字符集|
|`char32_t`|该类型存放UTF-32等32位字符集|

+ 一个char类型的变量存放一个字符，字符的种类由实现版本所用的字符集决定；

```
//查看任意字符对应的整数值

void intval(){
       for(char c : cin >> c){
              cout << "the value of :" << c << "is" << int {c} << '\n';
       } 
}
```

+ `singned char`存放-127~127之间的值，`unsigned char`存放0~255之间的值；字符类型属于整型，因此，可以在字符类型上执行算术运算和位逻辑运算；

```
void digits(){
       for(int i = 0 ; i != 10 ; i++){
              cout<<static_cast<char> ('0' + i);//字符字面量‘0’先转化为它对应的整数值，再与i相加，所得的int再转回char并输出到cout，‘0’ + i得到的结果本来是一个int，因此如果不加上static_cast<char>的话，输出的结果将会是48 ，49 .......而不是0，1........
       }
}
```

#### 整数类型
+ 一个不加修饰的`int`通常是带符号的；`int64_t`明确规定占用的64位的带符号整数；`uint_fast16_t`至少占用16位的无符号整数，一般被认为是最快的整数；`int_least32_t`至少占用32位的带符号整数，类似于`int`;`size_t`是一个依赖于实现的无符号整数类型；
#### 原始字符串
+ C++提供了原始字符串字面量常量，在原始字符串字面量常量中，反斜线就是反斜线，双引号就是双引号；如`string s = R"(\w\\w)"`显示的字符串就是`\w\\w`；原始字符串字面量常量使用`R"(ccc)"`的形式表示字符序列`ccc`；
#### 大字符集
+ 前缀是`L`的字符串如`L“angst”`由宽字符组成，它的类型是`const wchar_t[]`，类似的，前缀是`LR`的字符串如`LR“(angst)”`也是由宽字符组成的；它的类型同样的`const wchar_t[]`；

### 关键字

|`alignas`|`alignof`|`and`|`and_eq`|`asm`|`auto`|
|-------|-------|-------|-------|-------|-------|
|`bitand`|`bitor`|`bool`|`break`|`case`|`catch`|
|`char`|`char16_t`|`char32_t`|`class`|`compl`|`const`|
|`constexpr`|`const_cast`|`continue`|`decltype`|`default`|`delete`|
|`do`|`double`|`dynamic_cast`|`else`|`enum`|`explicit`|
|`extern`|`false`|`float`|`for`|`friend`|`goto`|
|`if`|`inline`|`int`|`long`|`mutable`|`namespace`|
|`new`|`noexcept`|`not`|`not_eq`|`nullptr`|`operator`|
|`or`|`or_eq`|`private`|`protected`|`public`|`register`|
|`reinterpret_cast`|`return`|`short`|`signed`|`sizeof`|`static`|
|`static_assert`|`static_cast`|`struct`|`switch`|`template`|`this`|
|`thread_local`|`throw`|`true`|`try`|`typedef`|`typied`|
|`typename`|`union`|`unsigned`|`using`|`virtual`|`void`|
|`volatile`|`wchar_t`|`while`|`xor`|`xor_eq`|`export`|

#### 运算符的替代形式

|`and`|`and_eq`|`bitand`|`bitor`|`compl`|`not`|`not_eq`|`or`|`or_eq`|`xor`|`xor_eq`|
|------|------|------|------|------|------|------|------|------|------|------|
|`&&`|`&=`|`&`|`\|`|`-`|`！`|`！=`|`\|\|`|`\|=`|`^`|`^=`|

+ 以下两个表达式等价；

```
bool b = not (x or y) and z;

bool b = !(x || y) && z;
```

#### const型数据的小结

|形式|含义|
|------|------|
|Time const t|t是常对象，常对象必须要有初值，其值在任何情况下都不能改变，如果一个对象被声明为常对象，则通过对象只能调用它的常成员函数，而不能调用该对象的普通成员函数|
|void Time::fun() const|fun是Time类中的常成员函数，可以引用，但不能修改本类中的数据成员，有时在编程时有要求，一定要修改常对象中的某个数据成员的值，C++考虑到实际编程时的需要，对此做出了特殊的处理，对该数据成员声明为mutable如mutable int count;把count声明为可变的数据成员，这样就可以用声明为const的成员函数来修改它的值；在声明函数和定义函数时都要有const关键字；常成员函数可以引用const数据成员，也可以引用非const的数据成员；常成员函数不能调用另一个非const成员函数；|
|Time* const p|p是指向Time类对象的常指针变量，p的指向不能改变|
|const Time* p|p是指向Time类常对象的指针变量，p指向的类对象的值不能通过p来改变|
|const Time & t1 = t|t1是Time类对象t的引用，二者指向同一存储空间，t的值不能改变|

+ 用关键字const来声明常数据成员，常数据成员的值是不能改变的，只能通过构造函数的参数初始化表对常数据成员进行初始化，任何其他函数都不能对常数据成员赋值；

|数据成员|非const的普通成员函数|const成员函数|
|------|------|------|
|非const的普通数据成员|可以引用，也可以改变值|可以引用，但不能改变值|
|const数据成员|可以引用，但不可以改变值|可以引用，但不能改变值|
|const对象|不允许|可以引用，但不能改变值|

#### 指向对象的常指针
+ 将指针变量声明为const型，这样指针变量始终保持为初值，不能改变，即其指向不变；`类名 *const 指针变量名`；、
#### 指向常对象的指针变量
+ `const char* ptr`，const 的位置在最左侧，表示指针变量ptr指向的char变量是常变量，不能通过ptr来改变其值；如果一个变量已被声明为常变量，只能用指向常变量的指针变量指向它；指向常变量的指针变量除了可以指向常变量，还可以指向被声明为const的变量，此时不能通过此指针变量改变该变量的值；

|形参|实参|合法性|改变指针所指向的变量的值|
|-------|-------|-------|-------|
|指向非const型变量的指针|非const变量的地址|合法|可以|
|指向非const型变量的指针|const变量的地址|非法||
|指向const型变量的指针|const变量的地址|合法|不可以|
|指向const型变量的指针|非const变量的地址|合法|不可以|

```
Time* const p;//指向对象的常指针变量

const Time* p;//指向常对象的指针变量
```
