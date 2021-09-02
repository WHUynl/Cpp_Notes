##### 1.const_cast

被用来将对象的常量性移除。**其是唯一有此能力的C++-style转型操作符**

##### 2、static_cast

用于各种隐式转换，比如非const转const，void*转指针等, static_cast能用于多态向上转化，如果向下转能成功但是不安全，结果未知；**但注意不能const转非const**

##### 3.dynamic_cast

用于动态类型转换。**只能用于含有虚函数的类**，用于类层次间的向上和向下转化，主要用来执行“安全向下转型”。**只能转指针或引用。**向下转化时，如果是非法的对于指针返回NULL，对于引用抛异常。

##### 4.reinterpet_cast

意图执行低级转型，实际动作可能取决于编译器，**这就是表示它不可移植**。例如将int*转为int。可以用于任意类型的指针之间的转换，**对转换的结果不做任何保证。**

**static_cast和const_cast的结合使用，使非const成员函数来调用const的实现**，例子如下。

```c++
#include <iostream>
using namespace std;
class TestCast {
public:
    int x=0;
    const int& getConstValue() const { return x; }
    int& getChangeableValue() { 
        //说明，使用const_cast去除掉返回结果的常量性。但同时使用static_cast来保证this转化为const从而调用getConstValue。
        return const_cast<int&>(
            static_cast<const TestCast&>(*this).getConstValue()
        ); 
    }
};
int main()
{
    TestCast t;
    int& num = t.getChangeableValue();
    num = 1;
    cout << t.getChangeableValue() << endl;//t的x变为了1
}
```

