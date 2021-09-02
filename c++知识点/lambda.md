### lambda表达式

#### **1.介绍lambda**

一个lambda表达式表示一个可调用（就是可以使用()）的代码单元。结构如下所示：

```c++
[capture list](parameter list)->return type{function body}
//注意必须包含捕获列表和函数体
auto f = []{return 42;}
cout<<f()<<endl;//打印42
```

#### **2.向lambda传递参数**

lambda不能有默认参数，故一个lambda调用的实参数目永远与形参数目相等。

```c++
[](const string&a,const string& b){return a.size()<b.size();}
string a="123456";
//使用lambda的示例
stable_sort(a.begin(),a.end(),[](const string&a,const string& b){return a.size()<b.size();});
```

#### **3.使用捕获列表**

lambda只能使用明确指明的局部变量

```c++
//错误：sz未捕获
[](const string& a){return a.size()>=sz;};
```

注意：**一个lambda只有在其捕获列表中捕获一个它所在函数中的局部变量**，才能在函数体中使用该变量。

故：**捕获列表只用于局部非static变量**，lambda可直接使用局部static变量和在它所在函数之外声明的名字

#### **4.lambda捕获和返回**

当定义一个lambda时，编译器生成一个与lambda对应的新的（未命名的）类类型。

当向一个函数传递一个lambda时，**同时定义了一个新类型和该类型的一个对象；传递的参数就是此编译器生成的类类型的未命名对象。**

##### 4.1值捕获

采用值捕获的**前提是变量可以拷贝**。与参数不同被捕获的变量的值是**在lambda创建时拷贝，而不是调用时拷贝。**

注意**在lambda内被拷贝的值是一个右值，是不可修改的！**

```c++
void fcn1(){
	size_t v1 = 42;//局部变量
	//将v1拷贝为名为f的可调用对象，不可修改v1!
	auto f = [v1]{return v1;};
    v1 = 0;
    auto j = f();//j为42;f保存了我们创建它时v1的拷贝，故随后的修改不影响捕获
}
```

##### 4.2引用捕获

使用&实现引用捕获,使用的捕获对象的值为调用前该对象的值。

```c++
void fcn2(){
    size_t v1 = 42;//局部变量
	//对象f2包含v1的引用
	auto f = [&v1]{return v1;};
    v1 = 0;
    auto j = f2();//j为0;f2保存v1的引用，而非拷贝
}
//注意保证使用时，该变量在lambda执行时依然存在
```

引用捕获的一些重要意义：如**ostream对象是无法拷贝的，故我们只能捕获其引用。**

##### **4.3隐式捕获**

为了指示编译器推断捕获列表，应在捕获列表中写一个&或=。&表示引用捕获，=表示值捕获。

```c++
words = "asfr";
size_t sz = 3;
wc = find_if(words.begin(),words.end(),[=](const string& s)
             {return s.size()>=sz;});
```

当然也可以混用值捕获和参数捕获

```c++
void biggies (vector<string> &words,
		vector<string>::size_type sz,
		ostream &os = cout,char c = ''){
    //os隐式捕获，引用捕获方式；c显式捕获，值捕获方式
    for_each(words.begin(),words.end(),
             [&,c](const string& s){os<<s<<s;});
    //os显式捕获，引用捕获方式；c隐式捕获，值捕获方式
    for_each(words.begin(),words.end(),
             [=,&os](const string& s){os<<s<<s;});
}
```

注意：**混合使用的时候，捕获列表中的第一个元素必须是一个&或=，指定默认捕获方式**。

##### **4.4 可变lambda**

值拷贝一般不会影响被捕获的变量，如需影响，则需要主动添加**mutable**关键字。

```c++
void fcn3(){
	size_t v1 = 42;//局部变量
	//f可以改变它所捕获的变量的值，v1变为了一个左值。
	auto f = [v1]()mutable{return ++v1;};
    v1 = 0;
    auto j = f();//j为43;   
}
```

引用捕获中 v1是一个非const变量的引用

```c++
void fcn4(){
	size_t v1 = 42;//局部变量
	//f可以改变它所捕获的变量的值
	auto f = [&v1] {return ++v1;};
    v1 = 0;
    auto j = f();//j为1; 
}
```

##### **4.5指定lambda的返回类型**

当遇到需要告知编译器的时候，则必须显式说明返回值的类型。

```c++
vector<int> vi(12,-1);
transform(vi.begin(),vi.end(),vi.begin(),[](int i)->int
          {if(i<0)return -i;else return i;});
```

