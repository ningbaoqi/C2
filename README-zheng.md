### 并发
#### 任务和thread
+ 那些可以与其他计算并行执行的计算为任务，线程是任务在程序中的系统级表示，若要启动一个与其他任务并发执行的任务，可以构造一个`std::thread`并将任务作为它的实参；
```
void f();
struct F{
       void operator()();
};
void user(){
       thread t1 {f};//f()在单独的线程中执行
       thread t2 {F()};//F()()在单独的线程中执行
       t1.join();//等待t1完成
       t2.join();//等待t2完成
}
```
+ `join()`保证我们在线程完成后才退出`user()`，一个程序的所有线程共享单一地址空间，在这一点上线程与进程不同，进程间通常不直接共享数据，由于共享单一地址空间，因此线程间可通过共享对象相互通信，通常通过锁或其他防止数据竞争的机制来控制线程间通信；
```
void f(){cout<<"please";}
struct F{
       void operator()(){cout<<"please";}
};
```
+ 以上是一个典型的严重错误，在这里`f`和`F()`都使用了对象`cout`，但没有采用任何形式的同步措施，因此，输出结果将是不可预测的，而且程序每一次执行都可能得到不同的结果；
#### 传递参数
```
void f(vector<double>& v);//处理v的函数
struct F{
       vector<double>& v;
       F(vector<double>& vv) : v{vv} {}
       void operator()();
};
int main(){
       vector<double> some_vec {1 , 2 , 3 , 4 , 5 , 6 , 7 , 8, 9};
       vector<double> vec2 {10 , 11 , 12 , 13 , 14};
       thread t1 {f , some_vec};//f(some_vec)在一个单独线程中运行
       thread t2 {F{vec2}};//F(vec2)()在一个独立线程中运行
       t1.join();
       t2.join();
}
```
#### 返回结果
```
void f(const vector<double>& v , double* res);//从v获取输入，将结果输入*res
thread t1 {f , some_vec , &res1};//f(some_vec , &res1)在一个独立线程中执行
t1.join();
```
#### 共享数据
+ `thread`使用`lock()`操作来获取一个互斥对象；
```
mutex m ;//控制共享数据访问的mutex互斥对象
int sh;//共享的数据
void f(){
       unique_lock<mutex> lck {m};//获取mutex
       sh += 7;//处理共享数据
       //隐式释放mutex
}
```
+ `unique_lock`的构造函数获取了互斥对象(通过调用`m.lock()`)，如果另一个线程已经获取了互斥对象，则当前线程或等待直至那个线程完成对共享数据的访问，一旦线程完成了对共享数据的访问，`unique_lock`会释放`mutex`(通过调用`m.unlock()`)，互斥和锁机制在头文件`<mutex>`中提供；标准库提供了一个同时获取多个锁的操作，可以帮助避免死锁的发生；
```
void f(){
       unique_lock<mutex> lck1 {m1 , defer_lock};//推迟加锁，还未尝试获取mutex
       unique_lock<mutex> lck2 {m2 , defer_lock};
       unique_lock<mutex> lck3 {m3 , defer_lock};
       lock(lck1 , lck2 , lck3);//获取全部三个锁
       //处理共享数据
}//隐式释放所有mutex
```
+ `lock()`调用只有在获取了全部`mutex`实参后才会继续执行，当它持有`mutex`时，绝不会阻塞，当然也就不会导致死锁，`unique_lock`的析构函数保证了当`thread`离开作用域时`mutex`会被释放；
### 资源管理
#### unique_ptr和shared_ptr
+ 标准库提供了两种`智能指针`来管理自由存储上的对象；`unique_ptr`对应所有权唯一的情况，`shared_ptr`对应所有权共享的情况；这些智能指针最基本的作用是防止由于编程疏忽而造成的内存泄露；
### 正则表达式
```
regex pat (R"(\w{2}\s*\d{5}(-d{4})?)");//美国邮政编码模式：xxdddd-dddd及其变形
cout<<"pattern:"<<pat<<'\n';
int lineno = 0;
for(string line : getline(cin , line)){
       ++lineno;
       smatch matches;//用于存放匹配的字符串
       if(regex_search(line , matches , pat))//在一行字符串中查找符合模式pat的子串
              cout<<lineno<<";"<<matches[0]<<'\n';
}
```
+ `regex_search(line , matches , pat)`负责在读入的`line`中查找所有符合模式`pat`的子串，如果找到了，把他们存入`matches`中，如果没有找到，`regex_search(line , matches , pat)`返回`false`；
