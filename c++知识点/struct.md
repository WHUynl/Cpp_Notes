### 1.C++中struct与C中struct的区别，以及和C++中class的区别。

#### 1.C++与C中struct的区别：

A. **C是一种过程化**的语言，struct只是作为一种复杂数据类型定义，**struct中只能定义成员变量，不能定义成员函数**，同时C语言的struct中，**不能含有静态成员，不能继承，也不能直接初始化数据成员**，且**访问关系一定是public**。

```c
        struct Point
        {
                
        	int x; // 合法
            int y; // 合法
            
            static int a;//不合法
            int z = 0;//不合法
            void print(){//不合法
            	printf("Point print\n"); //编译错误
            }
        };
```



B. C是面向对象的语言，所以C++提供的struct**还有class的其他特性，例如继承、虚函数等**。故出现下面的情形，且struct**访问关系只是默认为public**

```c++
        struct Point
        {
        	//全部合法
        	int x; 
            int y; 
            static int a;
            int z = 0;
            void print(){
            	printf("Point print\n"); 
            }
            
            /*
            注意static int a =0是不合法的，C++98中只有const static intergal 和
            enum是可以直接类内初始化的，但C++11中也没有放宽 static int 的struct的初始化
            */
        };
```

如下表所示：

![](.\pics\struct1.png)

**使用时的区别：C 中使用结构体需要加上 struct 关键字**，或者**对结构体使用 typedef 取别名**，而 **C++ 中可以省略 struct 关键字**直接使用，例如

```c++
struct Student{//本名
    int  iAgeNum;
    string strName;
};
typedef struct Student Student2;    //C中取别名

struct Student stu1;    // C 中正常使用
Student2 stu2;            // C 中通过取别名的使用
Student stu3;            // C++ 中使用，省略stuuct
```

#### 2.C++中struct与class的区别

1. struct 一般用于描述一个**数据结构集合**，而 class 是对一个**对象数据的封装**；
2. **struct 中默认的访问控制权限是 public** 的，而 **class 中默认的访问控制权限是 private** 的，例如：

```c++
struct A{
    int iNum;    // 默认访问控制权限是 public
}
class B{
    int iNum;    // 默认访问控制权限是 private
}
```

3. 在继承关系中，struct 默认是公有继承，而 class 是私有继承；

4. class 关键字可以用于定义模板参数，就像 typename，而 struct 不能用于定义模板参数，例如：

```c++
template<typename T, typename Y>    // 可以把typename 换成 class
int Func(const T& t, const Y& y) {
    //TODO
}
```

