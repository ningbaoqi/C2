### 类
#### 访问控制

|访问控制符号|说明|
|-------|-------|
|`private`|仅可被所属类的成员函数或友元函数所使用，类中的成员默认是private的，结构体中的成员默认是public的|
|`protected`|仅可被所属类的成员函数和友元函数以及派生类的成员函数和友元函数所使用|
|`public`|可以被任何函数所使用|

#### 基类成员在派生类中的访问属性

|在基类的访问属性|继承方式|在派生类中的访问属性|
|------|------|------|
|private|public|不可访问|
|private|private|不可访问|
|private|protected|不可访问|
|public|public|public|
|public|protected|protected|
|public|private|private|
|protected|public|protected|
|protected|protected|protected|
|protected|private|private|

#### 构造函数和析构函数

```
struct Tracer{
       string mess;
       Tracer(const string& s) : mess {s} {clog<<mess;}
       ~Tracer(){clog<<"~"<<mess;}
};
```
+ 析构函数不接受参数，每个类只能有一个析构函数，析构函数会被隐式调用，只有在极少数情况下用户才需要显式调用析构函数；

```
class Vector{
       public:
              Vector(int s) : elem {new double[s]} , sz {s}{};//构造函数获取内存
              ~Vector(){delete[] elem;}//析构函数，释放内存
       private:
              double* elem;
              int sz;
}
Vector v = new Vector(1);//在超出了该对象的作用域将会隐式调用该对象的析构函数
delete p;//对象的指针，使用delete操作也会调用该对象的析构函数
```

+ 构造函数调用顺序：基类的构造函数---基类成员的构造函数---本类的构造函数；析构函数的调用顺序：本类的析构函数----本类成员的析构函数----基类的析构函数；

#### 调用构造函数和析构函数
+ 当对象退出作用域或被`delete`释放时，析构函数会被隐式调用；但是使用`private修饰析构函数只能通过显式析构`；

```
class A{
       public:
               void destroy(){this -> ~A()}//显式析构
       private:
              ~A();//不能隐式析构
};
```

#### explicit构造函数
+ 可以指明构造函数不能用作隐式类型转换，如果构造函数的声明带有关键字`explicit`则它只能用作初始化和显式类型转换；
### 运算符重载

```
class complex{
              double rm , im;
       public:
              complex(double r , double i) : rm {r} , im {i}{}
              complex operator+(complex);
              complex operator*(complex);
};
```

+ 可以使用运算符`+、*`操作它，用`complex::operator+()和complex::operator*()`分别表示`+、*`的含义，假设`b`、`c`的类型是`complex`，则`b+c`等价于`b.operator+(c)`；

```
Time operator++();//声明前置自增运算符++重载函数
Time operator++(int);//声明后置自增运算符++重载函数
```

### static成员
+ 是类的一部分但不是某个类对象一部分的变量称为`static成员`，`static成员`只有唯一副本，而不是像普通非static成员那样每个对象都有其副本，类似的，需要访问类成员而不需要通过特定对象调用的函数称为`static成员函数`；调用`static`成员函数`Date::set_default(2 ,3 , 123);`；在多线程代码中，`static`数据成员需要某种`锁机制`或`访问规则`来避免竞争条件；静态数据成员既可以通过对象名引用，也可以通过类名来引用；
### 静态成员函数

```
static int volume();
Box::volume();
a.volume();//也允许通过对象名调用静态成员函数
```

+ 静态成员函数是类的一部分而不是对象的一部分，如果要在类外调用公用的静态成员函数，要用类名和域运算符`::`；静态成员函数可以直接引用本类中的静态成员，静态成员函数主要用来访问静态数据成员，而不访问非静态成员；

### 虚函数

+ 虚函数机制允许程序员在基类中声明函数，然后在每个派生类中重新定义这些函数；`多态性`：向不同的对象发送同一个消息，不同的对象在接收时会产生不同的行为；多态性分为两种：`静态多态性`和`动态多态性`，`静态多态性`是通过`函数重载`实现的，`静态多态性`又称为`编译时的多态性`；`动态多态性`的特点是：不在编译时确定调用的是哪个函数，而是在程序运行过程中才动态的确定操作所针对的对象，它又称为`运行时的多态性`，`动态多态性`是通过`虚函数`实现的通过多态的方式增加了灵活性，但是我们只能通过`引用`或`指针`操作对象；只能用virtual声明类的成员函数，把它作为虚函数，而不能将类外的普通函数声明为虚函数；

```
class Employee{
       public:
               Employee(const string& name , int dept);
               virtual void print() const;
       private:
               string first_name , family_name;
}

```

#### 覆盖控制

|特征|说明|
|-------|-------|
|`virtual`|函数可能被覆盖|
|`=0`|函数必须是virtual，且必须被覆盖|
|`override`|函数要覆盖基类中的一个虚函数|
|`final`|函数不能被覆盖|

```
class Deried : public Base{
       void f() override;//正确base有一个virtual f()的话
}；
void Deried::f() override{}//错误，类外不能使用override
void Deried::f(){}//正确
Type type() override final;//防止进一步覆盖
```

+ 通过在类名后加上`final`如`class A final`可以将该类A的所有`virtual`成员函数都声明为`final`的，它不仅阻止了覆盖，也阻止了从一个类进一步派生其他类；

### 类内函数定义

#### 常量成员函数
+ `int day() const {return d;}`函数声明中参数列表后的`const`指出这些函数不能修改类对象的状态；`const`和`非const`对象都可以调用`const成员函数`，但是`非const`成员函数只能被`非const`对象调用；
#### mutable
+ 可以将一个类成员定义成`mutable`，表示即使是在`const`对象中，也可以修改此成员`mutable bool cache`；
### 内置成员函数

+ 如果在类体内定义的成员函数中不包括循环等控制结构，C++系统自动的对它们作为内置函数处理，也就是说，在程序调用这些成员函数时，并不是真正的执行函数的调用过程(如保留返回地址等处理)，而是把函数代码嵌入程序的调用点，这样可以大大减少成员函数的时间开销；C++要求对一般的内置函数要用关键字inline声明，但对类内定义的成员函数，可以省略inline因为这些成员函数已被隐含的指定为内置函数；应该注意的是：如果成员函数不在类体内定义，而在类体外定义，系统并不把它默认为内置函数，调用这些成员函数的过程和调用一般函数的过程是相同的，如果想将这些成员函数指定为内置函数，应当用inline作显式声明；

```
class Student{
       public :
              inline void display();
};

inline void Student::display(){
       .......
}
```

+ 值得注意的是：如果在类体外定义inline函数，则必须将类定义和成员函数的定义放在同一个头文件中(或者写在同一个源文件中)，否则编译时无法进行置换；

### 成员函数的存储方式

+ 同一类的不同对象中的数据成员的值一般是不一样的，而不同对象的函数的代码是相同的，不论调用哪一个对象的函数的代码，其实调用的都是同样内容的代码，C++编译系统正是这样做的，因此每个对象所占用的存储空间只是该对象的数据成员所占用的存储空间，而不包括函数代码所占用的存储空间；函数的目标代码是存储在对象空间之外的，不论成员函数在类内定义还是在类外定义的，成员函数的代码段的存储方式是相同的，都不占用对象的存储空间，不论是用inline声明，成员函数的代码段都不占用对象的存储空间，inline函数只影响程序的执行效率，而与成员函数是否占用对象的存储空间无关；

### 成员指针与数据成员指针

```
struct C{
       const char* val;
       void print(int x){}
};
using Pmfi = void (C::*)(int);//C的成员函数指针，接收int
using Pm = const char* C::*;//C的数据成员的指针
void f(C& z){
       Pmfi pf = &C::print;
       Pm pm = &C::val;
       (z.*pf)(2);
       (z.*pm) = "newbi";
}
```
### 抽象类
+ 具有`一个`或`多个纯虚函数`的类称为`抽象类`，我们`无法创建抽象类的对象`，如果纯虚函数在派生类中未被定义，那么它仍保持是纯虚函数，因此派生类也是一个抽象类；
```
class Shape{
       public:
              virtual void rotate(int ) = 0;//纯虚函数
};
```
### 派生类
+ 实现继承：通过共享基类所提供的特性来减少实现工作量；接口继承：通过一个公共基类提供的接口允许不同派生类互换使用；`接口继承`通常被称为`运行时多态`或`动态多态`；`模版`所提供的类的通用性与继承无关，常被称为`编译时多态`或`静态多态`；

```
struct Employee{
       string first_name , family_name;
       char middle_initial;
       Date hiring_date;
       short department;
};

struct Manager : public Employee{//public公有继承
       list<Employee*> group;
       short level;
};
//多重继承
public A : public B , public C{}//多重继承如果两个基类中有相同名称的成员，在调用时会出现二义性，可以使用a1.B::a = 3;a1.C::a = 4;
```

+ `Manager`派生自`Employee`，除了自己成员`group`、`level`外，类`Manager`还拥有`Employee`的成员`first_name`、`family_name`等；派生一个类没有任何内存额外开销，所需要内存就是成员所需要空间；在子类转换成父类时可以使用隐式类型转换，但是父类转换成子类的时候，必须使用强制类型转换；通过指向基类对象的指针，只能访问派生类中的基类成员，而不能访问派生类增加的成员；

```
void g(Manager mm , Employee ee){
       Employee* pe = &mm;//正确，每个Manager都是一个Employee
       Manager* pm= &ee;//错误，并不是每个Employee都是一个Manager
       pm = static_cast<Manager*>(pe);//强制类型转换
}
```




