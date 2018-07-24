### 名字空间
```
namespace Text_lib{
       class Glyph{}//在名字空间Text_lib中定义了类Glyph
}
//使用类Glyph类
Text_lib::Glyph(Text_lib::Line& ln , int i);
```
#### 显式限定
```
namespace Parser{
       double expr(bool);
       double term(bool);
       double prim(bool);
}
double val = Parser::expr();//使用
double Parser::expr(bool b){//定义
       //.........
}
```
+ 不能用限定符语法在名字空间的定义之外为其声明一个新成员；
#### using声明
```
using std::string;//这样可以在代码中直接使用string而不用带上名字空间
```
#### using指示
+ 使用`using指示`，要求编译器允许我们在所在作用域内无需使用限定即可访问某个名字空间中的所有名字；
```
using namespace std;//来自std名字空间的所有成员的每个名字都可直接访问
```
#### 名字空间是开放的
+ 名字空间是开放的，即可以从多个分离的名字空间声明中向一个名字空间添加名字；这样，名字空间的成员就不需要连接放置在单一的文件中；
```
namespace A{
       int f();//现在A包含成员f()
}
namespace A{
       int g();//现在A包含成员f()和g()
}
```
#### 组合使用名字空间
##### 便利性和安全性
+ `using声明`将名字添加到局部作用域中，而`using指示`则不会，它只是简单的令名字在其作用域中可访问；
```
namespace X {
        int i , j , k;
}
void f1(){
       using namespace X;
       i++;
}
```
##### 名字空间别名
```
namespace American_Telephone_and_Telegraph(
)
namespace ATT = American_Telephone_and_Telegraph;//就可以使用ATT操作成员了
```
##### 组合名字空间
```
namespace My_lib{
       using namespace His_string;
       using namespace Her_vector;
       void my_fct(String&);
}
```
##### 名字空间嵌套
```
namespace X{
       void g();
       namespace Y{
              void f();
              void ff();
       }
}
void X::Y::ff(){
       f();g();
}
```
##### 无名名字空间
```
namespace{
       void f();
}
//每个无名名字空间都有一个隐含的using指示，前面声明的等价于下面的声明
namespace $$${
       void f();
}
using namespace $$$;
```
+ 其中`$$$`是此名字空间所在作用域中的一个独一无二的名字，特别的，不同编译单元中的无名名字空间是不同的；
