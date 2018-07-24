### 异常处理
+ 异常的概念可以帮助我们将信息从检测到错误的地方传递到处理该错误的地方；
```
void taskmaster(){
       try{
              auto result = do_task();
       }catch(Some_error){
              //执行对do_task是发生错误，处理该问题
       }
}
int do_task{
       if(){
              return result;
       }else{
              throw Some_error();
       }
}
```

+ 在`catch`块中使用`throw;`不带任何对象的`throw`表示重新抛出；`catch(...)`表示捕获任意异常；`current_exception()`把某一个线程的异常传递给另一个线程的处理程序；

```
try{
       //.......
}catch(.......){
       prom.set_exception(current_exception());
}
```

### 静态断言
+ `static_assert(4<=sizeof(int) , "intergers are too small")`检查整数的尺寸，如果`4<=sizeof(int)`不满足，将输出后面的字符串；通常情况下，`static_assert(A ,S)`的作用是当A不为`true`时，把S作为一条编译器错误信息输出；
### 程序
+ 所有C++实现都支持下面两个版本的`main()`；每个程序只能使用两者之一；`main()`返回的`int`作为程序执行的结果被传递给调用main函数的系统，`非0`返回值表示发生了一个错误；
```
int main(){......}
int main(int argc , char* argv[]){......}
```

### 程序终止

|程序终止方式|
|------|
|从main函数返回|
|调用exit()；会调用已构造的静态对象的析构函数|
|调用abort()；析构函数不会被调用|
|抛出一个未捕获的异常|
|违反noexcept|
|调用quick_exit()|

### 命名转换

|转换|含义|
|-------|-------|
|`static_cast`|执行关联类型之间的转换，如一种指针类型向同一类层次中其他指针类型的转换，或者整形向枚举类型的转换，或者浮点型向整型的转换，它还能行构造函数和转换运算符定义的类型转换|
|`reinterprt_cast`|处理非关联类型之间的转换，比如整型向指针的转换以及指针向另一个非关联指针类型的转换|
|`const_cast`|参与转换的类型仅在const修饰符及volatile修饰符上有所区别|
|`dynamic_cast`|执行指针或者引用向类层次体系的类型转换，并执行运行时检查|
#### dynamic_cast`
+ 运算符`dynamic_cast`接受两个运算对象，被`<`和`>`包围的一个类型和被`(`和`)`包围的一个指针或引用；dynamic_cast的真正用武之地是编译器无法确定类型转换正确性的情形，在此情况下，`dynamic_cast<T*>(p)`查看p指向的对象，如果对象的类型是类T，则`dynamic_cast`返回一个指向该对象的`T*`类型的指针，否则返回`nullptr`，如果`p`的值是`nullptr`，`dynamic_cast<T*>(p)`也会返回`nullptr`，注意，类型转换要求必须对可唯一标识的对象进行；`dynamic_cast`要求给定的指针或引用指向一个多态类型，即有虚函数；用`dynamic_cast`将`void*`转换成其他类型是不允许的；如果`dynamic_cast`的引用对象不是所期望的类型，将会抛出一个`bad_cast`异常；安全性比较高；
```
dynamic_cast<T*>(p);如果p是T*类型，或者是D*类型且T是D的基类，则得到的结果如同我们简单的将p赋予一个T*一样；
T* t = dynamic_cast<T*>(p);
T& t = dynamic_cast<T&>(p);
```
