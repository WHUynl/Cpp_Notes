#### 0.STL是什么：

标准模板库（STL），它的基本概念就是**把数据和操作分离**，含有容器、算法、迭代器组件等。迭代器是容器和算法之间的粘合剂，使任何算法都可以和任何容器进行交互操作。

在STL中体现了泛型程序设计的思想，是以类型参数化的方式实现的（模板）。

STL中的容器：

序列容器：vector   string   deque   list 

关联容器：set   map   multiset   multimap （键值对，或者存在关系）

适配容器：stack   queue   priority_queue（优先队列）使用场景

**使用场景：**

1、如果你**需要高效的随机存取**，而**不在乎插入和删除的效率**，使用**vector**（**底层数据结构为数组 ，支持快速随机访问。**）
2、如果你**需要大量的插入和删除**，而**不关心随机存取**，则应使用**list(底层数据结构为双向链表，支持快速增删。)**
3、如果你**需要随机存取**，而且**关心两端数据的插入和删除**，则应使用**deque(底层数据结构为一个中央控制器和多个缓冲区，支持首尾（中间不能）快速增删，也支持随机访问)**。

4、如果你要**存储一个数据字典**，并要求方便地**根据key找value**，那么应该使用**map(底层数据结构为红黑树，有序，不重复)。**

5、如果你要**查找一个元素是否在某集合内存中**，则使用**set(底层数据结构为红黑树，有序，不重复)。**

#### 1：说说 **vector 和 list 的区别**

1） **vector， 连续存储的容器，动态数组，在堆上分配空间 ；**

底层实现：**数组**。

如果没有剩余空间了，**则会重新配置原有元素个数的两倍空间**，然后将原空间元素**通过复制的方式初始化新空间，再向新空间增加元素。**

适用场景：**经常随机访问，且不经常对非尾节点进行插入删除。**

2）**list，动态链表，在堆上分配空间，每插入一个元素都会分配空间，每删除一个元素都会释放空间。**

底层：**双向链表**

访问：**随机访问性能很差，只能快速访问头尾节点**。

适用场景：**经常插入删除大量数据**

2） vector在中间节点进行插入删除会导致内存拷贝，list不会。

3） vector一次性分配好内存，不够时才进行2倍扩容；list每次插入新节点都会进行内存申请。

4） **vector拥有一段连续的内存空间，因此支持随机访问，如果需要高效的随即访问，而不在乎插入和删除的效率，使用vector。**

**list拥有一段不连续的内存空间，如果需要高效的插入和删除，而不关心随机访问，则应使用list。**

#### **2 map 和 set 有什么区别**

1） map和set都是C++的**关联容器**，其底层实现都是**红黑树（RB-Tree）**。

2） map中的元素是key-value（关键字—值）对：关键字起到索引的作用，值则表示与索引相关联的数据；Set与之相对就是**关键字的简单集合**，set中每个元素只包含一个关键字。

3） **set的迭代器是const的**，不允许修改元素的值；**map允许修改value，但不允许修改key。**

4） map支持下标操作（m[key]），set不支持下标操作。

#### 3.unordered_map和map 说说区别

**内部实现机理**

- map： map**内部实现了一个红黑树**，该结构具有自动排序的功能，因此map**内部的所有元素都是有序的**，红黑树的每一个节点都代表着map的一个元素，因此，对于map进行的查找，删除，添加等一系列的操作都相当于是对红黑树进行这样的操作，**故红黑树的效率决定了map的效率。**
- unordered_map: unordered_map**内部实现了一个哈希表**，因此其元素的排列顺序是**杂乱的，无序的**

**优缺点以及适用处**

- - - map 
      - 优点: 
        - **有序性**，这是map结构最大的优点，其元素的有序性在很多应用中都会简化很多的操作
        - **红黑树**，内部实现一个红黑书使得map的很多操作在的时间复杂度下就可以实现，因此效率非常的高.
      - 缺点： 
        - **空间占用率高**，因为map内部实现了红黑树，虽然提高了运行效率，但是因为**每一个节点都需要额外保存父节点，孩子节点以及红/黑性质**，使得每一个节点都占用大量的空间
      - 
    - unordered_map 
      - 优点： 
        - **因为内部实现了哈希表，因此其查找速度非常的快**
      - 缺点： 
        - **哈希表的建立比较耗费时间**

#### **4.STL 中迭代器的作用，有指针为何还要迭代器**

1） Iterator（迭代器）模式又称Cursor（游标）模式，用于**提供一种方法顺序访问一个聚合对象中各个元素, 而又不需暴露该对象的内部表示。**

2） **迭代器不是指针，是类模板**，表现的像指针。他只是模拟了指针的一些功能，通过重载了指针的一些操作符，->、*、++、--等，相当于一种智能指针。迭代器**返回的是对象引用而不是对象的值**，所以cout只能输出迭代器使用*取值后的值而不能直接输出其自身。

3） 迭代器产生原因：

Iterator类的访问方式就是把不同集合类的访问逻辑抽象出来，**使得不用暴露集合内部的结构而达到循环遍历集合的效果。**

#### **5 STL 迭代器是怎么删除元素的呢**，什么时候会失效？

1） 对于序列容器**vector,deque**来说，使用erase(iterator)后，**后边的每个元素的迭代器都会失效**，后边每个元素都会往前移动一个位置.**但是erase会返回下一个有效的迭代器**；

```c++
#include<iostream>
#include<vector>

using namespace std;

int main()
{
    vector<int> v = { 0,1,2,3,4,5,6,7,8,9, };
    for (auto it = v.begin(); it != v.end(); it++) {
        if (*it == 5) {
            auto itTmp = it + 2;
            auto eIt = v.erase(it);
            it = eIt;
            cout << *it << endl;//6
           // cout << *itTmp << endl;itTmp已经失效，故报错
        }
    }

    return 0;
}
```

2） 对于关联容器map set来说，使用了erase(iterator)后，**当前元素的迭代器失效，但是其结构是红黑树，删除当前元素的，不会影响到下一个元素的迭代器，所以在调用erase之前，记录下一个元素的迭代器即可。**

```c++
#include<iostream>
#include<set>
using namespace std;

int main()
{
    set<int> s = {0,1,2,3,4,5,6,7,8,9,};
    for (auto it = s.begin(); it != s.end(); it++) {
        if (*it == 5) {
            auto eIt = s.erase(it);//记录下一个即可
            it = eIt;
            cout << *it << endl;
        }
    }

    return 0;
}
```

3） 对于list来说，它使用了不连续分配的内存，**并且它的erase方法也会返回下一个有效的iterator**。

```c++
#include<iostream>
#include<list>
using namespace std;

int main()
{
    list<int> l = {0,1,2,3,4,5,6,7,8,9,};
    for (auto it = l.begin(); it != l.end(); it++) {
        if (*it == 5) {
            auto eIt = l.erase(it);//记录下一个即可
            it = eIt;
            cout << *it << endl;
        }
    }

    return 0;
}
```



#### **6 栈溢出的原因**

栈溢出指的是**程序向栈中某个变量中写入的字节数超过了这个变量本身所申请的字节数。**

1） **局部数组过大。**当函数内部的数组过大时，有可能导致堆栈溢出，局部变量是存储在栈中的。

2） **递归调用层次太多。**

3） **指针或数组越界。**例如进行字符串拷贝，或处理用户输入等等。

#### **7 哈希表（hash表）**

**采用散列技术将记录存储在一块连续的存储空间中，这块连续空间称为散列表或哈希表(Hash-Table)。**

哈希表的实现主要包括构造哈希和处理哈希冲突：**构造哈希，主要包括直接地址法，除留余数法。**

处理哈希冲突：当哈希表关键字集合很大时，关键字值不同的元素可能会映射到哈希表的同一地址上，这样的现象称为哈希冲突。常用的解决方法有：

1） 开放定址法，冲突时，**用某种方法继续探测哈希表中的其他存储单元，直到找到空位置为止。（如，线性探测，平方探测）**

2） 再哈希法：当发生冲突时，**用另一个哈希函数计算地址值，直到冲突不再发生。**

3） 链地址法：**将所有哈希值相同的key通过链表存储，key按顺序插入链表中。**

#### 8**平衡二叉树（AVL树）和红黑树**

1）平衡二叉树又称为AVL树，是一种特殊的二叉排序树。**其左右子树都是平衡二叉树，且左右子树高度之差的绝对值不超过1。**

2）红黑树是**一种二叉查找树**，**但在每个节点增加一个存储位表示节点的颜色，可以是红或黑**（非红即黑），红黑树是一种**弱平衡二叉树**，相对于要求严格的AVL树来说，它的旋转次数少，所以对于搜索，插入，删除操作较多的情况下，通常使用红黑树。

3）所以红黑树在**查找，插入删除的性能都是O(logn)**，且性能稳定，所以STL里面很多结构包括map底层实现都是使用的红黑树。

#### 9 allocator空间适配器

**两种C++类对象实例化方式的异同**

 在c++中，创建类对象一般分为两种方式：一种是直接利用构造函数,直接构造类对象，如 Test test()；另一种是通过new来实例化一个类对象，如 Test *pTest = new Test；那么，这两种方式有什么异同点呢？

那么，从**内存空间分配的角度**来对这两种方式的区别，就比较容易区分:

（1）对于第一种方式来说，是直接通过调用Test类的构造函数来实例化Test类对象的,如果该实例化对象是一个局部变量，则其是在栈空间分配相应的存储空间。
（2）对于第二种方式来说,就显得比较复杂。这里主要以new类对象来说明一下。new一个类对象,其实是执行了两步操作：首先,**调用new在堆空间分配内存,然后调用类的构造函数构造对象的内容**；同样，使用delete释放时，也是经历了两个步骤：**首先调用类的析构函数释放类对象，然后调用delete释放堆空间。**

**C++ STL空间配置器实现:**

为了精密分工，STL allocator将两个阶段操作区分开来：

1.内存配置有alloc::allocate()负责，内存释放由alloc::deallocate()负责；

2.对象构造由::construct()负责，对象析构由::destroy()负责。

**空间的配置和释放**

SGI设计了双层级配置器:

第一级配置器**直接使用malloc()和free();**(请求的内存**大于**128b)

第二级配置器设计如下，采用不同的策略。(请求的内存**不大于**128b)

**第一级配置器相对简单**，因为使用的正是我们平常使用的malloc，dealloc，free等，如果调用不成功，则会调用oom_malloc()和oom_realloc()，这两个函数内循环调用“内存不足处理样例”，期望在某次调用之后，获得足够内存圆满完成任务。但是“内存不足处理样例”没有设置时，返回

**第二层配置器维护16个自由链表（free lists），负责16种小型区块的次配置能力。**有一个内存池，以malloc()配置而得。用一个union obj数组free_list来存储内存的地址，数组的每一个元素都指向一个obj链表，也就是内存链表。数组从小到大表示负责8b,16b,24b,...,120b,128b内存请求。**当请求的内存为n时，会将请求上条至2的指数大小，并从数组相应位置获取内存。**例如如果请求为20b，则请求会上调至24b 。（伙伴算法）
![img](https://img-blog.csdnimg.cn/20190415145516941.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x4aW5fbGl1,size_16,color_FFFFFF,t_70)

这个图展示了当有内存请求到达时，**先找到负责这个内存大小的数组元素指向的内存链表**，**取出第一块内存，然后把数组元素(obj指针)指向第二块内存的首地址**。

当程序释放这块内存时，第二级配置器还负责回收这块内存，等下次有请求时，可以直接使用这块内存。示意图如下，从《STL源码剖析》摘取:


![img](https://img-blog.csdnimg.cn/20190415145642431.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x4aW5fbGl1,size_16,color_FFFFFF,t_70)

**先计算这块内存属于哪个数组元素负责，然后将这块回收的内存放置链表的第一个位置，这块内存的下一块内存为这个链表原先的第一块内存。**

之前分析分配内存时，当free_list没有可用的内存时，会调用**refill**来从内存池分配内存。例如，如果请求内存为32b，此时内存链表中没有足够的内存了，那么refill会分配20块32b的内存块，然后把第一块返回给程序，其他19块由数组相应链表管理。

当free_list没有内存返回给用户时，refill函数会调用chunk_alloc从内存池获取内存，如果内存池剩余的内存(end_free-start_free)满足需求的内存(size*nobjs)，则直接从从内存池获取内存，返回给程序；当内存池的块不能满足20块时，返回一个以上的内存块；当内存池一块都不能满足时，先是回收剩余的内存，然后调用malloc从系统获取内存。如果系统内存不足，则先从数组其他链表获取内存，如果其他链表也不足够内存的话，就调用第一级内存来分配，因为第一级内存分配失败，有处理函数来解决。

#### 10. 请说说 STL 的基本组成部分

 标准模板库STL主要由6大组成部分：

1. 容器(Container)

    是一种数据结构， 如list, vector, 和deques，以模板类的方法提供。为了访问容器中的数据，可以使用由容器类输出的迭代器。

2. 算法（Algorithm）

    是用来操作容器中的数据的模板函数。例如，STL用sort()来对一 个vector中的数据进行排序，用find()来搜索一个list中的对象， 函数本身与他们操作的数据的结构和类型无关，因此他们可以用于从简单数组到高度复杂容器的任何数据结构上。

3. 迭代器（Iterator）

    提供了访问容器中对象的方法。例如，可以使用一对迭代器指定list或vector中的一定范围的对象。 迭代器就如同一个指针。事实上，C++ 的指针也是一种迭代器。 但是，迭代器也可以是那些定义了operator*()以及其他类似于指针的操作符方法的类对象;

4. 仿函数（Function object）

    仿函数又称之为函数对象， 其实就是重载了操作符的struct,没有什么特别的地方。

5. 适配器（Adaptor）

    简单的说就是一种接口类，专门用来修改现有类的接口，提供一中新的接口；或调用现有的函数来实现所需要的功能。主要包括3中适配器Container Adaptor、Iterator Adaptor、Function Adaptor。

6. 空间配制器（Allocator）

    为STL提供空间配置的系统。其中主要工作包括两部分：

   （1）对象的创建与销毁；

   （2）内存的获取与释放。

#### 11.STL 中 resize 和 reserve 的区别

**参考回答**

1. 首先必须弄清楚两个概念：

   （1）capacity：该值在容器初始化时赋值，指的是**容器能够容纳的最大的元素的个数**。还不能通过下标等访问，因为此时容器中还没有创建任何对象。

   （2）size：指的是此时**容器中实际的元素个数**。可以通过下标访问0-(size-1)范围内的对象。

1. resize和reserve区别主要有以下几点：

   （1）**resize既分配了空间，也创建了对象**；**reserve表示容器预留空间，但并不是真正的创建对象**，需要通过insert（）或push_back（）等创建对象。

   （2）resize既修改capacity大小，也修改size大小；reserve只修改capacity大小，不修改size大小。

   （3）两者的形参个数不一样。 **resize带两个参数**，一个表示容器大小，一个表示初始值（默认为0）；**reserve只带一个参数**，表示容器预留的大小。

**答案解析**

 **问题延伸：**

 resize 和 reserve 既有差别，也有共同点。两个接口的**共同点**是**它们都保证了vector的空间大小(capacity)最少达到它的参数所指定的大小。**下面就他们的细节进行分析。

 为实现resize的语义，resize接口做了两个保证：

 （1）保证区间[0, new_size)范围内数据有效，如果下标index在此区间内，vector[indext]是合法的；

 （2）保证区间[0, new_size)范围以外数据无效，如果下标index在区间外，vector[indext]是非法的。

 **reserve只是保证vector的空间大小(capacity)最少达到它的参数所指定的大小n。**在区间[0, n)范围内，如果下标是index，vector[index]这种访问有可能是合法的，也有可能是非法的，视具体情况而定。

 以下是两个接口的源代码：

```c++
void resize(size_type new_size)

   { 
           resize(new_size, T());
   }
  void resize(size_type new_size, const T& x)
   {
        if (new_size < size()) 
            // erase区间范围以外的数据，确保区间以外的数据无效
              erase(begin() + new_size, end()); 
        else
            // 填补区间范围内空缺的数据，确保区间内的数据有效
              insert(end(), new_size - size(), x); 
   }


#include<iostream>
#include<vector>
using namespace std;
int main()
{
    vector<int> a;
    cout<<"initial capacity:"<<a.capacity()<<endl;
    cout<<"initial size:"<<a.size()<<endl;

    /*resize改变capacity和size*/
    a.resize(20);
    cout<<"resize capacity:"<<a.capacity()<<endl;
    cout<<"resize size:"<<a.size()<<endl;


    vector<int> b;
     /*reserve改变capacity,不改变resize*/
    b.reserve(100);
    cout<<"reserve capacity:"<<b.capacity()<<endl;
    cout<<"reserve size:"<<b.size()<<endl;
return 0;
}

/*    运行结果：
          initial capacity:0
        initial size:0
        resize capacity:20
        resize size:20
        reserve capacity:100
        reserve size:0
*/    
```

 **注意：**如果**n大于当前的vector的容量**(是容量，并非vector的size)，将会引起**自动内存分配**。所以**现有的pointer,references,iterators将会失效。**而内存的重新配置会很耗时间。

#### 12.说说 STL 容器动态链接可能产生的问题？

1. 可能产生 的问题

    容器是一种动态分配内存空间的一个变量集合类型变量。在一般的程序函数里，局部容器，参数传递容器，参数传递容器的引用，参数传递容器指针都是可以正常运行的，而在动态链接库函数内部使用容器也是没有问题的，但是**给动态库函数传递容器的对象本身，则会出现内存堆栈破坏的问题。**

2. 产生问题的原因
   容器和动态链接库相互支持不够好，动态链接库函数中使用容器时，**参数中只能传递容器的引用，并且要保证容器的大小不能超出初始大小，否则导致容器自动重新分配，就会出现内存堆栈破坏问题。**

#### 13. push_back 和 emplace_back 的区别

**参考回答**

 如果要将一个临时变量push到容器的末尾，**push_back()需要先构造临时对象，再将这个对象拷贝到容器的末尾**，而**emplace_back()则直接在容器的末尾构造对象**，这样就**省去了拷贝和临时对象的析构过程。**

 参考代码：

```c++
#include <iostream>
#include <cstring>
#include <vector>
using namespace std;

class A {
public:
    A(int i){
        str = to_string(i);
        cout << "构造函数" << endl; 
    }
    ~A(){}
    A(const A& other): str(other.str){
        cout << "拷贝构造" << endl;
    }

public:
    string str;
};

int main()
{
    vector<A> vec;
    vec.reserve(10);
    for(int i=0;i<10;i++){
        vec.push_back(A(i)); //调用了10次构造函数和10次拷贝构造函数,
//        vec.emplace_back(i);  //调用了10次构造函数一次拷贝构造函数都没有调用过
    }
```

