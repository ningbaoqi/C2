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
```
map<string , int> phone_book = {
       {"david" , 123456},
       {"bob" , 234567},
       {"cacy" , 345678}
};
int get_number(const string& s){
       return phone_book[s];
}
```
### unordered_map
+ 标准库unordered_map为string提供了默认的哈希函数，查询速度相当快；
![image](https://github.com/ningbaoqi/C2/blob/master/gif/pic-4.jpg)

```
unordered_map<string , int> phone_book{
       {"david" , 123456},
       {"bob" , 234567},
       {"cacy" , 345678}
};
int get_number(const string& s){
       return phone_number[s];
}
```
### 集合概述

|标准库集合|概述|
|------|------|
|`Vector<T>`|可变大小向量|
|`List<T>`|双向链表|
|`forward_list<T>`|单向链表|
|`deque<T>`|双向队列|
|`set<T>`|集合|
|`mutiset<T>`|允许重复值的集合|
|`map<K,V>`|关联数组|
|`mutimap<K,V>`|允许重复关键字的map|
|`unordered_map<K,V>`|采用哈希搜索的map|
|`unordered_mutimap<K,V>`|采用哈希搜索的mutimap|
|`unordered_set<T>`|采用哈希搜索的set|
|`unordered_mutiset<T>`|采用哈希搜索的mutiset|

### 使用迭代器

```
bool has_c(const string& s , char c){
       return find(s.begin() , s.end() , c) != s.end;//find在一个序列中查找一个值，返回的结果是指向我们找到的元素的迭代器
}
```

#### 算法概述

|部分标准库算法|概述|
|------|------|
|`p = find(b , e , x)`|`p是[b : e)中第一个满足*p == x的迭代器`|
|`p = find_if(b , e , f)`|`p是[b : e)中第一个满足f(*p) == true的迭代器`|
|`n = count(b , e , x)`|`n是[b : e)中满足 *q == x的元素 *q的数目`|
|`n = count_if(b , e,  f)`|`n是[b : e)中满足f(*q) == true的元素*q的数目`|
|`replace(b , e, v, v2)`|`将[b : e)中满足*q == v的元素*q替换为v2`|
|`replace_if(b , e, f,  v2)`|`将[b:e)中满足f(*q) == true的元素*q替换为v2`|
|`p = copy(b , e , out)`|`将 [b : e)`拷贝到[out : p)|
|`p = copy_if(b , e, out , f )`|`将[b : e)中满足f(*q) == true的元素*q拷贝到[out : p)`|
|`p = unique_copy(b , e , out , f)`|`将[b : e)拷贝到[out : p)，不拷贝连续的重复的元素`|
|`sort(b , e)`|`排序[b : e)中的元素，用<作为排序标准`|
|`sort(b , e , f)`|`排序[b : e)中的元素，用谓词f作为排序标准`|
|`(p1 , p2) = equal_range(b , e, v)`|`[p1 : p2]是已经排序序列[b : e)的子序列，其中元素的值都等于v，本质上等价于二分搜索v`|
|`p = merge(b , e, b2 , e2 , out)`|`将两个序列[b: e)和[b1 : b2]合并，结果保存到[out : p)`|
