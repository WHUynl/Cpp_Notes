模板将声明和定义均写在头文件中，文件后缀名可以更改为hpp

#### 1. 说说模板类是在什么时候实现的

1. 模板实例化：模板的实例化分为**显示实例化**和**隐式实例化**，前者是研发人员明确的告诉模板应该使用什么样的类型去生成具体的类或函数，后者是在编译的过程中由编译器来决定使用什么类型来实例化一个模板不管是显示实例化或隐式实例化，最终生成的类或函数完全是按照模板的定义来实现的
2. 模板具体化：当模板使用某种类型类型实例化后生成的类或函数不能满足需要时，可以考虑对模板进行具体化。具体化时可以修改原模板的定义，当使用该类型时，按照具体化后的定义实现，具体化相当于对某种类型进行特殊处理。
3. 代码示例：

```c++
#include <iostream>
using namespace std;

// #1 模板定义
template<class T>
struct TemplateStruct
{
    TemplateStruct()
    {
        cout << sizeof(T) << endl;
    }
};

// #2 模板显示实例化
template struct TemplateStruct<int>;

// #3 模板具体化
template<> struct TemplateStruct<double>
{
    TemplateStruct() {
        cout << "--8--" << endl;
    }
};

int main()
{
    TemplateStruct<int> intStruct;
    TemplateStruct<double> doubleStruct;

    // #4 模板隐式实例化
    TemplateStruct<char> llStruct;
}
```

#### 2. C++ 11新特性：可变参数模板

在了解模板函数和模板类之前，我们需要先知道两个概念：

- 模板参数包：**使用 *typename/class ... Args* 来指出 Args 是一个模板参数包**，其中可能包含了零个或多个模板参数
- 函数参数包：**使用 *Args ... rest* 来指出 rest 是一个函数参数包**，其中可能包含了零个或多个函数参数

```c++
template <typename ... Args>
void func(Args& ... rest){
    /* 函数体 */
}
```

与一般的模板相同，当编译器遇到可变参数模板函数的调用时，会根据调用时所传递的实参来推断模板参数类型以及包中参数的数目。例如：

```c++
int i = 10;
double pi = 3.14;
string str = "hello world!";
func(i, pi, str);   //包中含有 3 个参数
func(pi, str);      //包中含有 2 个参数
func(str);          //包中含有 1 个参数
func();             //包中含有 0 个参数（空包）
```

根据上述调用方式，编译器会为 func 实例化出以下四个不同版本

```c++
void func(int&, double&, string&);
void func(double&, string&);
void func(string&);
void func();
```

#### 包扩展

对于一个可变参数函数模板而言，我们无法直接获取参数包中的参数，只能通过展开参数包的方式来访问参数包中的所有参数。C++ 中允许我们对参数包做执行两种操作：

- 获取包大小：通过 *sizeof...* 运算符获取参数包中的参数个数，例如：

```c++
template <typename ... Args>
void g(Args ... args){
    cout << sizeof...(Args) << endl;
    cout << sizeof...(args) << endl;
}
```

包扩展：**将一个包分解为构成的元素，并对每个元素应用模式，获得扩展后的列表**。C++ 通过在模式的右边加上一个省略号来触发包扩展，例如:

```c++
template <typename T, typename... Args>
ostream& print(ostream &os, const T &t, const Args&... rest){
  os << t << ", ";
  return print(os, rest...);
}
```

在 print 函数当中，我们进行了两次包扩展操作。**第一次扩展操作扩展了模板参数包 Args， 编译器将模式 const Args& 应用到了 Args 中的每个元素，最终的结果式得到了一个用逗号分隔的零个或多个类型的参数列表。**例如：

```c++
int i = 10;
double d = 3.14;
string str = "hello world";
print(cout, i, d, str); 
//将 ostrea& print(ostream&, const T&,const Args&...)扩展为: ostream& print(ostream &os, const int&, const double&, const string&);
```

第二次扩展则是发生在 print 的递归调用中，**此时模式为函数参数包的名字 rest。该模式扩展出一个由包中元素组成并用逗号分隔的列表，因此最终的效果相当于: print(cout, d, str)**

**使用递归则必须有终止，**上述的完整程序如下所示：

```c++
#include<iostream>
using namespace std;
//递归终止条件
template <typename T>
ostream& print(ostream& os, const T& t) {
    return os << t;
}  

template <typename T, typename... Args>
ostream& print(ostream& os, const T& t, const Args&... rest) {
    os << t << ", ";
    return print(os, rest...);
}

int main(int argc, char** argv)
{
    int i = 10;
    double d = 3.14;
    std::string str = "hello world";
    print(cout, i, d, str);
    return 0;
}
/*
结果
10, 3.14, hello world
*/
```

