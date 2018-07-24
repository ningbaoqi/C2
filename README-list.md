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
