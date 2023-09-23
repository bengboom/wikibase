# C++

变量 指针 引用 对象

## 保留关键字

INT_MAX IMT_MIN

## 内存

malloc -> new
dealloc -> delete

a++ 比 a=a+1 内存开销更小 字符串很长时效果明显

调用函数会有更大内存开销 简单操作用宏

## 指针

函数指针是函数代码所在的地址，但在c++里应当倾向于使用函数对象 lambda而非函数指针

例如iterate(数组A,函数B) 对数组中的每个元素调用函数B

又或者sort(,,cmp);  bool cmp(const type &a,const type &b){return a<b;} 就是函数指针

## 面向对象

多态 继承 虚函数

继承：基类和派生类（子类）

private public 控制外部访问权限
protected 特殊控制子类对基类访问权限

public private protected继承 子类分别能否访问？

虚函数 成员函数前标注virtual 允许子类重新实现 纯虚函数要求子类必须重新实现

虚表：子类隐藏的成员数组，标注每个虚函数具体指向哪一个实现版本
final:不允许子类重写 override:要求子类重写

运行时多态

## lambda 表达式

是一个匿名函数 可以捕获当前作用域的变量

e.g. auto cmp=[捕获变量](const type &a,const type &b){return a<b;}
