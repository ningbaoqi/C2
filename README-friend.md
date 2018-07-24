### 友元
+ 友元可以访问与其有友好关系的类中的私有成员；
#### 友元函数
##### 将普通函数声明为友元函数

```
class Time{
      public:
            friend void display(Time& );
      private:
            int count ;
};
void display(Time& t){
      cout<<t.count<<endl;
}
```
##### 友元成员函数
```
class Date;
class Time{
       public:
              void display(Date&);
};
class Date{
       public:
              friend void Time::display(Date&);//声明Time类中的display函数为本类的友元成员函数
       private:
              int month;
};
```
#### 友元类
+ 不仅可以将一个函数声明为一个类的朋友，而且可以将一个类(如类B)声明为另一个类(如类A)的朋友；这时B类就是A类的友元类，友元类B中的所有函数都是A类的友元函数，可以访问A类中的所有成员；

```
friend class B
```
