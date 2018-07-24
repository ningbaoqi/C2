### Vector
+ 一个`Vector`就是一个给定类型元素的序列，`元素在内存中是连续存储的`；
```
struct Entry{
       string name;
       int number;
};
vector<Entry> phone_book = {
       {"david" , 1234567}, 
       {"karl" , 2345678}, 
       {"arthur" , 3456789}
};

void print_book(const vector<Entry>& book){
       for(int i = 0 ; i != book.size ; ++i){
              cout<<book[i]<<'\n';
       }
}
```
+ Vector最常用的一个操作就是`push_back()`它向`Vector`末尾追加一个新元素，从而将`Vector`的规模增大`1`；
```
void input(){
       for(Entry e : cin >> e){
              phone_book.push_back(e);
       }
}
```

### List
+ 标准库提供了一个名字为`List`的`双向链表`；如果希望在一个序列中添加和删除元素的同时无须移动其他元素，则应该使用List；
![image](https://github.com/ningbaoqi/C2/blob/master/gif/pic-2.jpg) 
```
List<Entry> phone_book = {
       {"david" , 123456},
       {"bob" , 234567},
       {"cacy" , 345678}
};
int get_number(const string& s){//这段代码从链表头开始搜索s，直到找到s或到达phone_book的末尾为止
       for(const auto& x : phone_book){
              if(x.name == s){
                     return x.number;
              }
       }
}
//每个标准库容器都提供了begin()和end()函数，分别返回一个指向首元素的迭代器和一个指向尾后位置的迭代器
int get_number(const string& s){
       for(auto p = phone_book.begin() ; p != phone_book.end();++p){
              if(p -> name == s)
                     return p -> number;
       }
       return 0;
}

//向List中添加元素以及从list中删除元素都很简单
void f(const Entry& ee , List<Entry>::iterator p , List<Entry>::iterator q){
       phone_book.insert(p , ee);//将ee添加到p指向的元素之前
       phone_book.erase(q);//删除q指向的元素
}
```
+ `Vector`无论是遍历如`find()`和`count()`性能还是排序和搜索`sort()`和`binary_search()`性能都优于`List`；
### map
+ `map`也被称为`关联数组`或`字典`，`map`通常用平衡二叉树实现；
![image](https://github.com/ningbaoqi/C2/blob/master/gif/pic-3.jpg)
