

# C++

## 函数

函数的分文件编写：

- 创建.h后缀名的头文件
- 创建.cpp后缀名的源文件
- 在头文件中写函数的声明
- 在源文件中写函数的定义
- [视频](https://www.bilibili.com/video/BV1et411b73Z?p=55&spm_id_from=pageDriver)

### 函数提高

函数的形参列表中的形参是可以有默认值

返回值 函数名（形参=默认值）{}

#### 注意：

1、如果某个位置已经有了默认参数，那么从这个位置往后，从左往右都必须有默认值

2、如果函数声明有默认参数，函数实现就不能有默认参数；声明和实现只能有一个有默认参数

#### 占位参数

返回值类型 函数名（数据类型）{}

还可以有默认参数

#### 函数重载

函数名可以相同，提高复用性

满足条件：

- 同一作用域下
- 函数名称相同
- 函数参数类型不同，或者个数不同，或者顺序不同

注意：

- 函数的返回值不可以作为函数重载的条件

- 引用作为重载条件

  ```c++
  void func(int &a) // int &a = 10 不合法
  void func(const int &a) // const int &a = 10
  ```

  

- 函数重载不要设置函数默认参数 

  ```C++
  void func(int a, int b = 10)
  void func(int a)
  ```

  

## 指针

指针就是地址，指针内包含变量的存储地址

const修饰指针：常量指针，特点：指针的指向可以修改，但指针指向的值不可以修改 const int *p = &a

const修饰常量：指针常量特点：指针的指向不可以改，指针指向的值可以改 int * const p = &a 

都不可以改：const int * const p = &a 

const可以防止结构体误操作

数组名就是数组首地址

## 内存分区模型

C++程序在执行时，将内存分为4个区域：

- 代码区：存放函数体的二进制代码，由操作系统进行管理
  - 存放CPU执行的机器指令
  - 是共享只读的
- 全局区：存放全局变量和静态变量以及常量，结束后由操作系统回收释放
  - 静态变量：static
  - ![image-20220113220212975](../picture/image-20220113220212975.png)
  - 常量区中存放const修饰的全局常量和字符串常量
- 栈区：由编译器自动分配释放，存放函数的参数值，局部变量等
  - 不要返回局部变量的地址
  - ![image-20220113221127502](../picture/image-20220113221127502.png)
- 堆区：由程序员分配和释放，结束后由操作系统回收
  - 主要利用new在堆区开辟内存，new返回的是该数据类型的指针
  - ![image-20220113221843224](../picture/image-20220113221843224.png)![image-20220113221919432](../picture/image-20220113221919432.png)![image-20220113222703618](../picture/image-20220113222703618.png)
  - 释放利用操作符delete，释放数组delete[]

## 引用

作用：给变量起别名

语法：数据类型 &别名 = 原名

### 引用注意

- 引用一旦初始化后，就不可以更改
- 引用必须要初始化
- 引用的本质在C++内部实现是一个==指针常量==
- 引用必须引一块合法的内存空间

### 做函数参数

- ![image-20220114144014003](../picture/image-20220114144014003.png)
- 通过引用参数产生的效果同地址传递一致

### 做函数返回值

不要返回局部变量的引用

函数的调用可以作为左值

![image-20220114145723554](../picture/image-20220114145723554.png)

#### 常量引用

修饰形参，防止误操作

const int &val

## 类和对象

特性：封装、继承、多态 

### 封装的意义

- 将属性和行为作为一个整体
  - 类中的属性和行为统一称为成员
- 将属性和行为加以权限控制
  - 公共权限 public：成员类内外都可以访问
  - 保护权限 protected：成员类内可以访问，子类可以访问父类中的保护内容
  - 私有权限 private：成员类内可以访问，子类不可以访问父类中私有内容

### struct和class的区别

struct默认权限为公共，class默认权限为私有

### 成员属性设置为私有

优点：可以自己控制读写权限，对于写可以检测数据的有效性

```c++
class Person
{
public:
	void setName(string name)
    {
        m_name = name;
    }
    void getName()
    {
        cout << m_name << endl;
    }
    // 年龄可读可写，修改要合法
    int getAge()
    {
        return m_age;
    }
    void setAge(int age)
    {
        if (age<0 || age>150)
        {
            cout << "craze" << endl;
            return;
        }
        m_age = age;
    }
    void setLover(string name)
    {
        m_Lover = name;
    }
private:
    string m_name;
    int m_age;
    string m_lover;
};
```

### 对象的初始化和清理（难）

#### 构造函数

语法：类名(){}

1. 构造函数，没有返回值也不写void
2. 函数名称与类名相同
3. 构造函数可以有参数，因此可以发生重载
4. 程序在==调用对象时候==会自动调用构造，无需手动调用，而且只会调用一次

分类方式：

- 按参数：有参构造和无参构造

  ```c++
  class Person
  {
  public:
      Person()
      {
          return 0;
      }
  
      Person(int a)
      {
          return 0;
      }
  };
  ```

  

- 按类型：普通构造和拷贝构造

  ```c++
  class Person
  {
  public:
      int age;
      Person(int a)
      {
          age = a;
      }
      Person(const Person &p)
      {
          age = p.age;
      }
  };
  ```

  

#### 拷贝构造函数调用时机

1. 使用一个已经创建完毕的对象来初始化一个新对象
2. 值传递的方式给函数参数传值
3. 值方式返回局部对象

```c++
class Person
{
public:
    int m_age;
    Person()
    {
        cout << "默认构造" << endl;
    }
    
    Person(int age)
    {
        m_age = age;
        cout << "有参构造" << endl;
    }
    
    Person(const Person &p)
    {
        m_age = p.m_age;
        cout << "拷贝构造" << endl;
    }
    
    ~Person()
    {
        cout << "析构" << endl;
    }
};


```

调用规则：

1. 如果用户定义有参构造函数，c++不提供默认无参构造，但会提供默认拷贝构造
2. 如果用户定义拷贝构造函数，c++不会提供其他构造函数

调用方式：

- 括号法

  - 调用默认构造函数时候，不要加()

- 显示法

  - 不要利用拷贝构造函数初始化匿名对象

- 隐式转换

- [视频]: https://www.bilibili.com/video/BV1et411b73Z?p=107&amp;spm_id_from=pageDriver

  

#### 深拷贝与浅拷贝

浅拷贝：简单的赋值拷贝操作，存在问题是堆区的内存重复释放

深拷贝：在堆区重新申请空间，进行拷贝操作

如果属性有在堆区开辟的，一定要自己提供拷贝构造函数

#### 析构函数

语法：~类名(){}

1. 析构函数，没有返回值也不写void
2. 函数名称与类名相同，在名称前加上符号~
3. 析构函数不可以有参数，不能发生重载
4. 程序在==对象销毁前==会自动调用析构，无需手动调用，而且只会调用一次
5. 将堆区开辟的数据做释放操作

#### 初始化列表

用来初始化属性；语法：构造函数（）：属性1（值1），属性2（值2） ... {}

```c++
class Person
{
    Person(int a, int b, int c):m_a(a),m_b(b),m_c(c)
};

```

#### 类对象作为类成员

类中的成员可以作为另一个类的对象，称为对象成员

```c++
class Phone
{
public:
    string m_PName;
    Phone(string name)
    {
        m_PName = name;
    }
};

class Person
{
public:
    string m_name;
    Phone m_phone;
    
    Person(string name, string pname):m_name(name),m_phone(pname)
    {
        
    }
};

// 当其他类对象作为本类成员，构造时候先构造类对象，再构造自身，析构的顺序相反
```

#### 静态成员

所有对象共享同一个函数，静态成员函数只能访问静态成员变量，静态成员函数也有访问权限

```c++
class Person
{
public:
    static void func()
    {
        m_a = 100;
        cout << endl;
    }
    static int m_a;
};
int Person::m_a = 0; // 静态变量在外面需要声明

void test()
{
    //1、通过对象访问
    Person p;
    p.func();
    
    //2、通过类名访问
    Person::func();
}
```

### c++对象模型和this指针

成员变量和成员函数分开存储，空对象占用1字节的空间，只有非静态成员变量属于类的对象上

this是隐含每一个非静态成员函数内的一种指针，指向被调用的成员函数所属的对象；本质是指针常量，指针的指向不可以修改

```c++
class Person
{
public:
    int age;
    
    Person(int age)
    {
        //指向被调用的成员函数所属的对象
        this->age = age;
    }
    
    Person& PersonAddAge(Person &p)
    {
        this->age += p.age;
        return *this
    }
}

// 返回对象本身用*this
void test()
{
    Person p1(10);
    Person p2(10);
    p2.PersonAddAge(p1).PersonAddAge(p1);
}
```

空指针也是可以调用成员函数

#### const修饰成员函数

在成员函数后面加const，修饰this的指向，让指针指向的值也不可以修改

```c++
void showPerson() const
mutable int m_b; //加上mutable，在常函数中也可以修改
```

常对象：在声明对象前加const，只能调用常函数

常函数：成员函数前加const，其内部不可以修改成员属性

### 友元

实现：

- 全局函数做友元
- 类做友元
- 成员函数做友元

```c++
class Building
{
    friend void Goodgay::visit(); // 成员函数
    friend class Goodgay; // 类
    friend void goodgay(Building *building); // 全局函数
    // 可以访问私有成员
public:
    string m_settingroom;
    Building();
    
private:
    string m_bedroom;
};
// 类外写成员函数
Building::Building()
{
    m_sittingroom = "客厅";
    m_bedroom = "卧室";
};

// 全局函数
void goodgay(Building *building)
{
    cout << building->m_bedroom << endl;
}

class Goodgay
{
public:
    Building *building;
    
    void visit();
    
    goodgay();
};

Goodgay::goodgay()
{
    building = new Building;
}

void Goodgay::visit()
{
    cout << building->m_bedroom << endl;
}
void test()
{
    Building building;
    goodgay(&building);
    
    Goodgay gg;
    gg.visit();
}
```

### 运算符重载

对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

```c++
class Person
{
public:
    int m_a;
    int m_b;
    int *m_age;
    // 成员函数重载
    Person operator+(Person &p)
    {
        Person temp;
        temp.m_a = this->m_a + p.m_a;
        temp.m_b = this->m_b + p.m_b;
        return temp;
    }
    Person& operator=(Person &p)
    {
        if(m_age != NULL)
        {
            delete m_age;
            m_age = NULL;
        }
        m_age = new int(*p.m_age);
        return *this;
    }
};

//全局函数重载
Person operator+(Person &p1, Person &p2)
{
    Person temp;
    temp.m_a = p1.m_a + p2.m_a;
    temp.m_b = p1.m_b + p2.m_b;
    return temp;
}
ostream& operator<<(ostream &cout, Person &p)
{
    cout << p.m_a << p.m_b;
    return cout;
}


```

### 继承

减少重复代码

class 子类 : 继承方式 父类

![image-20220124161251460](../picture/image-20220124161251460.png)

父类中所有非静态成员属性都会被子类继承下去

继承中的构造和析构顺序：先构造父类，再构造子类，析构与构造的顺序相反

通过子类对象访问父类中同名成员、函数、静态成员需要加作用域

一个类可以继承多个类，实际开发不建议

利用虚继承解决菱形继承的问题：==virtual==

### 多态

静态多态：函数重载和运算符重载，复用函数名；函数地址早绑定（编译阶段确定函数地址）

动态多态：派生类和虚函数实现运行时多态；函数地址晚绑定（运行阶段确定函数地址）

动态多态满足条件：有继承关系、子类重写父类的虚函数

使用：父类的指针或者引用，执行子类对象

好处：组织结构清晰、可读性强、对于后期维护拓展有利

重写：函数返回值类型、函数名、参数列表 完全相同

当子类重写父类的虚函数，子类中的虚函数表内部会替换成子类的虚函数地址  [视频](https://www.bilibili.com/video/BV1et411b73Z?p=136&spm_id_from=pageDriver)

#### 纯虚函数

只要有一个纯虚函数，就称为抽象类

抽象类特点：无法实例化对象；抽象类的子类，必须重写父类的纯虚函数，否则也属于抽象类

#### 虚析构和纯虚析构

可以解决父类指针释放子类对象；都需要有具体的函数实现

纯虚析构需要声明也需要实现

## 文件

包含头文件<fstream>

```c++
// 读文件
#include <string>
#include <fstream>
// 第一种
char buf[1024] = {0};
while (ifs >> buf)
{
    cout << buf << endl;
}
// 第二种
char buf[1024] = {0};
while (ifs.getline(buf,sizeof(buf)))
{
    cout << buf << endl;
}
// 第三种
string buf;
while (getline(ifs,buf))
{
    cout << buf << endl;
}
```

使用完文件后要关闭

## 模板

模板就是建立通用的模具，提高复用性

### 函数模板语法

- 利用关键字template
- 使用函数模板方式：自动类型推导、显示指定类型
- 模板的目的是为了提高复用性，将类型参数化
- 自动类型推导，必须推导出一致的数据类型T才能使用
- 模板必须要确定出T的数据类型，才可以使用

```c++
template<class T>
void swap(T &a, T &b)
{
    T temp = a;
    a = b;
    b = temp;
}

swap(a,b);
swap<int>(a,b);
```

普通函数与函数模板的区别：

- 普通函数调用可以发生隐式类型转换
- 函数模板用自动类型推导，不可以发生隐式类型转换
- 函数模板用显示指定类型，可以发生隐式类型转换

普通函数与函数模板的调用规则：

- 如果函数模板和普通函数都可以实现，优先调用普通函数
- 可以通过空模板参数列表来强制调用函数模板
- 函数模板也可以发生重载
- 如果函数模板可以产生更好的匹配，优先调用函数模板

==提供函数模板就不用普通函数==

模板的通用性并不是万能的

### 类模板

和函数模板的区别：

- 类模板没有自动类型推导的使用方法，只能使用显示指定类型方式
- 类模板在模板参数列表中可以有默认参数

普通类中的成员函数一开始就可以创建，类模板中的成员函数在调用时才创建

通过类模板创建的对象进行传参，常用 指定传入的类型

类模板与继承：

- 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型，如果不指定，编译器无法给子类分配内存
- 如果需要灵活指定出父类中T的类型，子类也需要变为类模板

类模板成员函数类外实现

```C++
template<class T1, class T2>
Person<T1,T2>::Person(T1 name, T2 age)
{
    this->m_name = name;
    this->m_age = age;
}

template<class T1, class T2>
void Person<T1, T2>::showperson()
{
    
}
```

类模板分文件编写

类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到

解决：将声明和实现写到同一个文件中，并更改后缀名为.hpp

类模板与友元

全局函数类内实现：直接在类内声明友元

全局函数类外实现：需要提前让编译器知道全局函数的存在

## STL

六大组件：

- 容器：各种数据结构，如vector、list、deque、set、map等，用来存放数据。
- 算法：各种常用的算法，如sort、find、copy、for_each等
- 迭代器：扮演了容器与算法之间的胶合剂。
- 仿函数：行为类似函数，可作为算法的某种策略。
- 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西。
- 空间配置器：负责空间的配置与管理。

### STL中容器、算法、迭代器

#### 容器

STL容器就是将运用最广泛的一些数据结构实现出来

常用的数据结构：数组,链表,树,找,队列,集合,映射表等

这些容器分为序列式容器和关联式容器两种：
	序列式容器：强调值的排序，序列式容器中的每个元素均有固定的位置。
	关联式容器：二叉树结构，各元素之间没有严格的物理上的顺序关系

#### 算法

有限的步骤，解决逻辑或数学上的问题

算法分为：质变算法和非质变算法。

质变算法：是指运算过程中会更改区间内的元素的内容。例如拷贝，替换，删除等等

非质变算法：是指运算过程中不会更改区间内的元素内容，例如查找、计数、遍历、寻找极值等等

#### 迭代器

容量和算法之间粘合剂

提供一种方法，使之能够依次寻访某个容器所含的各个元素，而又无需暴露该容器的内部表示方式

```c++
#include <vector>
#include <algorithm> // 标准算法头文件
// vector<类型>::iterator

void print(int val)
{
    cout << val << endl;
}

int test()
{
    // 创建容器：数组
    vector<int> v;
    // 向容器中插入数据
    v.push_back(10);
    // 通过迭代器访问容器中的数据
    vector<int>::iterator itBegin = v.begin(); // 起始迭代器 指向容器中第一个元素
    vector<int>::iterator itEnd = v.end(); // 结束迭代器 指向容器中最后一个元素的下一个位置
    
    // 遍历方式
    // 1
    while (itBegin != itEnd)
    {
        cout << *itBegin << endl; // 类似指针使用
        itBegin++;
    }
    // 2
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
    {
        cout << *it << endl;
    }
    // 3
    for_each(v.begin(), v.end(), print);
    
}
```

### 容器嵌套容器

```c++
#include <vector>

void test()
{
	vector<vector<int>> v;
    // 创建小容器
    vector<int> v1;
    vector<int> v2;
    vector<int> v3;
    vector<int> v4;
    
    for (int i = 0; i<4; i++)
    {
        v1.push_back(i+1);
        v2.push_back(i+2);
        v3.push_back(i+3);
        v4.push_back(i+4);
    }
    
    v.push_back(v1);
    v.push_back(v2);
    v.push_back(v3);
    v.push_back(v4);
    // 通过大容器访问
    for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++)
    {
        for (vector<vector<int>>::iterator vit = (*it).begin; vit != (*it).end(); vit++)
        {
            cout << *vit << " ";
        }
        cout << endl;
    }
}
```

### string容器

string和char *区别：

- char * 是一个指针
- string是一个类，类内部封装了char *，是一个char *型的容器

![image-20220214105253048](../picture/image-20220214105253048.png)![image-20220214105205741](../picture/image-20220214105205741.png)![image-20220214105403567](../picture/image-20220214105403567.png)![image-20220214105509927](../picture/image-20220214105509927.png)![image-20220214105555958](../picture/image-20220214105555958.png)![image-20220214105618469](../picture/image-20220214105618469.png)![image-20220214105707830](../picture/image-20220214105707830.png)![image-20220214105729436](../picture/image-20220214105729436.png)

### vector容器

基本概念：

vector和数组相似；不同之处在于数组是静态空间，而vector可以动态拓展（寻找更大的内存空间，将原数据拷贝到新空间，释放原空间）

![image-20220214105014464](../picture/image-20220214105014464.png)![image-20220214105048528](../picture/image-20220214105048528.png)![image-20220214104924793](../picture/image-20220214104924793.png)![image-20220214110847964](../picture/image-20220214110847964.png)![image-20220214111524585](../picture/image-20220214111524585.png)![image-20220214111833546](../picture/image-20220214111833546.png)![image-20220214112813412](../picture/image-20220214112813412.png)

### deque容器

双端数组，可以对头端进行插入删除操作

与vector的区别：

- vector对于头部的插入删除效率低，数据量越大，效率越低
- deque相对于vector，对头部的插入删除快
- vector访问元素时的速度比deque快

![image-20220216100214541](../picture/image-20220216100214541.png)

构造、赋值函数、数据存取与vector类似

![image-20220216102312825](../picture/image-20220216102312825.png)![image-20220216103234068](../picture/image-20220216103234068.png)

### stack容器

栈结构

![image-20220217105020022](../picture/image-20220217105020022.png)

### queue容器

队列

![image-20220217105314282](../picture/image-20220217105314282.png)

### list容器

将数据进行链式存储，数据元素的逻辑顺序是通过链表中的指针链接实现的，结点由存储数据元素的数据域和存储下一个结点地址的指针域组成；STL中的链表是一个双向循环链表

![image-20220218103526590](../picture/image-20220218103526590.png)![image-20220218103733279](../picture/image-20220218103733279.png)![image-20220218104757655](../picture/image-20220218104757655.png)![image-20220218105639449](../picture/image-20220218105639449.png)![image-20220218110323575](../picture/image-20220218110323575.png)![image-20220218110457239](../picture/image-20220218110457239.png)

### set/ multiset容器

所有元素都会在插入时自动被排序

set容器不允许容器中有重复的元素，multiset允许；它们都属于关联式容器，底层结构是用二叉树实现

![image-20220219170314067](../picture/image-20220219170314067.png)![image-20220219170750210](../picture/image-20220219170750210.png)![image-20220219170833547](../picture/image-20220219170833547.png)![image-20220219170924790](../picture/image-20220219170924790.png)

利用仿函数，可以改变排序规则

### pair对组创建

成对出现的数据，利用对组可以返回两个数据

![image-20220219171711639](../picture/image-20220219171711639.png)

### map容器

map中所有元素都是pair；pair中第一个元素为key，起索引作用，第二个元素为value；所有元素都会根据元素的键值自动排序

map/multimap 属于关联式容器，底层结构是用二叉树实现

![image-20220220140945223](../picture/image-20220220140945223.png)![image-20220220141428399](../picture/image-20220220141428399.png)![image-20220220141532138](../picture/image-20220220141532138.png)![image-20220220141609674](../picture/image-20220220141609674.png)

### 函数对象

重载函数调用操作符的类，其对象常称为函数对象

函数对象使用重载的()时，行为类似函数调用，也叫仿函数

函数对象是一个类，可以像普通函数那样调用，可以有参数，可以有返回值，可以有自己的状态，可以作为参数传递

#### 谓词

返回bool类型的仿函数称为谓词；如果operator()接受一个参数，就叫一元谓词

使用内建函数对象，需要包含头文件functional





















