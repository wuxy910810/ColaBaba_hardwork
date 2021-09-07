 工欲善其事，必先利其器。刷题从掌握常用STL 容器开始。	
# 0 概念
STL（Stand Template Library），标准模板库。将数据和操作分离，数据由容器进行管理，操作由算法进行，迭代器是两者自检的粘合剂，使任何算法和任何容器交互操作。
 	容器（Containers） + 迭代器（Iterators） +  算法（Algorithms）
 	容器：管理某类对象的集合。
 	迭代器：在一个对象集合的元素上进行遍历的动作。这个对象集合可以是容器，也可以是容器的一部分。
 	算法：处理对象集合中的元素，比如：sort，search，copy，erase，find, insert等。

# 1 容器
## 1.1 STL容器分类 - 两类
-  **顺序（序列）容器**：**vector**，list，deque, **string**，**stack**( 适配器类)，**queue**( 适配器类), priority queues( 适配器类)
-  **关联容器**：**set**，multiset，**map**，multimap，bitset，hash_set，hash_map，hash_multiset， hash_multimap

## 1.2 vector

``` C++
#include<vector>
// 1. 声明和定义
// 一维数组
vector<int> vec;        //声明一个int型向量
vector<int> vec(5);     //声明一个初始大小为5的int向量
vector<int> vec(10, 1); //声明一个初始大小为10且值都是1的向量
vector<int> vec(tmp);   //声明并用tmp向量初始化vec向量
vector<int> tmp(vec.begin(), vec.begin() + 3); //用向量vec的第0个到第2个值初始化tmp
/* 将arr数组的元素用于初始化vec向量
 * 说明：当然不包括arr[4]元素，末尾指针都是指结束元素的下一个元素，
 * 这个主要是为了和vec.end()指针统一。
 */
int arr[5] = {1, 2, 3, 4, 5};   
vector<int> vec(arr, arr + 5);      //将arr数组的元素用于初始化vec向量
vector<int> vec(&arr[1], &arr[4]);  //将arr[1]~arr[4]范围内的元素作为vec的初始值

// 二维数组
vector<vector<int>> vec(10, vector<int>(9, 0)); // 初始化一个10行 9列的二维数组

// 2. 基本操作
vec.size(); // 元素个数
vec.empty(); // return: TURE or FALse
vec.push_back(1); // 末尾插入
vec.pop_back(); // 末尾删除
vec.clear() // 清空所有元素
vec.erase(vec.begin() + 5); // 删除指定元素, 删除第六个元素，参数只能是迭代器

// 3. 遍历
vector<int>::iterator it;
for (it = vec.begin(); it != vec.end(); it++) {
    cout << *it << endl;
    it = vec.erase(it);// 正确，遍历删除
    // vec.erase(it); // 错误
    // vec.erase(it++); // 正确，erase()返回的是删除的迭代器的下一个
}
for (int i = 0; i < vec.size(); i++) {
    cout << vec.at(i) << endl; // 会检查越界
    cout << vec[i] << endl; // 不会检查是否越界
}
// 4. 查找、删除
vector<int>::iterator it = std::find(vec.begin(), vec.end(), value);
if (it != vec.end) {
    // 找到
    a.erase(it);
} else {
    // 未找到
}
```

## 1.3 stack

``` C++
// stack:先进后出，不支持遍历，没有迭代器
template <class T, class Container = deque<T> > class stack;
// T 指定存放的数据类型
// Container 指定栈的内存实现方式，可以是vector,deque,list，默认是deque。
stack<int> st;
st.empty(); // 堆栈为空则返回真
st.pop();   // 移除栈顶元素
st.push(8);  // 在栈顶增加元素
st.size();
st.top();   // 返回栈顶元素，只能通过top还访问栈顶元素
while (!st.empty) {
    st.pop();
}
```

## 1.3 string

```C++
#include <cstring>
// 数字转化为string
string to_string (int val); //int,long,long long,unsinged, float.etc.
// 字符串转化为int
int stoi (const string&  str, size_t* idx = 0, int base = 10);  // idx，表示从 base:表示当前string代表是什么进制。

string strNum = "123";
string strNum1 = "A";
int num = atoi(strNum.c_str()); // 只能识别10进制, num = 123
long int num = strol(strNum2.c_str(), nullptr, 16); // 可以识别其他进制, num = 10
int num = stoi(strNum); // num = 123

strNum.size();
strNum.find("1"); // 未找到，则返回std::string::npos或者-1
string s="abcdefg";
//s.substr(pos1,n)返回字符串位置为pos1后面的n个字符组成的串
string s2=s.substr(1,5);//bcdef
//s.substr(pos)//得到一个pos到结尾的串
string s3=s.substr(4);//efg
//如果输入的位置超过字符的长度，会抛出一个out_of_range的异常
```

## 1.4.1 map

```C++
map<int, int> myMap; // map中的元素是自动按Key升序排序，所以不能对map用sort函数
// 内部是红黑数，key有序

// 0.赋值, 三种方式
myMap.insert(pair<int, int>(1, 2));
myMap.at(1) = 2;
myMap[1] = 2;

myMap.size();
myMap.empty();

// 1. 遍历
map<int, int>::iterator iter;  
for(iter = myMap.begin(); iter != myMap.end(); iter++) {
	cout<<iter->first<<' '<<iter->second<<endl; 
    if (iter->first == 2) {
        myMap.erase(iter++); // 正确, 在执行erase之前，iter已经被加1了。erase会使得以前那个未被加一的iter失效，而加了一之后的新的iter是有效的。
        // 与以下代码等价
        /*
        map<int, int>::iterator iterTemp = iter; 
		++iter; 
		mapData.erase(iterTemp);
		*/
        // iter = myMap.erase(iter); // 正确
        // myMap.erase(iter); iter++; // 错误，iter已经失效，后续再++ 也是无效
    } else {
        iter++;
    }
} 
// 2. 遍历删除
for(iter = myMap.begin(); iter != myMap.end(); iter++) {
	cout<<iter->first<<' '<<iter->second<<endl; 
} 
// 3. 查找
iter = myMap.find(2);
if(iter != myMap.end()) {
   cout<<"Find, the value is "<<iter->second<<endl;   
} else {
    // 未找到
}
// 4. 删除
// 如果要删除1,用迭代器删除  
iter = myMap.find(1);  
myMap.erase(iter);  

// 如果要删除1，用关键字删除  
int n = myMap.erase(1);// 如果删除了会返回1，否则返回0  
myMap.clear(); // 删除所有元素
// 遍历删除
```

## 1.4.2 unordered_map

``` c++
unordered_map<int, int> mp; // 无序的
// 内部是哈希表， 无序
// 使用方法参考map
```

## 1.5.1 queue

```C++
queue<int> myQueue; // 先进先出
myQueue.size();
myQueue.empty();
myQueue.front(); // 第一个元素
myQueue.back(); // 最后一个元素
myQueue.push(1); // 从末尾添加元素
myQueue.pop(); //清除第一个元素

queue<pair<int, int>> que;
que.emplace(1, 2); // 插入元素
while (!que.empty) {
 	int a = que.front.first();
	int b = que.front.second();
    que.pop();
}

```

## 1.5.2 priority_queue

``` c++
priority_queue<std::string> que; // 最高优先级先出， 默认大顶堆，降序
// 优先队列，元素具有优先级，按照一定的规则有序，最高优先级的元素先删除
// 用法参考queue

//升序队列，小顶堆
priority_queue <int,vector<int>,greater<int> > que;
//降序队列，大顶堆
priority_queue <int,vector<int>,less<int> > que;

//greater和less是std实现的两个仿函数（就是使一个类的使用看上去像一个函数。其实现就是类中实现一个operator()，这个类就有了类似函数的行为，就是一个仿函数类了

// 规则：pair的比较，先比较第一个元素，第一个相等比较第二个。
priority_queue<pair<int, int> > a;
pair<int, int> b(1, 2);
pair<int, int> c(1, 3);
pair<int, int> d(2, 5);
a.push(d);
a.push(c);
a.push(b);
while (!a.empty()) {
    cout << a.top().first << ' ' << a.top().second << '\n';
    a.pop();
}
2 5
1 3
1 2

// 用自定义类型做优先队列元素的例子
//方法1
struct tmp1 //运算符重载<
{
    int x;
    tmp1(int a) {x = a;}
    bool operator<(const tmp1& a) const
    {
        return x < a.x; //大顶堆
    }
};
//方法2
struct tmp2 //重写仿函数
{
    bool operator() (tmp1 a, tmp1 b)
    {
        return a.x < b.x; //大顶堆
    }
};
int main()
{
    tmp1 a(1);
    tmp1 b(2);
    tmp1 c(3);
    priority_queue<tmp1> d;
    d.push(b);
    d.push(c);
    d.push(a);
    while (!d.empty())
    {
        cout << d.top().x << '\n';
        d.pop();
    }
    cout << endl;

    priority_queue<tmp1, vector<tmp1>, tmp2> f;
    f.push(b);
    f.push(c);
    f.push(a);
    while (!f.empty())
    {
        cout << f.top().x << '\n';
        f.pop();
    }
}
3
2
1
 
3
2
1
```

## 1.5.3 deque

```c++

```





## 1.6.1 set

``` c++
set<int> mySet;
// 1. 关联容器，存储同一数据类型的数据类型。内部使用红黑树
// 2. set中每个元素的值是唯一的。
// 3. 有序的。（插入相同的元素，只存一个）
// 4. 每次insert之后，以前保存的iterator不会失效。
mySet.find(2);
mySet.erase(2); // 删除2，返回值1或者0，1:成功，0:删除元素不存在
mySet.empty();
mySet.clear(); // 清空
mySet.insert(2); // 插入2
mySet.count(2); // 值为2的个数，0 或者 1

// 1. 遍历
set<int>::iterator it;
for (it = mySet.begin(); it != mySet.end(); ++it) {
    // todo:
}
// 2. 遍历删除
for (it = mySet.begin(); it != mySet.end()) {
    if (*it == 2) {
        it = mySet.erase(it); // 正确，遍历删除
        // mySet.erase(it++); // 正确，erase()返回的是删除的迭代器的下一个
    } else {
        it++;
    }
}
// 2. 查找
if (mySet.find(2) != mySet.end()) {
    // 找到
} else {
    // 未找到
}
```

## 1.6.2 unordered_set

``` c++
unordered_set<int, int> mySet; // 无序的
// 内部是哈希表， 无序
// 使用方法参考set
```

## 1.7 二分查找



# 2 容器的异同

- **vector - 数组**
1. 封装了数组，使用连续内存存储。
2. 快速查找；但内部插入，删除效率低。
3. 随机访问方便，支持[]运算符和vector.at()。
4. 只能在vector的最后进行push和pop，不能在vector的头进行push和pop。
5. 节省空间。
6. 当动态添加的数据超过vector默认分配的大小时要进行整体的重新分配、拷贝与释放 
- **list - 双向链表**
1. 封装了链表，不支持[]运算符。
2. 查找慢；内存插入，删除效率高。
3. 在两端进行push和pop
4. 占相对于verctor占用内存多
5. 不能进行内部的随机访问，即不支持[ ]操作符和vector.at()
- **deque - 双端队列**
1. deque是在功能上合并了vector和list。
2. 随机访问方便，即支持[ ]操作符和vector.at()
3. 在内部方便的进行插入和删除操作
4. 可在两端进行push、pop
5. 占用内存多
- **vector、list、deque 使用区别：**
1. 如果你需要高效的随即存取，而不在乎插入和删除的效率，使用vector 
2. 如果你需要大量的插入和删除，而不关心随即存取，则应使用list 
3. 如果你需要随即存取，而且关心两端数据的插入和删除，则应使用deque
- **map**
1. 属于标准关联容器，使用高效的平衡检索二叉树：**红黑树**。即封装了二叉树，
2. 插入键值的元素不允许重复，比较函数只对元素的键值进行比较，元素的各项数据可通过键值检索出来。
3. key是有顺序的，默认从小到大。
4. map中的元素是自动按Key升序排序，**所以不能对map用sort函数**；
- **set**
1. 关联容器，存储同一数据类型的数据类型。
2. set中每个元素的值是唯一的。有序的。（插入相同的元素，只存一个）
3. 每次insert之后，以前保存的iterator不会失效。
- **map和set的相同点：**
1. 都属于标准关联容器，使用高效的平衡检索二叉树：红黑树
2. 插入删除效率高：不需要内存拷贝和内存移动，直接替换指向结点的指针。
- **map和set的区别：**
1. set值只含有key，map有key和value。
- **set和vector区别**
1. set不包含重复的数据

- **map和hash_map的区别：**
1. hash_map使用hash算法来加快查找过程，但需要更多的内存来存放这些hash桶元素。（空间换时间）

（1）为何map和set的插入删除效率比用其他序列容器高？
大部分人说，很简单，因为对于关联容器来说，不需要做内存拷贝和内存移动。说对了，确实如此。set容器内所有元素都是以节点的方式来存储，其节点结构和链表差不多，指向父节点和子节点。结构图可能如下：
　　A
　 / \
　 B C
　/ \ / \
 D E F G
因此插入的时候只需要稍做变换，把节点的指针指向新的节点就可以了。删除的时候类似，稍做变换后把指向删除节点的指针指向其他节点也OK了。这里的一切操作就是指针换来换去，和内存移动没有关系。
（2）为何每次insert之后，以前保存的iterator不会失效？
iterator这里就相当于指向节点的指针，内存没有变，指向内存的指针怎么会失效呢(当然被删除的那个元素本身已经失效了)。相对于vector来说，每一次删除和插入，指针都有可能失效，调用push_back在尾部插入也是如此。因为为了保证内部数据的连续存放，iterator指向的那块内存在删除和插入过程中可能已经被其他内存覆盖或者内存已经被释放了。即使时push_back的时候，容器内部空间可能不够，需要一块新的更大的内存，只有把以前的内存释放，申请新的更大的内存，复制已有的数据元素到新的内存，最后把需要插入的元素放到最后，那么以前的内存指针自然就不可用了。特别时在和find等算法在一起使用的时候，牢记这个原则：不要使用过期的iterator。
（3）当数据元素增多时，set的插入和搜索速度变化如何？
如果你知道log2的关系你应该就彻底了解这个答案。在set中查找是使用二分查找，也就是说，如果有16个元素，最多需要比较4次就能找到结果，有32个元素，最多比较5次。那么有10000个呢？最多比较的次数为log10000，最多为14次，如果是20000个元素呢？最多不过15次。看见了吧，当数据量增大一倍的时候，搜索次数只不过多了1次，多了1/14的搜索时间而已。你明白这个道理后，就可以安心往里面放入元素了。