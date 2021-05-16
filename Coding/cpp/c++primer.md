<h2>函数指针<h2>
<h3>函数原型</h3>
函数的声明，包含函数名字，返回类型以及形参类型。
想调用某函数，在调用之前一定要有函数的声明
<h3>声明</h3>

- 声明一个函数指针只需要将指针名替换函数名
- ```c++
  Bool (*pf) (const string& const string&)
  ```
- pf是个指针，pf左侧是指向的函数返回值类型，右侧是指向的函数形参列表
- (*pf)的**括号必不可少**，如果没有括号 ```Bool *pf (const string& const string&)```就变成了一个函数，返回一个bool*

<h3>赋值</h3>

- pf=functionName
- pf = &functionName
- 可以为函数指针初始化为nullptr或者0
<h3>调用函数</h3>

1. pf(param1,param2);
2. (*pf)(param1,param2)

<h3>函数指针形参</h3>
函数指针形参可以有如下定义
```cpp
//1
void funcWithFuncPtr(void pf(char*));
//2
void funcWithFuncPtr(void (*pf)(char*));
```

这样看来声明定义一个带有函数指针的函数还是有些麻烦的，冗长的代码会造成阅读困难
我们可以使用类型别名和decltype来简化我们使用的代码
```c++
typedef void FuncType(int*);
typedef decltype(func2) func2Type;

cout << "func2Type:" << typeid(func2Type).name() << endl;
cout << "FuncType:"<< typeid(func2Type).name() << endl;
```
然而FuncType和Func2Type都是函数类型，我们需要在类型前加上*才可以得到函数指针类型，如下
```c++
typedef void (*FuncType)(int*);
typedef decltype(func2) *func2Type;

cout << "func2Type:" << typeid(func2Type).name() << endl;
cout << "FuncType:"<< typeid(func2Type).name() << endl;
```
**!!!decltype只会返回函数类型，不会将函数类型自动转换成指针类型**

<h3>函数指针返回值</h3>
```c++
int (*f1(int)) (int*,int);
```
我们可以从内向外看，f1有形参列表所以是一个函数，左侧有个*符号，说明返回值返回一个指针。再看外侧，该指针是个函数指针类型，因此该方法是返回一个函数指针

为了方便阅读我们使用类型别名的方式去定义函数指针返回值

```c++
int func(int*);
using F = int(int*);    //F是函数类型不是指针
typedef int F1(int*);
typedef decltype(func) F2;

using PF = int(*)(int*); //PF是指针类型
typedef int (*PF1)(int*);
typedef decltype(func) *PF2;


F* f1();
F1* f2();
F2* f3();

PF f4();
PF1 f5();
PF2 f6();
int main()
{
    int i = 0;
    int* ip = &i;
    cout << "f1:" << f1()(ip) << endl;
    cout << "f2:" << f2()(ip) << endl;
    cout << "f3:" << f3()(ip) << endl;
    cout << "f4:" << f4()(ip) << endl;
    cout << "f5:" << f5()(ip) << endl;
    cout << "f6:" << f6()(ip) << endl;
}

int func(int* ip) {
    static int i = 10;
    return ++i;
}

F* f1() {
    return func;
}
F1* f2() {
    return func;
}
F2* f3() {
    return func;
}

PF f4() {
    return func;
}
PF1 f5() {
    return func;
}
PF2 f6() {
    return func;
}

```
当然，我们还可以使用尾置返回类型
```c++
auto f1(int)->int(*)(int*);
```

<h3>返回数组的函数声明？？</h3>
<h3>尾置返回类型？？</h3>