# 高级程序设计

**🐻桑**



[TOC]



## Lecture 01 数据抽象与封装概述

### 抽象与封装

抽象与封装是两个重要的程序设计手段，主要是用来驾驭程序的复杂度，便于**大型程序**的设计、理解与维护。

对于一个程序实体而言，
- 抽象是指该程序实体的外部可观察到的行为，不考虑该程序实体的内部是如何实现的。（控制复杂度）
- 封装是指把该程序实体内部的具体实现细节对使用者隐藏起来，只对外提供一个接口。（信息保护）

主要的程序抽象与封装机制包括：
- 过程抽象与封装
- 数据抽象与封装



### 过程抽象与封装

**过程抽象**

- 用一个名字来代表一段完成一定功能的程序代码，代码的使用者只需要知道代码的名字以及相应的功能，而不需要知道对应的程序代码是如何实现的。

**过程封装**

- 把命名代码的具体实现隐藏起来（对使用者不可见，或不可直接访问），使用者只能通过代码名字来使用相应的代码。
- 命名代码所需要的数据是通过参数（或全局变量）来获得，计算结果通过返回机制（或全局变量）返回。

实现过程抽象与封装的程序实体通常称为子程序。在C/C++语言中，子程序用函数来表示。

过程抽象与封装是基于功能分解与复合的**过程式程序设计**的基础。



### 数据抽象与封装

**数据抽象**

- 只描述对数据能实施哪些**操作**以及这些**操作之间的关系**，数据的使用者不需要知道数据的具体表示形式。

**数据封装**

- 把数据及其操作作为一个**整体**来进行实现，其中，数据的**具体表示**被隐藏起来（使用者**不可见**，或不可直接访问），对数据的访问（使用）只能通过提供的操作（对外**接口**）来完成。

与过程抽象与封装相比，数据抽象与封装能够实现更好的数据保护。

数据抽象与封装是**面向对象程序设计**的基础。



### “栈”数据的表示与操作（Example）

**栈**是一种由若干个具有线性次序关系的元素所构成的复合数据。对栈只能实施两种操作：

- **进栈（push）**：往栈中增加一个元素
- **退栈（pop）**：从栈中删除一个元素
- 上述两个操作满足一个重要性质：
  - push(...); ...pop(...); ... ;**push(x);pop(y)**; ...
  - x == y
- 即，增加/删除操作在线性序列的同一端进行（**后进先出**，Last In First Out，简称LIFO）

从数据抽象的角度，栈的使用者只需要知道上面的这些信息，不需要知道栈是如何实现的（数组或链表等）。

#### “栈”的实现 -- 非数据抽象和封装途径

定义一个数据类型来表示栈数据

```c++
const int STACK_SIZE=100;
struct Stack {	
    int top;
    int buffer[STACK_SIZE];
};
```

##### 直接操作栈数据

```c++
Stack st; //定义栈数据
st.top = -1; //对st进行初始化
//把12放进栈
if (st.top == STACK_SIZE-1) { 
    cout << “Stack is overflow.\n”; exit(-1); 
}
st.top++; 
st.buffer[st.top] = 12; 
......
//把栈顶元素退栈并存入变量x
if (st.top == -1) { 
    cout << “Stack is empty.\n”; exit(-1); 
}
int x = st.buffer[st.top]; 
st.top--;

```

存在的问题

- 操作必需知道数据的具体表示形式。
- 数据表示形式发生变化将会影响操作。

麻烦并易产生误操作，因此不安全。例如，把进栈操作误写成：
```c++
st.top--; //书写失误导致误操作
st.buffer[st.top] = 12;
```
- 忘了初始化： 

```c++
st.top = -1;
```

##### 通过过程抽象与封装操作栈数据

先预定义三个函数

```c++
void init(Stack &s){   
    s.top = -1;
}

void push(Stack &s, int i){	
    if (s.top == STACK_SIZE-1) {	
        cout << “Stack is overflow.\n”;
		exit(-1);
	}
	else{	
        s.top++; s.buffer[s.top] = i;
		return;
	}
}
void pop(Stack &s, int &i){
    if (s.top == -1) {
        cout <<“Stack is empty.\n”;
		exit(-1);
	}
  	else {
        i = s.buffer[s.top]; s.top--;
		return;
	}
}
```

利用预定义的函数操作栈数据

```c++
Stack st; //定义栈数据
int x;
init(st);  //对st进行初始化。
push(st,12);  //把12放进栈。
......
pop(st,x);  //把栈顶元素退栈并存入变量x。
```

存在的问题

- 数据类型的定义与操作的定义是**分开**的，二者之间没有**显式**的联系，push、pop在形式上与下面的函数f没有区别，函数f也能作用于st：

  ```c++
  void f(Stack &s) { ...... }
  f(st); //操作st之后，st可能不再是一个“栈”了！
  ```

- 数据表示仍然是公开的，无法防止使用者直接操作栈数据，因此也会面临直接操作栈数据所带来的问题：

  ```c++
  st.top--; 
  st.buffer[st.top] = 12;
  ```

- 忘了初始化：

  ```c++
  init(st);
  ```

#### “栈”的实现 -- 数据抽象和封装途径

定义栈数据类型

```c++
const int STACK_SIZE=100;
class Stack {	 
public: //对外的接口（外部可使用的内容）
	Stack();
   void push(int i);
   void pop(int &i);
private: //隐藏的内容，外部不可使用
   int top;
   int buffer[STACK_SIZE];
};
```

```c++
Stack::Stack(){
    top = -1; 
}

void Stack::push(int i){
    if (top == STACK_SIZE-1) {
        cout << “Stack is overflow.\n”;
		exit(-1);
	}
	else{
        top++;
        buffer[top] = i;
	    return;
	}
}
	
void Stack::pop(int &i){ 
    if (top == -1) {	
        cout << “Stack is empty.\n”;
		exit(-1);
	}
	else {	
        i = buffer[top]; top--;
		return;
	}
}
```

使用栈类型数据

```c++
Stack st;  //会自动地去调用st.Stack()对st进行初始化。
int x;
st.push(12);  //把12放进栈st。
......
st.pop(x);  //把栈顶元素退栈并存入变量x。
......
st.top = -1;  //Error
st.top++;  //Error
st.buffer[st.top] = 12;  //Error
st.f(); //Error
```

#### “栈”的另一种实现（链表表示）-- 数据抽象和封装途径

```c++
class Stack{	
public:
    Stack();
	void push(int i);
	void pop(int &i);
private:
	struct Node { 
        int content;
		Node *next;
	} *top;
};
```

```c++
Stack::Stack() { 
    top = NULL; 
}

void Stack::push(int i) {
    Node *p=new Node;
	if (p == NULL) {
        cout << "Stack is overflow.\n";
		exit(-1);
	}
	else {
        p->content = i;
		p->next = top;	top = p;
		return;
	}
}

void Stack::pop(int &i) {
    if (top == NULL) {
        cout << "Stack is empty.\n";
		exit(-1);
	}
	else {
        Node *p=top;
		top = top->next;
		i = p->content;
		delete p;
		return;
	}
}
```

改变栈类型数据的表示对使用者没有影响！

```c++
Stack st; 
int x;
st.push(12);
st.pop(x);
```



## Lecture 02 面向对象程序设计概述

### 什么是面向对象程序设计（Object-oriented Programming）

面向对象程序设计具有以下几个特征：
- 程序由若干对象组成，每个对象是由一些数据以及对这些数据所能实施的操作所构成的封装体；
- 对象的特征（包含那些数据与操作）由相应的类来描述；
- 对数据的使用是通过向包含数据的对象发送消息（调用对象类的对外接口中的操作）来实现的；
- 一个类所描述的对象特征可以从其它的类继承（获得）。

面向对象程序设计的基础是数据抽象与封装。

注意：如果没有“继承”，则称为：基于对象的程序设计（Object-based Programming）



### 面向对象程序的执行过程

对象构成了面向对象程序的基本计算单位，程序的执行体现为：对象间的一系列消息传递：
- 从程序外部向程序中的某个对象发送第一条消息启动计算过程；
- 该对象在处理这条消息的过程中，又向程序中的其它对象发送消息，从而引起进一步的计算；
- 消息处理可分为两种方式：
  - 同步消息处理：消息发送者必须等待消息处理完才能继续执行其它操作（顺序执行）。
  - 异步消息处理：消息发送者不必等待消息处理完就能继续执行其它操作（并发执行）



### 面向对象程序设计带来的好处

一个方法的优劣主要看它是否能提高软件开发效率和保证软件质量！

影响软件开发效率和软件质量的因素主要包括： 
- 抽象（控制复杂度）
- 封装（保护信息）
- 模块化（组织和管理大型程序）
- 软件复用（缩短开发周期）
- 可维护性（延长软件寿命）
- 软件模型的自然度（缩小解题空间与问题空间之间的语义间隙，实现从问题到解决方案的自然过渡）



### 面向对象程序设计的基本内容 

对象/类(Object&Class)
- 对象是由数据及能对其实施的操作所构成的封装体。
- 类描述了对象的特征（包含什么类型的数据和哪些操作），实现抽象。
- 对象属于值的范畴，是程序运行时刻的实体；类则属于类型的范畴，属于编译时刻的实体。

继承(Inheritance) 
- 在定义一个新的类（子类、派生类）时，可以把已有类（父类、基类）的一些特征描述先包含进来，然后再定义新的特征。
- 单继承与多继承

消息的多态与动态绑定
- 多态(Polymorphism)：某一论域中的一个元素存在多种解释。在程序中，多态通常体现为：
- 一名多用：
  - 函数名重载
  - 操作符重载（语言预定义和用户自定义）
- 类属：
  - 类属函数：一个函数能对多种类型的数据进行操作。
  - 类属类：一个类可以描述多种类型的对象。
- 绑定(Binding)：确定对多态元素的某个使用是多态元素的哪一种形式。可分为：
  - 静态绑定（Static Binding）：在编译时刻确定。
  - 动态绑定（Dynamic Binding）：在运行时刻确定。

面向对象程序特有的多态（继承机制带来的）：
- 对象类型的多态：子类对象既属于子类，也属于父类。
- 对象标识的多态：父类的引用或指针可以引用或指向父类对象，也可以引用或指向子类对象。
- 消息的多态：发给父类对象的消息也可以发给子类对象，父类与子类会给出不同的解释（处理）。
- 由于存在对象标识的多态，消息有时要采用动态绑定！

多态带来的好处
- 使得程序功能扩充变得容易（程序上层代码不变，只要增加底层的具体实现即可）。
- 增强语言的可扩充性（如操作符重载等）。 



## Lecture 03 对象与类



### 类

对象构成了面向对象程序的基本计算单位，而对象的特征则由相应的类来描述。

对象是用类来创建的，因此，程序中首先要定义类。

C++的类是一种用户自定义类型，定义形式如下：

```c++
class <类名> { <成员描述> } ;
```
- 成员包括：**数据成员**和**成员函数**。
- 类成员标识符的作用域为整个类定义范围。



### 数据成员

**数据成员**是对类的对象所包含的数据描述，它们可以是常量和变量，类型可以是任意的C++类型（void除外） 。

数据成员的描述格式与C的结构成员的描述格式相同，例如：

```c++
class Date {//类定义
 ......
 private: //访问控制说明
 int year,month,day; //数据成员描述
};
```

在C++旧标准中，描述数据成员时不允许进行初始化（某些静态数据成员除外）。例如：

```c++
class A { 
  int x=0;   //Error
  const double y=0.0;   //Error
  ......
};
```



### 成员函数

成员函数是对类定义中的数据成员所能实施的操作描述。

成员函数的实现（函数体）可以放在类定义中，例如：

```c++
class A
{ ...
  void f() {...} //建议编译器按内联函数处理
};
```

成员函数的实现也可以放在类定义外，例如：

```c++
class A
{ ...
  void f(); //声明
};

void A::f() { ... } //要用类名受限，区别于非成员函数（全局函数）
```

注意：在C++中，也允许在结构（`struct`）和联合（`union`）中定义函数，但成员的访问控制与类不同！

类成员函数名是可以重载的（析构函数除外），它遵循一般函数名的重载规则。例如：

```c++
class A { 
 ......
 public:
 void f();
 int f(int i);
 double f(double d);
  ......
};
```



### 类成员的访问控制 

在C++的类定义中，可以用访问控制修饰符public、private或protected来控制在类的外部对类成员的访问限制。例如：

```c++
class A
{	public: //访问不受限制。 
		int x;
		void f();
	private: //只能在本类和友元的代码中访问。 
		int y;
		void g();
	protected: //只能在本类、派生类和友元的代码中访问。 
		int z;
		void h();
};
```

在C++的类定义中，可以有多个public、private和protected访问控制说明；默认访问控制是private。例如：

```c++
class A
{		int m,n; //m,n的访问控制为private。
	public:
		int x;
		void f()；
	private:
		int y;
		void g();
	protected:
		int z;
		void h();
	public:
		void f1();
};
```

一般来说，类的数据成员和在类的内部使用的成员函数应该指定为private，只有提供给外界使用的成员函数才指定为public。 
- 具有public访问控制的成员构成了类与外界的一种接口（interface）。
- 在一个类的外部只能访问该类接口中的成员。

protected类成员访问控制具有特殊的作用（在派生类中使用）。



### 对象

类属于类型范畴的程序实体，它一般存在于静态的程序（编译程序看到的）中。

而动态（运行中）的面向对象程序则是由对象构成。

对象在程序运行时创建。



### 对象的创建和标识

#### 直接方式

通过在程序中定义一个类型为类的变量来实现的。例如：

```c++
class A {
public:
   void f();
   void g();
private:
   int x,y;
}
......
    
A a1; //创建一个A类的对象。

A a2[100]; //创建100个A类对象。
```

对象在进入相应变量的生存期时创建，通过变量名来标识和访问。相应变量的生存期结束时，对象消亡。

分为：全局对象、局部对象和成员对象。

#### 间接方式（动态对象）

在程序运行时刻，通过用new操作来创建对象，用delete操作来撤消（使之消亡）。对象通过指针来标识和访问。

单个动态对象的创建与撤消

```c++
A *p;
p = new A; // 创建一个A类的动态对象。
… *p … //或，p->...，通过p访问动态对象
delete p;  // 撤消p所指向的动态对象。
```

动态对象数组的创建与撤消

```c++
A *q;
q = new A[100];  //创建一个动态对象数组。
...q[i]... //或，*(q+i)，访问动态对象数组中的第i个对象
delete []q;  //撤消q所指向的动态对象数组。 
```



### 成员对象

对于类的数据成员，其类型可以是另一个类。即，一个对象可以包含另一个对象（称为成员对象）。例如： 

```c++
class A
{ ...
};

class B
{ ...
  A a; //成员对象
  ...
};

B b; //对象b包含一个成员对象：b.a
```

成员对象跟随包含它的对象一起创建。

在类中说明一个数据成员的类型时，如果未见到相应类型的定义，或相应的类型未定义完，则该数据成员的类型只能是这些类型的指针或引用类型。例如：

```c++
class A; //A是在程序其它地方定义的类，这里是声明。

class B
{ A a; //Error，未见A的定义。
  B b; //Error，B还未定义完。
  A *p; //OK
  B *q; //OK
  A &aa; //OK
  B &bb; //OK
}; 
```



### 对象的操作

对于创建的一个对象，需要通过向它发送消息（调用对象类中定义的某个public成员函数）来对它进行操作。例如：

```c++
class A
{		int x;
	public:
		void f();
};
int main()
{	A a; 	//创建A类的一个局部对象a。
	a.f(); 	//调用A类的成员函数f对对象a进行操作。
	A *p=new A;	 //创建A类的一个动态对象，p指向之。
	p->f(); 	//调用A类的成员函数f对p所指向的对象进行操作。
	delete p;
	return 0;
}
```

在类的外部，通过对象来访问类的成员时要受到类成员访问控制的限制，例如：

```c++
class A
{	public:
		void f() 
		{ ...... //允许访问：x,y,f,g,h 
		}
	private:
		int x;
		void g() 
		{ ...... //允许访问：x,y,f,g,h 
		}
	protected:
		int y;
		void h();
};
void A::f() //或A::g、A::h
{ ...... //允许访问：x,y,f,g,h
   A a;
   ...//能访问a.x、a.y、a.g和a.h
}
```

```c++
void func()
{  A a;
a.f();  //OK
a.x = 1;  //Error
a.g();  //Error
a.y = 1;  //Error
a.h();  //Error
......
}
```

可以对同类对象进行赋值

```c++
Date yesterday,today,some_day;
some_day = yesterday; //默认是把对象yesterday的
  //数据成员分别赋值给对象some_day的相应数据成员
```

取对象地址

```c++
Date today;
Date *p_date;
p_date = &today; //把对象today的地址赋值给指针p_date
```

把对象作为参数传给函数。例如：

```c++
void f(Date d) //创建一个新对象d，其数据成员用实参对象的数据成员对其初始化
{ ...... }

void g(Date &d) //不创建新对象，d就是实参对象！
{ ...... }

Date today;
f(today); //调用函数f，对象today不会被f修改
g(today); //调用函数g，对象today会被g修改！
```

把对象作为函数的返回值。例如，

```c++
Date f(Date &d)
{ d.print(); //输出：2020.2.20
   return d;  //创建一个临时对象作为返回值，用d对其初始化
}
Date& g(Date &d)
{ d.print(); //输出：2020.2.20
   return d;  //不创建新对象，把对象d作为返回值
}
Date some_day; //创建一个日期对象
some_day.set(2020,2,20); 
f(some_day).set(2017,3,13); some_day.print() //?
g(some_day).set(2017,3,13); some_day.print() //?
//前者显式：2020.2.20，因为修改的是临时对象
//后者显式：2017.3.13，因为修改的是some_day!
```



## Lecture 04 “this”指针

类的每一个成员函数（静态成员函数除外）都有一个隐藏的形参this，其类型为该类对象的指针；在成员函数中对类成员的访问是通过this来进行的。例如，
- 对于前面A类的成员函数g：

  ```c++
  void g(int i) { x = i; }
  ```

- 编译程序将会把它编译成：

  ```c++
  void g(A *const this, int i)  { this->x = i; }; 
  ```

当通过对象访问类的成员函数时，将会把相应对象的地址传给成员函数的this参数。例如，
- 对于下面的成员函数调用：`a.g(1);` 和 `b.g(2);`
- 编译程序将会把它编译成：`g(&a,1);` 和 `g(&b,2);`

一般情况下，类的成员函数中不必显式使用this指针，编译程序会自动加上。

但如果成员函数中要把this所指向的对象作为整体来操作，则需要显式地使用this指针。例如：

```c++
void func(A *p);
class A
{ int x;
 public:
  void g(int i) { x = i; func(this);  } 
  ...... 
};

......
A a,b;
a.g(1); //要求在g中调用func(&a)
b.g(2); //要求在g中调用func(&b)
```



### 用C语言实现C++的类

一个C++程序

```c++
class A
{  int x,y;
  public:
   void f();
   void g(int i) { x = i; f(); }
}；
......
A a,b;
a.f();
a.g(1);
b.f();
b.g(2);
```

功能上与前面C++程序等价的C程序

```c++
struct A
{ int x,y;
};
void f_A(struct A *this);
void g_A(struct A *this, int i)
{ this->x = i;
   f_A(this);
}
......
struct A a,b;
f_A(&a);
g_A(&a,1);
f_A(&b);
g_A(&b,2);
```

#### 结论：

面向对象是一种程序设计思想，用任何语言都可以实现！

采用面向对象语言会使得面向对象程序设计更加容易，语言也能提供更多的面向对象保障！



## Lecture 05 构造函数与析构函数

### 对象的初始化--构造函数

当一个对象被创建时，它将获得一块存储空间，该存储空间用于存储对象的数据成员。在使用对象前，需要对对象存储空间中的数据成员进行初始化。

C++提供了一种对象初始化的机制：构造函数
- 它是类的特殊成员函数，名字与类名相同、无返回值类型。
- 创建对象时，构造函数会被自动调用。例如：

```c++
class A
{		int x,y;
	public:
		A() { x = 0; y = 0; } //构造函数
	......
};
......
A a; //创建对象a：为a分配内存空间，然后调用A类的构造函数A()。
```

构造函数可以重载，其中，不带参数的（或所有参数都有默认值的）构造函数被称为默认构造函数。例如：

```c++
class A
{		int x,y;
	public:
		A() //默认构造函数
		{ x = y = 0;
		}
		A(int x1)
		{	x = x1; y = 0;
		}
		A(int x1,int y1)
		{	x = x1; y = y1;
		}
		......
};
```

也可以写成功能等价的：

```c++
class A
{  int x,y;
  public:
	A(int x1=0,int y1=0)
	{	x = x1; y = y1;
	}
	......
};
```

在创建对象时，
- 如果没有指定调用对象类中哪一个构造函数，则调用默认构造函数初始化。
- 也可以显式地指定调用对象类的某个构造函数。

```c++
class A
{		......
	public:
		A();
		A(int i);
		A(char *p);
};
......
A a1;    //调用默认构造函数。也可写成：A a1=A(); 
		   //但不能写成：A a1();
A a2(1);    //调用A(int i)。也可写成：A a2=A(1); 或 A a2=1; 
A a3("abcd");    //调A(char *)。也可写成：A a3=A("abcd");
			     //或 A a3="abcd"; 
A a[4];    //调用对象a[0]、a[1]、a[2]、a[3]的默认构造函数。
A b[5]={A(),A(1),A("abcd"),2,"xyz"};     //调用b[0]的A()、
					//b[1]的A(int)、b[2]的A(char *)、
					//b[3]的A(int)和b[4]的A(char *)
```

```c++
A *p1=new A;     //调用默认构造函数
A *p2=new A(2);     //调用A(int i)
A *p3=new A("xyz");    //调用A(char *)
A *p4=new A[20];   //创建动态对象数组时
			  //只能调用各对象的默认构造函数
```

- n注意：对象创建后，不能再调用构造函数！例如，

```c++
A a;
......
a.A(1); //Error!
```



### 成员初始化表

对于常量和引用数据成员，不能在说明它们时初始化，也不能采用赋值操作在构造函数中对它们初始化。例如： 

```c++
class A 
{	 int x;
     const int y=1;   //Error
     int &z=x;     //Error
   public:
     A()
     { x = 0;    //OK
	   y = 1;    //Error  
	   z = &x;  //Error    
	   z = x;  //Error   
     }
};
```

可以在构造函数的函数头和函数体之间加入一个成员初始化表来对常量和引用数据成员进行初始化。例如：

```c++
class A
 {	int x;
	  const int y;
	  int& z;
   public:
	  A(): z(x),y(1)  //成员初始化表
	  { x = 0;
	  }
};
```

在成员初始化表中，成员的书写次序并不决定它们的初始化次序，它们的初始化次序由它们在类定义中的描述次序来决定。



### 析构函数

在类中可以定义一个特殊的成员函数：析构函数，它的名字为`~<类名>`，没有返回类型、不带参数、不能被重载。例如：

```c++
class A 
{  ......
 public:
   ......
   ~A(); //析构函数
}; 
```

一个对象消亡时，系统在收回它的内存空间之前，将会自动调用对象类中的析构函数。可以在析构函数中完成对象被删除前的一些清理工作。

一般情况下，类中不需要自定义析构函数，但如果对象创建后，自己又额外申请了资源（如：额外申请了内存空间），则可以自定义析构函数来归还它们。

例如，在下面的程序中，系统为对象`s1`分配的内存空间只包含`len`和`str`（指针）本身所需的空间，`str`所指向的空间不由系统分配的，而是由对象作为资源自己申请的，`s1`消亡时，需要在析构函数中归还`str`指向的空间！

```c++
class String 
{      int len; 
       char *str;
    public:
       String(char* s)
       {  len = strlen(s);
          str = new char[len+1]; //申请额外的内存空间
          strcpy(str, s);
        } 
        ~String()
        {  delete[] str; //归还额外申请的空间
           len = 0; str = NULL; //有必要吗？-- 如果不显示调用，则没有必要
        }
......
};
void f()
{  String s1("abcd"); //调用s1的构造函数
    ...... 
} //调用s1的析构函数
```

```c++
class Stack
{	public:
		Stack() { top = NULL; }
		~Stack() //析构函数
		{ while (top != NULL)
		   {  Node *p=top;   
              top = top->next;
		      delete p;
		   }  
		}
		void push(int i);
		void pop(int &i);
	private:
		struct Node
		{ int content;
		   Node *next;
		} *top;
};
```

析构函数可以显式调用，这时并不是让对象消亡，而是暂时归还对象额外申请的资源。

例如，

```c++
String s1("abcd");
......
s1.~String(); //把字符串s1清空，对象并未消亡！
... s1 ... //仍然可以使用对象s1
```

再例如，

```c++
Stack st;
... st.push ... st.pop ...
st.~Stack(); //清空栈st
... st.push ... st.pop ... //仍然可以使用栈st
```



### 成员对象的初始化和消亡处理

如果创建的对象包含成员对象，那么，成员对象如何进行初始化和消亡处理？

- 在创建包含成员对象的对象时，除了会自动调用本身类的构造函数外，还会自动去调用成员对象类的构造函数。

- 通常是自动调用成员对象类的默认构造函数。

- 如果要调用成员对象类的**非默认构造函数**，需要在包含成员对象的对象类的构造函数**成员初始化表**中显式指出！

- 包含成员对象的对象消亡时，除了会自动调用本身类的析构函数外，还会自动去调用成员对象类的析构函数。

```c++
class A
{   int x;
   public:
	 A() { x = 0; }
	 A(int i) { x = i; }
};
class B
{    A a;
     int y;
   public:
     B() { y = 0;}
     B(int i) { y = i; } 
     B(int i, int j): a(j) { y = i; } 
};
B b0; //调用B()和A()：b0.y=0；b0.a.x=0
B b1(1); //调用B(int)和A()：b1.y=1；b1.a.x=0
B b2(1,2); //调用B(int,int)和A(int)：b2.y=1；b2.a.x=2
......
//b0、b1、b2消亡时，会分别去调用B和A的析构函数
```



### 成员对象初始化和消亡处理的次序

创建包含成员对象的对象时，
- 先执行成员对象类的构造函数，再执行本对象类的构造函数。
- 若包含多个成员对象，这些成员对象的构造函数执行次序则按它们在本对象类中的说明次序进行。
- 从实现上说，
  - 是先调用本身类的构造函数，但在进入函数体之前，会去调用成员对象类的构造函数，然后再执行本身类构造函数的函数体！
  - 也就是说，构造函数的成员初始化表（即使没显式给出）中有对成员对象类的构造函数的调用代码。
  - 注意：如果类中未提供任何构造函数，但它包含成员对象，则编译程序会隐式地为之提供一个默认构造函数，其作用就是调用成员对象类的构造函数！

对象消亡时，
- 先执行本身类的析构函数，再执行成员对象类的析构函数。
- 如果有多个成员对象，则成员对象析构函数的执行次序则按它们在本对象类中的说明次序的逆序进行。
- 从实现上说，
  - 是先调用本身类的析构函数，本身类析构函数的函数体执行完之后，再去调用成员对象类的析构函数！
  - 也就是说，析构函数的函数体最后有对成员对象类的析构函数的调用代码！
  - 注意：如果类中未提供析构函数，但它包含成员对象，则编译程序会隐式地为之提供一个析构函数，其作用就是调用成员对象类的析构函数。



## Lecture 06 拷贝构造函数

若一个构造函数的参数类型为本类的引用，则称它为拷贝构造函数。例如：

```c++
class A
{	......
	public:
		A();  //默认构造函数
		A(const A& a);  //拷贝构造函数
}; 
```

在创建一个对象时，若用另一个同类型的对象对其初始化，将会调用对象类中的拷贝构造函数。

在三种情况下，会调用类的拷贝构造函数：
- 创建对象时显式指出。例如：

  ```c++
  A a1; 
  A a2(a1); //创建对象a2，用对象a1初始化对象a2
  ```

- 把对象作为值参数传给函数时。例如：

  ```c++
  void f(A x) { ...... };
  ......
  A a;
  f(a); //创建形参对象x，用对象a对x进行初始化。
  ```

- 把对象作为函数的返回值时。例如：

  ```c++
  A f() 
  { A a;
   ......
   return a; //创建一个临时对象，用对象a对创建的临时对象进行初始化。
  }
  ```



### 隐式拷贝构造函数

在程序中，如果没有为某个类提供拷贝构造函数，则编译器将会为其生成一个隐式拷贝构造函数。

隐式拷贝构造函数将逐个成员进行拷贝初始化
- 对于非对象成员：它采用通常的拷贝操作；
- 对于成员对象：则调用成员对象类的拷贝构造函数来对成员对象进行初始化。（递归定义）

```c++
class A
{	  int x,y;
	public:
	  A() { x = y = 0; }
	  ......
};
class B
{	  int z;
	  A a;
   public:
	  B() { z = 0; }
	  ...... //其中没有定义拷贝构造函数
};
...
B b1; //b1.z以及b1.a.x和b1.a.y均为0。
B b2(b1); //b2.z初始化成b1.z；
		      //调用A的拷贝构造函数用b1.a对b2.a初始化
```



### 自定义拷贝构造函数

一般情况下，编译程序提供的隐式拷贝构造函数的行为足以满足要求，类中不需要自定义拷贝构造函数。

但在一些特殊情况下，必须要自定义拷贝构造函数，否则，将会产生设计者未意识到的严重的程序错误。 

例如，在下面的类中没有自定义拷贝构造函数：

```c++
class String
{	   int len;
	   char *str;
	public:
		String(char *s) 
		{ len = strlen(s); 
		   str = new char[len+1]; 
		   strcpy(str,s); 
		}
		~String() { delete []str; len=0; str=NULL; }
};
......
String s1("abcd");
String s2(s1);
```

系统提供的隐式拷贝构造函数将会使得`s1`和`s2`的成员指针`str`指向同一块内存区域！

它带来的问题是：
- 如果对一个对象（`s1`或`s2`）操作之后修改了这块空间的内容，则另一个对象将会受到影响。如果不是设计者特意所为，这将是一个隐藏的错误。
- 当对象`s1`和`s2`消亡时，将会分别去调用它们的析构函数，这会使得同一块内存区域将被归还两次，从而导致程序运行错误。
- 当对象`s1`和`s2`中有一个消亡，另一个还没消亡时，则会出现使用已被归还的空间问题！

解决上面问题的办法是在类String中显式定义一个拷贝构造函数

```c++
String::String(const String& s)
{ len = s.len;
 str = new char[len+1];
 strcpy(str,s.str);
}
```

注意：自定义的拷贝构造函数默认调用的是成员对象类的默认构造函数来对成员对象初始化！

```c++
class A
{		int x,y;
	public:
		A() { x = y = 0; }
		void inc() { x++; y++; }
};
class B
{    int z;
	  A a;
   public:
	  B() { z = 0; }
	  B(const B& b) { z = b.z; }
	  void inc() { z++; a.inc(); }
};
...
B b1;  /b1.z、b1.a.x和b1.a.y均为0
b1.inc();  //b1.z、b1.a.x和b1.a.y均变成了1
B b2(b1); //b2.z为1，b2.a.x和b2.a.y均为0
```

如何能让`b2`与`b1`一致呢？

```c++
class A
{		int x,y;
	public:
		A() { x = y = 0; }
		void inc() { x++; y++; }
};
class B
{    int z;
	  A a;
   public:
	  B() { z = 0; }
	  B(const B& b) : a(b.a) { z = b.z; }
	  void inc() { z++; a.inc(); }
};
...
B b1;  /b1.z、b1.a.x和b1.a.y均为0
b1.inc();  //b1.z、b1.a.x和b1.a.y均变成了1
B b2(b1); //b2.z为1，b2.a.x和b2.a.y均为1
```



## Lecture 07 常成员函数、静态成员

### 获取 vs. 改变 对象的状态

在程序运行的不同时刻，一个对象可能会处于不同的状态（由对象的数据成员的值来体现）。

可以把类中的成员函数分成两类：
- 获取对象状态（不改变数据成员的值）
- 改变对象状态（改变数据成员的值）

例如:

```c++
class Date

{ public:

 void set(int y, int m, int d); //改变对象状态
 int get_day(); //获取对象状态
 int get_month(); //获取对象状态
 int get_year(); //获取对象状态
  ......
}; 
```



### 常成员函数

为了防止在一个获取对象状态的成员函数中无意中修改对象的数据成员，可以把它说明成常成员函数。例如，

```c++
class Date
{	public:
	 void set(int y, int m, int d); 
	 int get_day() const; //常成员函数
     int get_month() const; //常成员函数
     int get_year() const; //常成员函数
  ......
}; 
int Date::get_year() const { return year; }
int Date::get_day() const { return day; }
int Date::get_month() const { return month; }
void Date::set(int y, int m, int d) { year=y; month=m; day=d; }
```

编译器一旦发现在常成员函数中修改数据成员的值，将会报错！

注意：对于有些修改对象的常成员函数，编译程序不会指出错误！

```c++
class A
{	    int x;
	    char *p;
	public:
		......
		void f() const 
		{ x = 10; //Error
		   p = new char[20]; //Error 
		   strcpy(p,"ABCD"); //因为没有改变p的值，
                             //编译程序认为OK！
		}
};
```

常成员函数还有一个作用：指出对常量对象能实施哪些操作，即，只能调用对象类中的常成员函数。例如：

```c++
class Date
{ public:
      int set (int y, int m, int d);
      int get_day() const;
      int get_month() const;
      int get_year() const;
   ......
}； 
void f(const Date &d) //d是个常量对象！
{  ... d.get_day() ... //OK
   ... d.get_month() ... //OK
   ... d.get_year() ... //OK
   d.set (2011,3,23); //Error
}     
```



### 同类对象如何共享数据?

采用全局变量来表示共享数据：

- 共享的数据与对象之间缺乏显式的联系

- 数据缺乏保护！

```c++
int x=0; //全局变量
class A
{  int y;
   ......
   void f()   { y = x; x++; ......   }
} a,b;
//以下操作对同一个x进行
a.f(); 
b.f();
x++; //不通过A类对象也能访问x，不安全！
```



### 静态数据成员

采用静态数据成员可以更好地实现同一个类的不同对象之间的数据共享。例如

```c++
class A
{  int y;
   ......
   static int x; //静态数据成员声明
   void f()   { y = x; x++; ......   }
};
int A::x=0; //静态数据成员定义及初始化
......
A a,b;
a.f();
b.f();
//上述操作对同一个x进行
x++; //Error，不通过A类对象不能访问x！
```

类的静态数据成员对该类的所有对象只有一个拷贝。例如，对于下面的类A：

```c++
class A
{	  int x,y;
	  static int shared; 
	public:
	  A() { x = y = 0; }
	  void increase_all() { x++; y++; shared++; }
	  int sum_all() const { return x+y+shared; }
	  ......
};
int A::shared=0; 
```



### 静态成员函数

成员函数也可以声明成静态的（静态成员函数）。

```c++
class A
{	int x,y;
	static int shared;
public:
	A() { x = y = 0; }
	static int get_shared() //静态成员函数
    { return shared; 
    }
	......
};
int A::shared=0;
```

- 静态成员函数只能访问类的静态成员。
- 静态成员函数没有隐藏的this参数！

- 静态成员除了通过对象来访问外，也可以直接通过类来访问。例如：

```c++
A a;
...... 
cout << a.get_shared();
```

或者

```c++
cout << A::get_shared(); 
```

例： 对运行时刻某类对象的个数进行计数

```c++
class A 
{      static int obj_count;
       ......
   public:
       A()  //在所有构造函数中增加：obj_count++;
       { obj_count++; ...... }
       ~A()  //在析构函数中增加：obj_count--;
       { obj_count--; ...... }
       static int get_num_of_objects() 
       { return obj_count; 
	   }
       …...
};
int A::obj_count=0;
...... //其中创建了一系列A类对象
cout << A::get_num_of_objects(); //输出A类对象的个数
```



## Lecture 08 友元

根据数据封装的要求，类中定义的数据成员不能在外界直接访问，必须要通过类中定义的public成员函数来访问。在有些情况下，这种对数据的访问方式效率不高！

为了提高在类的外部对类的数据成员的访问效率，在C++中，

- 可以指定某些与一个类密切相关的、又不适合作为该类成员的程序实体直接访问该类的非public成员，这些程序实体称为该类的友元。

- 友元是数据保护和数据访问效率之间的一种折衷方案。

友元需要在类中用friend显式指出，它们可以是全局函数、其它的类或其它类的某些成员函数。

```c++
class A
{ ......
 friend void func(); //全局函数func可访问x
 friend class B; //类B的所有成员函数可访问x
 friend void C::f(); //类C的成员函数f可访问x
private:
 int x;
}; 
```



### 关于友元的几点说明

- 友元不是一个类的成员。

- 友元关系具有不对称性。例如：假设B是A的友元，如果没有显式指出A是B的友元，则A不是B的友元。

- 友元也不具有传递性。例如：假设C是B的友元、B是A的友元，如果没有显式指出C是A的友元，则C不是A的友元。



### 例：实现矩阵与向量相乘操作

矩阵类的定义

```c++
class Matrix  //矩阵类
{		int *p_data;  //表示矩阵数据
		int row,col;  //表示矩阵的行数和列数
	public:
		Matrix(int r, int c) 
		{	if (r <= 0 || c <= 0)
			{	cerr << "矩阵尺寸不合法！\n";
				exit(-1);
			}
			row = r;	col = c;
			p_data = new int[row*col];
			for (int i=0; i<row*col; i++) p_data[i] = 0;
		}
 
		~Matrix() 	{ delete []p_data; }
 
		int &element(int i, int j) //访问矩阵元素。
		{	if (i < 0 || i >= row || j < 0 || j >= col)
			{	cerr << "矩阵下标越界\n";
				exit(-1);
			}
			return *(p_data+i*col+j);
		}
 
        int element(int i, int j) const //重载的特殊情况！
                       //访问矩阵元素（为常量对象而提供）。
		{	if (i < 0 || i >= row || j < 0 || j >= col)
			{	cerr << "矩阵下标越界\n";
				exit(-1);
			}
			return *(p_data+i*col+j);
		}

        int dimension_row() const //获得矩阵的行数。
		{	return row;
		}
		int dimension_column() const //获得矩阵的列数。
		{	return col;
		}
	    void display() const //显示矩阵元素。
		{	int *p=p_data; 
			for (int i=0; i<row; i++)
			{	for (int j=0; j<col; j++)
				{ cout << *p << ' ';
				   p++;
				}
				cout << endl;
			}
		}
};
```

向量类的定义

```c++
class Vector  //向量类
{		int *p_data;
		int num;
	public:
		Vector(int n)
		{	if (n <= 0) 
			{	cerr << "向量尺寸不合法！\n"; 
				exit(-1);
			}
			num = n;
			p_data = new int[num];
			for (int i=0; i<num; i++) p_data[i] = 0;
		}
 
		~Vector()
		{	delete []p_data;
		}

        int &element(int i) //访问向量元素。
		{	if (i < 0 || i >= num)
			{	cerr << "向量下标越界！\n";
				exit(-1);
			}
			return p_data[i];
		}
 
		int element(int i) const 
                     //访问向量元素（为常量对象而提供）。
		{	if (i < 0 || i >= num)
			{	cerr << "向量下标越界！\n";
				exit(-1);
			}
			return p_data[i];
		}
 
        int dimension() const //返回向量的尺寸。
		{	return num;
		}
		void display() const //显示向量元素。
		{	int *p=p_data;
			for (int i=0; i<num; i++,p++)
				cout << *p << ' ';
			cout << endl;
		}
};
```

矩阵与向量对象的创建

```c++
int row,column; //矩阵的行数、列数
cin >> row >> column;
Matrix m(row,column); //创建矩阵对象
for (int i=0; i<row; i++) //输入矩阵数据
	for (int j=0; j<column; j++)
		cin >> m.element(i,j);
m.display(); //显示矩阵数据

int dim; //向量元素个数
cin >> dim;
Vector v(dim); //创建向量对象
for (i=0; i<column; i++) //输入向量数据
	 cin >> v.element(i);
v.display(); //显示向量数据
```

编写一个函数multiply，实现矩阵与向量相乘的操作： 

```c++
void multiply(const Matrix &m, const Vector &v, Vector &r); 
```

#### multiply的实现1（非友元）

```c++
void multiply(const Matrix &m, const Vector &v, Vector &r)//矩阵与向量相乘。
{	if (m.dimension_column() != v.dimension() || m.dimension_row() != r.dimension())
	{  cerr << "矩阵和向量的尺寸不匹配！\n";
		  exit(-1);
	}
	int row=m.dimension_row(), col=m.dimension_column();
	for (int i=0; i<row; i++)
	{  r.element(i) = 0;
		  for (int j=0; j<col; j++) //计算r[i]
			r.element(i) += m.element(i,j)*v.element(j);
	}
}
```

- 由于频繁地通过调用成员函数element来访问矩阵和向量的元素，每一次调用中都要检查下标的合法性，因此效率不高！

#### multiply的实现2（友元）

- 把multiply说明成Matrix和Vector的友元

```c++
class Vector; //Vector的声明（由于在定义它前需要用到它）
class Matrix
{	......
	friend void multiply(const Matrix &m, const Vector &v, 
					Vector &r); //这里提前用到Vector。
};
class Vector
{	......
	friend void multiply(const Matrix &m, const Vector &v, 
					Vector &r);
};
```

- 在multiply中直接访问Matrix和Vector的私有数据成员

```c++
void multiply(const Matrix &m, const Vector &v, Vector &r)
{	if (m.col != v.num || m.row != r.num)
	{	cerr << "矩阵和向量的尺寸不匹配！\n";
			exit(-1);
	}
	int *p_m=m.p_data,*p_r=r.p_data,*p_v;
	for (int i=0; i<m.row; i++)
	{	*p_r = 0;
			p_v = v.p_data;
			for (int j=0; j<m.col; j++)
			{	*p_r += (*p_m)*(*p_v);
				p_m++;
				p_v++;
			}
			p_r++;
	}
}
```



## Lecture 09 类作为模块

### 什么是模块？

模块是指：从物理上对程序中定义的实体进行分组，是可以单独编写和编译的程序单位。

模块是组织和管理大型程序的一个重要手段。

一个模块包含接口和实现两部分：
- 接口：是指在模块中定义的、可以被其它模块使用的一些程序实体的描述。
- 实现：是指在模块中定义的所有程序实体的具体实现描述。

对于C语言程序中的模块：

- 接口：包含被外界使用的类型定义、常量定义以及全局变量和函数的声明。（.h文件）

- 实现：包含本模块中所有的类型、常量、全局变量和函数的定义。（.c文件）



### 如何划分模块

划分模块的基本准则

- 内聚性最大：模块内的各实体之间联系紧密。

- 耦合度最小：模块间的各实体之间关联较少。

- 便于程序的设计、理解和维护，能够保证程序的正确性。

过程式程序的模块划分

- 通常是基于子程序，把共同完成某独立功能的或使用相同数据的子程序及相关的实体放在一起构成模块。

- 缺点是模块边界模糊：
  - 一个子程序可能参与几个功能。
  - 一个子程序可能使用多个数据集。



### 类作为模块

在面向对象程序中，类是一个自然的模块划分单位，模块边界比较清晰。

一个C++程序的模块由两部分构成：

- 接口：类的定义，存放在一个.h文件中

- 实现：类的实现（主要是类中成员函数），存放在一个.cpp文件中。 



### 良好的面向对象程序设计风格

结构化程序设计为过程式程序设计提供了一种良好的风格指南，它要求程序单位具有“单入口/单出口”性质，具体体现为：

- 不带goto的顺序、分支和循环流程控制。

- 子程序

良好的面向对象程序设计风格是什么呢？

- 模块间的耦合度反映在对象类之间的关联性上。

- 要降低模块间的耦合度，可以对类中成员函数能访问的对象的集合作一定的限制，并尽量使该集合为最小。

- 上述要求被称为Demeter法则。



### Demeter法则（Law of Demeter）

一个类的成员函数

- 除了能访问自身类结构的直接子结构（本类的数据成员）外，不能以任何方式依赖于任何其它类的结构。

- 只应向某个有限集合中的对象发送消息。

“仅与你的直接朋友交谈！”

```c++
class Student
{ ... id; ... name;
   ......
   void print();
};
class Class
{ ... id; ... name; 
   Student student_list[100];
   ......
   void print();
   Student& get(int i);
};
class School
{ ... id; ... name;
   Class class_list[300];
   ......
   void f()
   { ... id, name ...  //OK
      ... class_list[i].id ,class_list[i].name ... //Not OK，非直接子结构
      class_list[i].print(); //OK
      class_list[i].get(j).print(); //Not OK，Student不是直接朋友
   }
};
```

Demeter法则有两种表达形式：

- 类表达形式：一个类中的成员函数能访问（向其发消息）的对象所属类的集合。

- 对象表达形式：一个类中的成员函数能访问（向其发消息）的对象的集合。

- 前者适合于静态类型的面向对象语言（如：C++），它可以在编译时刻检查程序是否满足法则；后者则适合于动态类型的面向对象语言（如：Smalltalk），它需要在运行时刻检查程序是否满足法则。

#### Demeter法则的类表达形式

对于类C中的任何成员函数M，M中只能向以下类的对象发送消息：

1. 类C本身。

2. 成员函数M的参数类。

3. M或M所调用的成员函数中创建的对象所属的类。

4. 全局对象所属的类。

5. 类C的成员对象所属的类。

#### Demeter法则的对象表达形式

对于类C中的任何成员函数M，M中只能向以下的对象发送消息：

- this指向的对象。

- 成员函数M的参数对象。

- M或M所调用的成员函数所创建的对象。

- 全局变量中包含的对象。

- 类C的成员对象。



## Lecture 10 操作符重载

### 操作符重载的需要性 

C++语言本身没有提供复数类型，可通过定义一个类来实现：

```c++
class Complex //复数类定义
{public:
  Complex(double r=0.0,double i=0.0)   
  { real=r; imag=i; 
  }
  void display() const
  { cout << real << '+' << imag << 'i';
  } 
  ......

 private: 
  double real;
  double imag;
}; 
```

如何实现两个复数（类型为Complex）相加？ 

- 一种方案：为Complex类定义一个成员函数add，例如：

```c++
class Complex
{	public:
 		……	
		Complex add(const Complex& x) const
		{	Complex temp;
			temp.real = real+x.real;
			temp.imag = imag+x.imag;
			return temp;
		}
};
……
Complex a(1.0,2.0),b(3.0,4.0),c;
c = a.add(b);
```

- 另一种方案：定义一个全局函数（声明成Complex类的友元），例如：

```c++
class Complex	//复数类定义
{  ......
   friend Complex add(const Complex& x1, const Complex& x2);
}; 
Complex add(const Complex& x1, const Complex& x2)
{	Complex temp;
	temp.real = x1.real+x2.real;
	temp.imag = x1.imag+x2.imag;
	return temp;
}
……
Complex a(1.0,2.0),b(3.0,4.0),c;
c = add(a,b);
```

- 在前面的两种实现中，加法操作形式不符合数学上的习惯：`c=a+b`

- C++允许对已有的操作符进行重载，使得它们能对自定义类型（类）的对象进行操作。

- 与函数名重载一样，操作符重载也是实现多态性的一种语言机制。



### 操作符重载的实现途径

操作符重载可通过下面两个途径来实现：
- 作为一个类的非静态的成员函数（操作符new和delete除外）。

- 作为一个全局（友元）函数。
  - 至少应该有一个参数是类、结构、枚举或它们的引用类型。

- 不管是采用成员函数还是全局函数，重载的函数名字都为：
  - `operator #`

  - “#”代表任意可重载的操作符。

例如，以成员函数重载复数的“+”：

```c++
class Complex
{	public:
		Complex operator + (const Complex& x) const
		{	Complex temp;
			temp.real = real+x.real;
			temp.imag = imag+x.imag;
			return temp;
		}
    ......
};
……
Complex a(1.0,2.0),b(3.0,4.0),c;
c = a + b;
```

再例如，以全局函数重载重载复数的“+”：

```c++
class Complex
{	......
	friend Complex operator + (const Complex& c1, 
					           const Complex& c2);
};
Complex operator + (const Complex& c1, const Complex& c2)
{	Complex temp;
	temp.real = c1.real + c2.real;
	temp.imag = c1.imag + c2.imag;
	return temp;
}
……
Complex a(1.0,2.0),b(3.0,4.0),c;
c = a + b;
```

- 一般情况下，操作符既可以作为全局函数，也可以作为成员函数来重载。

- 在有些情况下，操作符只能作为全局函数或只能作为成员函数来重载！



### 操作符重载的基本原则 

- 只能重载C++语言中已有的操作符，不可臆造新的操作符。

- 可以重载C++中除下列操作符外的所有操作符：
  - `. `， `.* `，`?: `，`:: `，`sizeof `

- 遵循已有操作符的语法:
  - 不能改变操作数个数。
  - 不改变原操作符的优先级和结合性。

- 尽量遵循已有操作符原来的语义：
  - 语言本身没有对此做任何规定，使用者自己把握 ！



### 双目操作符重载

#### 作为成员函数重载

定义格式

```c++
class <类名>
{	......
	<返回值类型> operator # (<类型>); //#代表可重载的操作符
};
<返回值类型> <类名>::operator # (<类型> <参数>) { ...... }
```

使用格式

```c++
<类名> a;
<类型> b;
a # b
//或
a.operator#(b)
```

例：实现复数的“等于”和“不等于”操作

```c++
class Complex
{		double real, imag;
	public:
		......
		bool operator ==(const Complex& x) const
		{	return (real == x.real) && (imag == x.imag);
		}
		bool operator !=(const Complex& x) const
		{	return (real != x.real) || (imag != x.imag);
		}
};
......
Complex c1,c2;
...... 
if (c1 == c2) //或 if (c1 != c2)
......
```

```c++
bool operator !=(const Complex& x) const //更好！
{	return !(*this == x);
}
```

#### 作为全局（友元）函数重载

定义格式

```c++
<返回值类型> operator #(<类型1> <参数1>, <类型2> <参数2>) 
{ ...... }
// <类型1>和<类型2>中至少应该有一个是类、结构、枚举或它们的引用类型。
```

使用格式

```c++
<类型1> a;
<类型2> b;
a # b
或
operator#(a,b)
```

例：重载操作符+，使其能够实现实数与复数的混合运算。 

```c++
class Complex
{		double real, imag;
	public:
		Complex() { real = 0; imag = 0; }
		Complex(double r, double i) { real = r; imag = i; }
		......
	friend Complex operator + (const Complex& c1, const Complex& c2);
	friend Complex operator + (const Complex& c, double d);
	friend Complex operator + (double d, const Complex& c);
};
```

```c++
Complex operator + (const Complex& c1, 
				      const Complex& c2)
{	return Complex(c1.real+c2.real,c1.imag+c2.imag);
}
Complex operator + (const Complex& c, double d)
{	return Complex(c.real+d,c.imag);
}
Complex operator + (double d, const Complex& c)
//“实数+复数”只能作为全局函数重载，因为类里面重载操作符的第一个操作数默认为this，是本类
{	return Complex(d+c.real,c.imag);
}
......
Complex a(1,2),b(3,4),c1,c2,c3;
c1 = a + b;
c2 = b + 21.5;
c3 = 10.2 + a;
```



### 单目操作符重载

#### 作为成员函数重载

定义格式

```c++
class <类名>
{	......
	<返回值类型> operator # (); 
};
<返回值类型> <类名>::operator # () { ...... }
```

使用格式

```c++
<类名> a;
#a
//或，
a.operator#()
```

例：实现复数的取负操作

```c++
class Complex
{	......
	public:
		......
		Complex operator -() const
		{	Complex temp;
			temp .real = -real;
			temp.imag = -imag;
			return temp;
		}
};
......
Complex a(1,2),b;
b = -a;  //把b修改成a的负数。
```

#### 作为全局（友元）函数重载

定义格式

```c++
<返回值类型> operator #(<类型> <参数>) { …... }
// <类型>必须是类、结构、枚举或它们的引用类型。
```

使用格式为：

```c++
<类型> a;
\#a
// 或
operator#(a)
```

例：实现判断复数为“零”的操作

```c++
class Complex
{	......
	public:
		......
		friend bool operator !(const Complex &c);
};
bool operator !(const Complex &c)
{	return (c.real == 0.0) && (c.imag == 0.0);
}
......
Complex a(1,2);
......
if (!a) //a为0
  ......
......
```



### 操作符++和-- 的重载

单目操作符++（--）：

```C++
int x,y;
++x;或 x++; //x的值加1
x = 0; y = ++x; //x的值为1，y的值为1
x = 0; y = x++; //x的值为1，y的值为0
++(++x);
//or 
(++x)++; //OK，x的值加2。++x为左值表达式

++(x++);
//or
(x++)++; //Error。x++为右值表达式
```

重载++（--）时，如果没有特殊处理，它们的后置用法和前置用法共用同一个重载函数。

为了能够区分++（--）的前置与后置用法，可为它们再写一个重载函数用于实现它们的后置用法，该重载函数应有一个int型参数（函数体中可以不使用该参数的值）。

```c++
class Counter
{		int value;
	public:
		Counter() { value = 0; }
		Counter& operator ++()  //前置的++重载函数
		{	value++;
			return *this;
		}
		const Counter operator ++(int)  //后置的++重载函数
		{	Counter temp=*this; //保存原来的对象
			value++; //写成：++(*this);更好！调用前置的++重载函数
			return temp; //返回原来的对象
		}
};
.....
Counter a,b,c;
++a;  //使用的是不带参数的操作符++重载函数
a++;  //使用的是带int型参数的操作符++重载函数
b = ++a;  //加一之后的a赋值给b
c = a++;  //加一之前的a赋值给b
++(++a);或 (++a)++; //OK，a加2
++(a++);或 (a++)++; //Error，编译不通过
```



## Lecture 11 C++特殊操作符的重载

### C++特殊操作符

- 赋值操作符`=`

- 访问数组元素操作符`[]`

- 动态对象创建与撤销操作符`new`与`delete`

- 函数调用操作符`()`

- 类成员访问操作符`->`

- 类型转换操作符

### 赋值操作符“=”的重载

同一个类的两个对象如何赋值？

```c++
A a,b;
......
a = b; 
```

C++编译程序会为每个类定义一个隐式的赋值操作，其行为是：逐个成员进行赋值操作。
- 对于普通成员，它采用常规的赋值操作。
- 对于成员对象，则调用该成员对象类的赋值操作进行成员对象的赋值操作。

隐式的赋值操作有时不能满足要求！

例如，对下面String类的对象进行赋值操作时，系统提供的隐式赋值操作存在问题：

```c++
class String
{	   int len;
	   char *str;
	public:
		String(char *s) 
		{ len = strlen(s); 
		   str = new char[len+1]; 
		   strcpy(str,s); 
		}
		~String() { delete []str; str=NULL; }
};
......
String s1("xyz"),s2("abcdefg");
.......
s1 = s2;  
//赋值后，s1.str原来所指向的空间成了“孤儿”
//s1和s2互相干扰
//s1和s2消亡时，"abcdefg"所在的空间将会被释放两次！
```

解决上面问题的办法是自己定义赋值操作符重载函数：

```c++
class String
{  ......
String& operator = (const String& s)
{	if (&s == this) return *this;  //防止自身赋值。
	delete []str;
	str = new char[s.len+1];
	strcpy(str,s.str);
    len = s.len; 
	return *this;
    }
};
```

上面的返回值类型为什么是String的引用？

- 为了允许下面的操作：`(a=b)=c`

自定义的赋值操作符重载函数不会自动地去进行成员对象的赋值操作，必须要在自定义的赋值操作符重载函数中显式地指出，例如：

```c++
class A { .......};
class B
{		A a;
		int x,y;
	public:
		......
		B& operator = (const B& b)
		{	a = b.a;//调用A类的赋值操作符重载函数来实现成员对象的赋值。
			x = b.x;
			y = b.y;
			return *this;
		}
}; 
```

- 赋值操作符只能作为非静态的成员函数来重载。不能被继承。 

- 一般来讲，需要自定义拷贝构造函数的类通常也需要自定义赋值操作符重载函数。 

- 注意：要区别下面两个“=”的不同含义。

```c++
A a;
A b=a; //初始化，等价于：A b(a);，调用拷贝构造函数。
......
b = a; //赋值，调用赋值操作符=重载函数。
```



### 访问数组元素操作符`[]`的重载 

对于由具有线性关系的元素所构成的对象，可通过重载下标操作符“[]”来实现对其元素的访问。例如：

```c++
class String
{	   int len;
	   char *str;
	public:
 		......
		char &operator [](int i) {	return str[i]; 	}
};
......
String s("abcd");
... s[2] ... //访问s中的'c'
```

再例如：

```c++
class Vector  //向量类
{	   int *p_data;
	   int num;
	public:
		......
		int &operator[](int i) //访问向量第i个元素。
		{	return p_data[i];
		}
};
Vector v(10);
......
... v[2] ... //访问向量第三个元素
```

对于常量对象的场合要加上另一个重载函数：

```c++
int operator[](int i) const
{ return p_data[i];
}
```



### 操作符new与delete的重载

操作符new和delete用于创建和撤销动态对象：

```c++
A *p=new A;
......
delete p;
```

- 系统提供的new和delete操作所涉及的空间分配和释放是通过系统的堆区管理系统来进行的，效率常常不高。

- 可以对操作符new和delete进行重载，使得程序能以自己的方式来实现动态对象空间的分配和释放功能。

操作符new有两个功能：
- 为动态对象分配空间
- 调用对象类的构造函数

操作符delete也有两个功能：

- 调用对象类的析构函数
- 释放动态对象的空间 

重载操作符new和delete时，重载的是它们的分配空间和释放空间的功能，不影响对构造函数和析构函数的调用。



### 重载操作符new

操作符new必须作为静态的成员函数来重载（static说明可以不写），其格式为： 

```c++
void *operator new(size_t size); 
```

- 返回类型必须为`void *`

- 参数size表示对象所需空间的大小，其类型为`size_t（unsigned int）`

例：把动态对象初始化为全‘0’

```c++
#include <cstring>
class A
{		int x,y;
	public:
		void *operator new(size_t size)
		{	void *p = malloc(size); //调用系统堆空间分配操作。
			memset(p,0,size); //把申请到的堆空间初始化为全“0”。
			return p;
		}
		......
};
```

- 为一个没有定义任何构造函数的类的动态对象提供初始化。

重载的new操作符的使用格式与系统提供的基本相同。例如（假设A中重载了new）：

```c++
A *p=new A; 
```

- 自动计算A的大小，把它作为参数（size）去调用new的重载函数。

- 调用A类默认构造函数。

```c++
A *q=new A(...); 
```

- 自动计算A的大小，把它作为参数（size）去调用new的重载函数。
- 调用A类带参数（...）的构造函数。

重载new时，除了对象空间大小参数以外，也可以带有其它参数，

```c++
void *operator new(size_t size,…); 
```

如果重载的new包含其它参数，其使用格式为：

```c++
p = new (<1>) A(<2>); 
```

- <1> 表示提供给new重载函数的其它参数

- <2> 表示提供给A类构造函数的参数

#### 例：在非“堆区”为动态对象分配空间

```c++
#include <cstring>
class A
{		......
	public:
		A(int i) { ... }
		void *operator new(size_t size,void *p)
		{	return p;
		}
};
char buf[sizeof(A)];
A *p = new (buf) A(0);//动态对象的空间分配为buf
......
p->~A(); //使得p所指向的对象消亡。不能用系统的delete
```



### 重载delete

一般来说，如果对某个类重载了操作符new，则相应地也要重载操作符delete。

操作符delete也必须作为静态的成员函数来重载（static说明可以不写），其格式为： 

```c++
void operator delete(void *p, size_t size);
```

- 返回类型必须为void。

- 第一个参数类型为void *，指向对象的内存空间。

- 第二个参数可有可无，如果有，则必须是size_t类型。 

重载后，操作符delete的使用格式与未重载的相同。

#### 例：重载操作符new与delete来管理程序中某类动态对象的堆空间

就某一个类的动态对象而言，系统的堆空间管理效率不高：

- 系统要考虑程序中各种大小的堆空间申请和释放问题。

- 系统要处理“碎片”问题。

针对某个类的动态对象，程序可以自己管理该类对象的空间分配和释放，以提高效率。例如，

- 重载new
  - 第一次创建该类的动态对象时，先从系统管理的堆区中申请一块大的空间；
  - 把上述大空间分成若干小块，每个小块的大小为该类一个对象的大小，然后用链表来管理这些小块；
  - 在该链表上为该类对象分配空间。

- 重载delete
  - 该类的一个对象消亡时，该对象的空间归还到new操作中申请到的大空间（链表）中，而不是归还到系统的堆区中。

```c++
#include <cstring>
class A
{		...... //类A的已有成员说明。
	public:
		static void *operator new(size_t size);
		static void operator delete(void *p);
	private:
		A *next; //用于组织A类对象自由空间结点的链表。
		static A *p_free; //用于指向A类对象的自由空间链表头。
};
A *A::p_free=NULL;
const int NUM=32; 
void *A::operator new(size_t size)
{	A *p;
	if (p_free == NULL)
	{	//申请NUM个A类对象的大空间。
	  p_free = (A *)malloc(size*NUM);  
		//在大空间上建立自由结点链表。
		for (p=p_free; p!=p_free+NUM-1; p++)  
		   p->next = p+1;
		p->next = NULL;
	}
   //从链表中给当前对象分配空间
	p = p_free;
	p_free = p_free->next;
	memset(p,0,size);
	return p;
}
void A::operator delete(void *p)
{	((A *)p)->next = p_free;
	p_free = (A *)p;
}
A *q1=new A;
A *q2=new A;
delete q1;
```

>  在申请空间时，一块用完了，将会申请第二块。

#### 动态对象数组的创建与撤销

```c++
A *p=new A[10];
... p[0]、p[1]、...、p[9] ...
delete []p; //“[]”不能漏掉！
```

上述操作将调用下面隐式的重载函数：

```c++
void *operator new[](size_t size);
void operator delete[](void *p);
```

可以自定义上面的重载函数完成特殊的操作。

注意：传给函数new[]的参数size的实际值可能比对象数组需要的空间多4个字节，用于系统管理！



### 重载函数调用操作符“()” --函数对象

- 在C++中，把函数调用也作为一种操作符来看待：
  - 操作数为函数名以及各个实参！

- 重载函数调用操作符之后，相应类的对象就可当作函数来使用了。例如：

```c++
class A
{		int value;
	public:
		A() { value = 0; }
		A(int i) { value = i; }
		int operator () (int x) //函数调用操作符()的重载函数
		{ return x+value; 
		}
};
......
A a(3);
cout << a(10) << endl; //a(10)将会去调用A中的函数调用操
					//作符重载函数，10作为实参
```

函数调用操作符重载主要用于只有一个操作的对象（函数对象，functor），该对象除了具有一般函数的行为外，它还可以拥有状态。例如，下面是一个能产生随机数的对象：

```c++
class Random
{	   unsigned int seed; //状态
	public:
   	   Random(unsigned int i) { seed = i; }
       unsigned int operator ()() //函数调用操作符重载
   	   {   seed = (25173*seed+13849)%65536;
	       return seed;
       }
};
......
Random random(1); //创建一个函数对象
...random()... //利用函数对象random产生一个随机数
```

> n在C++中，λ表达式是通过函数对象来实现的。



### 重载间接类成员访问操作符“->” --智能指针

“->”为一个双目操作符，其第一个操作数为一个指向类或结构的指针，第二个操作数为第一个操作数所指向的类或结构的成员。

```c++
A a,*p=&a;  p->f();
```

通过对“->”进行重载，可以实现一种智能指针（smart pointers）：

- 一个具有指针功能的对象，通过该对象能访问所“指向”的另一个对象。

- 通过该指针对象去访问所指向的对象的成员前能做一些额外的事情。

```c++
B b(&a); b->f(); //b是个智能指针对象，指向对象a
```

#### 例：在程序执行的某个时刻获得某个对象被访问的次数

```c++
class A
{		int x,y;
	public:
		void f();
		void g();
};
void func(A *p)
{  ...... p->f(); ...... p->g(); ......
}
......
A a;
func(&a); 
...... //调用完func后，如何知道在func中访问了a多少次?
```

一种解决方案：

```c++
class A
{	int x,y;
	int count;
public:
	A() { count = 0; ... }
	void f() { count++; ... }
	void g() { count++; ... }
	int num_of_access() const { return count; }
};
void func(A *p) {  ...... p->f(); ...... p->g(); ...... }
......
A a;
func(&a);
... a.num_of_access() ... //获得对a的访问次数
```

问题：

- 要修改类A，每个成员函数都要加上：`count++;`

- 如果类A中有外界可访问的数据成员，怎么处理？

另一种解决方案：

```c++
class B  //智能指针类
{		A *p_a;
		int count;
	public:
		B(A *p) 
		{	p_a = p; count = 0; 
		}
		A *operator ->()  //操作符“->”的重载函数，按单目操作符重载！
		{	count++;  return p_a; 
		}
		int num_of_a_access() const
		{	return count; 
		}
};
void func(B &p) //注意：p是个B类对象！
{  ...... p->f(); ...... p->g(); ......
}
//p->f(); //等价于：p.operator->()->f(); 

A a;
B b(&a);  //b为一个智能指针对象，它指向了a
func(b);
... b.num_of_a_access() ... //获得对a的访问次数
```

为了完全模拟普通的指针功能，针对智能指针类，还可以重载`*`（对象间接访问）、`[]`、`+`、`-`、`++`、`--`、......等操作符：

```c++
class B  //智能指针类
{		 A *p_a;
		......
		A& operator *()
		{	return *p_a; 
		}
		A& operator [](int i)
		{	return p_a[i]; 
		}
		......
};
```



### 自定义类型转换操作符 

类中带一个参数的构造函数可以用作从其它类型到该类的转换。 例如：

```c++
class Complex
{		double real, imag;
	public:
		Complex() { real = 0; imag = 0; }
		Complex(double r)  {	real = r; imag = 0; }
		Complex(double r, double i) { real = r; imag = i; }
		......
    friend Complex operator + (const Complex& x, const Complex& y);
}; 
......
Complex c1(1,2),c2,c3;
c2 = c1 + 1.7;  //1.7隐式转换成一个复数对象Complex(1.7)
c3 = 2.5 + c2;  //2.5隐式转换成一个复数对象Complex(2.5)
```

可自定义类型转换，从一个类转换成其它类型，例如：

```c++
class A
{  int x,y;
  public:
   ......
   operator int() { return x+y; }  //类型转换操作符int的重载函数
};
...…
A a;
int i=1;
int z = i + a; //将调用类型转换操作符int的重载函数把对象a隐式转换成int型数据。
```

#### 歧义问题

```c++
class A
{		int x,y;
	public:
		A() { x = 0;  y = 0; }
		A(int i) { x = i; y = 0; }
      A(int i,int j) { x = i;  y = j; }
		operator int() { return x+y; }
	friend A operator +(const A &a1, const A &a2);
};
......
A a;
int i=1,z;
z = a + i;  //是a转换成int呢，还是i转换成A？ 
```

对上面的问题，可以用显式类型转换来解决：

```c++
z = (int)a + i;
//或
z = a + (A)i;
```

也可以通过给A类的构造函数A(int i)加上一个修饰符explicit，禁止把它当作隐式类型转换符来用：

```c++
class A
{	......
   explicit A(int i) //禁止把它当作隐式类型转换符来用。 
   { x = i; y = 0; 
	}
   operator int() { return x+y; }
	......
};
A a;
int i=1,z;
z = a + i;  //a转换成int
```



## Lecture 12 类成员指针

### 操作符`.*`和`->*`

`a.*p`

- a是一个对象（或结构）

- p是一个指针，它指向a所属类的某个成员

- 含义：访问a的由p指向的成员

`pa->*p`

- pa是一个指向对象（或结构）的指针

- p是一个指针，它指向*pa所属类的某个成员

- 含义：访问pa所指向的对象的由p指向的成员

p的类型如何定义？



### 类成员指针

```c++
class A
{ public:
	 int x;
	 void f();
};
int *p_x = &A::x; //Error，A::x没有内存空间
void (*p_f)() = &A::f; //Error，A::f有个参数this
```

```c++
class A
{ public:
	 int x;
	 void f();
};
int A::*pm_x = &A::x; //OK
void (A::*pm_f)() = &A::f; //OK
//上面的pm_x和pm_f分别指向A的成员x和f
```



### 通过类成员指针访问对象的成员

```c++
A a;
a.*pm_x = 0; //相当于：a.x = 0;
(a.*pm_f)();  //相当于：a.f(); 
......
A *p=new A;
p->*pm_x = 0; //相当于：p->x = 0;
(p->*pm_f)(); //相当于：p->f();
```



### 类成员指针的应用

```c++
class A
{  ......
  public:
    void f1(int x);
    void f2(int x);
    void f3(int x);
    .......
};
typedef void (A::*PM_F)(int);
PM_F f_list[3]={&A::f1,&A::f2,&A::f3} //类成员函数表
......
A a;
int i,n;
......
(a.*f_list[i])(n); //访问a的由f_list[i]指向的成员函数
```



### 操作符`->*`的重载

操作符`.`和`.*`是不能重载的

- 第一个操作数本来就是类（结构）类型对象！

操作符`->`和`->*`是可以重载的

- 使得第一个操作数可以是类（结构）类型的对象（本来是指针！）

操作符 `->*`可按普通的双目操作符重载，语法上没特殊要求：

- 第一个操作数是类（结构）

- 第二个操作数类型不限。



## Lecture 13 λ表达式

### 问题的提出

对于下面求定积分的函数：

```c++
double integrate(double (*f)(double), double a, double b);
```

如果要求函数`x^2`在区间[0,1]的定积分，则一般要先定义一个函数square：

```c++
double square(double x) { return x*x; }
```

然后再把它作为参数去调用integrate：

```c++
integrate(square,0,1);
```

如果还要再求函数`x^3` 、......等一些在程序中很少使用的简单函数的定积分呢？



### 匿名函数--λ表达式

对于一些临时用一下的简单函数，如果也要先给出这个函数的定义并为之取个名字，然后再通过这个函数的名字来使用它们，在有些场合下会给程序编写带来不便。

C++新国际标准（C++11）为C++提供了一种匿名函数机制――λ表达式（lambda expression），利用它可以实现把函数的定义和使用合而为一。

例如，求函数x2在区间[0,1]的定积分：

```c++
integrate([](double x)->double { return x*x; },0,1);
//上面第一个参数就是一个λ表达式
```



### λ表达式的定义格式

λ表达式的常用格式为：`[<环境变量使用说明>]<形式参数><返回值类型指定><函数体>`

- `<形式参数>`：指出函数的参数及类型，其格式为：`(<形式参数表>)`
  - 如果函数没有参数，则这项可以省略。

- `<返回值类型指定>`：指出函数的返回值类型，其格式为：`-> <返回值类型>`
  - 它可以省略，这时根据函数体中return返回的值隐式确定返回值类型。

- `<函数体>`为一个复合语句。

- `<环境变量使用说明>`：指出函数体中对外层作用域中的自动变量的使用限制：
  - 空：不能使用外层作用域中的自动变量。
  - `&`：按引用方式使用外层作用域中的自动变量（可以改变这些变量的值）。
  - `=`：按值方式使用使用外层作用域中的自动变量（不能改变这些变量的值）。
  - &和=可以用来统一指定对外层作用域中自动变量的使用方式，也可以用来单独指定可使用的外层自动变量（变量名前可以加&，默认为=）。

下面是一些合法的λ表达式：

```c++
{  int k,m,n; //环境变量
   ......
   ...[](int x)->int { return x*x; }... //不能使用k、m、n
   ...[&](int x)->int { k++; m++; n++; 
               return x+k+m+n; }... //k、m、n可以被修改
   ...[=](int x)->int { return x+k+m+n; }... //k、m、n不能被修改
   ...[&,n](int x)->int { k++; m++; 
                         return x+k+m+n; }... //n不能被修改
   ...[=,&n](int x)->int { n++; return x+k+m+n; }... //n可以被修改
   ...[&k,m](int x)->int { k++; return x+k+m; }...   //只能使用k和m，k可以被修改
   ...[=] { return k+m+n; }... //没有参数，返回值类型为int
}
```

- λ表达式通常用于把一个匿名函数作为参数传给另一个函数的场合。

- 不使用环境变量的λ表达式可隐式转换成函数指针。



### λ表达式的实现

在C++中，λ表达式是通过函数对象来实现的：

- 对于λ表达式：`[...](int x)->int { ....... }`

- 首先，隐式定义一个类：
  - 数据成员对应用到的环境变量，用构造函数对其初始化。
  - 重载了函数调用操作符，重载函数按相应λ表达式的功能来实现。

- 然后，创建上述类的一个临时对象（设为obj）

- 最后，在使用上述λ表达式的地方用该对象来替代：
  - 作用于实参进行函数调用

    ```c++
    cout << obj(3);
    ```

  - 传给其它函数

    ```c++
    f(obj);
    ```



## Lecture 14 继承——派生类

### 继承的基本概念

在开发一个新软件时，把现有软件或软件的一部分拿过来用称为**软件复用**。

目前，不加修改地直接复用已有软件比较困难。已有软件的功能与新软件所需要的功能总是有差别的。解决这个差别有下面的途径：

- 修改已有软件的源代码，它的缺点是：
  - 需读懂源代码，可靠性差、易出错，有时源代码难以获得

- **继承机制**（Inheritance）：
  - 在定义一个新的类时，先把已有的一个或多个类的功能全部包含进来，然后在新的类中再给出新功能的定义或对已有类的某些功能重新定义。
  - 不需要已有软件的源代码，属于目标代码复用！

### 基类与派生类

在继承关系中存在两个类：**基类**（或称父类）**和派生类**（或称子类）。派生类拥有基类的所有成员，并可以

- 定义新的成员，或

- 对基类的一些成员（成员函数）进行重定义。 

继承分为：单继承和多继承

- 单继承：一个类只有一个直接基类。

- 多继承：一个类有多个直接基类。



### 继承对程序设计的支持

继承机制除了支持软件复用外，它还具有下面的作用： 

- 对处理的对象按层次进行分类。
  - 有利于问题的描述和解决。
- 对概念进行组合
  - 给新类的设计带来便利。 

- 支持软件的增量开发。（版本升级）
  - 这给软件开发和维护带来便利。



### 单继承

在定义单继承时，派生类只能有一个直接基类，其定义格式如下：

```c++
class <派生类名>：[<继承方式>] <基类名>
{ <成员说明表>
}; 
```

- `<派生类名>`为派生类的名字。

- `<基类名>`为直接基类的名字。

- `<成员说明表>`是在派生类中新定义的成员，其中包括对基类成员的重定义。

- `<继承方式>`用于指出从基类继承来的成员在派生类中对外的访问控制 

```c++
class A //基类
{		int x,y;
	public:
		void f();
		void g();
};
class B: public A //派生类
{		int z; //新成员
	public:
		void h(); //新成员
};
```

派生类除了拥有新定义的成员外，还拥有基类的所有成员（基类的构造函数、析构函数和赋值操作符重载函数除外）。例如：

```c++
class A //基类
{		int x,y;
	public:
		void f();
		void g();
};
class B: public A //派生类
{		int z; //新成员
	public:
		void h(); //新成员
};
```

```c++
b.f();   //A类中的f
b.g();  //A类中的g
b.h();  //B类中的h 
```

定义派生类时一定要见到基类的定义。

```c++
class A;  //声明
class B: public A  //Error
{  int z;
  public:
   void h() { g(); }  //Error，编译程序不知道基类中
				       //是否有函数g以及函数g的原型。
};
......
B b; //Error，编译无法确定b所需内存空间的大小。
```

如果在派生类中没有显式说明，基类的友元不是派生类的友元；如果基类是另一个类的友元，而该类没有显式说明，则派生类也不是该类的友元。



### 在派生类中访问基类成员

C++中，派生类不能直接访问基类的私有成员。

```c++
class A
{		int x,y;
	public:
		void f();
		void g() { ... x ，y ... }
};
class B: public A
{		int z;
	public:
		void h() 
		{	... x，y ...  //Error，x、y为基类的私有成员。
			f();  //OK
			g();  //OK，通过函数g访问基类的私有成员x和y。
		}
}; 
```



### 继承与封装的矛盾 

在派生类中定义新的成员函数或对基类已有成员函数重定义时，往往需要直接访问基类的一些private成员（特别是private数据成员），否则新的功能无法实现。

类的private成员是不允许外界使用的（数据封装）！

这样就带来了继承与封装的矛盾。

实际上，有了继承机制以后，一个类的成员有两种被外界使用的场合：

- 通过类的对象（实例）使用

- 在派生类中使用

```c++
class A
{ ......
    m
};
class B: public A
{ ......
   f() { ... m ...} //通过派生类使用A的成员m
};
void g()
{ A a;
   ... a.m ... //通过A的对象（实例）使用A的成员m
}
```



### 在派生类中访问基类成员（protected访问控制）

在C++中，除了`public`和`private`，还提供了另外一种类成员访问控制：`protected`，

- 用`protected`说明的成员不能通过对象使用，但可以在派生类中使用。

- `protected`访问控制缓解了封装与继承的矛盾

C++类向外界提供两种接口：

- `public`：对象的使用者（类的实例用户）

- `public`+`protected`：派生类

```c++
class A
{	protected:
		int x,y;
	public:
		void f();
};
class B: public A
{		......
		void h()
		{	f();  //OK
			... x ...  //OK
			... y ...  //OK
		}
};
void g()
{	A a;
	a.f();  //OK
	... a.x ...  //Error
	... a.y ...  //Error
}
```

- 引进protected成员访问控制后，基类的设计者必须要慎重地考虑应该把那些成员声明为protected。

- 一般情况下，应该把今后不太可能发生变动的、有可能被派生类使用的、不宜对实例用户公开的成员声明为protected！



### 派生类成员标识符的作用域

派生类对基类成员的访问除了受到基类的访问控制的限制以外，还要受到标识符作用域的限制。

对基类而言，派生类成员标识符的作用域是**嵌套**在基类作用域中的。

```c++
class A
{  ......
    void f() { g(); } //Error!
    ......
}

class B:public A
{ ......
   void g() { f(); } //OK
}
```

如果派生类中定义了与基类同名的成员，则基类的成员名在派生类的作用域内不直接可见（被隐藏，Hidden）。访问基类同名成员时要用基类名受限。例如:

```c++
class A //基类
{		int x,y;
	public:
		void f();
		void g();
};

class B: public A
{		int z;
  public:
		void f();
		void h()
		{	f();  //B类中的f
			A::f();  //A类中的f
		}
};
```

即使派生类中定义了与基类同名但参数不同的成员函数，基类的同名函数在派生类的作用域中也是不直接可见的，可以用基类名受限方式来使用之：

```c++
class A //基类
{		int x,y;
	public:
		void f();
		void g();
};

class B: public A
{		int z;
	public:
		void f(int); //不是重载A的f！ 
		void h() 
		{	f(1);  //OK
			f();  //Error
			A::f();  //OK
		}
};
```

也可以在派生类中使用`using`声明把基类中某个的函数名对派生类开放：

```c++
class A //基类
{		int x,y;
	public:
		void f();
		void g();
};

class B: public A
{		int z;
	public:
		using A::f;
		void f(int); 
		void h() 
		{	f(1);  //OK
			f();  //OK，等价于A::f();
		}
};
```



### 基类成员在派生类中对外的访问控制（继承方式）

在C++中，派生类拥有基类的所有成员。问题是：

- 基类的成员在派生类中对外的访问控制是什么？即，

- 派生类的用户能访问基类的哪些成员？

上面的问题由基类的访问控制与继承方式共同决定。继承方式在定义派生类时指定：

```c++
class <派生类名>：[<继承方式>] <基类名>
{ <成员说明表>
};
```

- 继承方式可以是：public、private和protected。

- 默认的继承方式为：private。



### 继承方式的含义

| 基类成员    派生类  继承方式 | public    | private      | protected |
| ---------------------------- | --------- | ------------ | --------- |
| public                       | public    | 不可直接访问 | protected |
| private                      | private   | 不可直接访问 | private   |
| protected                    | protected | 不可直接访问 | protected |

```c++
class A
{	public:
		void f();
	protected:
		void g();
	private:
		void h();
};
class B: public A
{ //f为public
   //g为protected
   //h为不可直接访问
   public:
		void q() 
		{ f(); //?
		   g(); //?
		   h(); //?
		}
};

class C: public B
{	public:
		void r()
		{	f();  //OK
			g();  //OK
			h();  //Error
		     q();  //?
     }
};
void func()
{ B b;
   b.f();  //OK
   b.g(); //Error
   b.h(); //Error
   b.q(); //?
}
```

> ?跟B的继承方式无关！

继承方式的调整

```c++
class A
{  public:
	void f1();
	void f2();
	void f3();
  protected:
	void g1();
	void g2();
	void g3();
};
class B: private A
{ public:
	A::f1;  //把f1调整为public
	A::g1;  //把g1调整为public
		 //是否允许弱化基类的访问控制要视具体的实现而定
  protected:
	A::f2; //把f2调整为protected
	A::g2; //把g2调整为protected
   ......
};
```



### 子类型

对用类型T表达的所有程序P，当用类型S去替换程序P中的所有的类型T时，程序P的功能不变，则称类型S是类型T的子类型。

- 类型T的操作也适合于类型S。

- 在需要T类型数据的地方可以用S类型的数据去替代（类型S的值可以赋值或作为函数参数传给T类型变量）。 

在C++中，把类看作类型，把以public方式继承的派生类看作是基类的子类型。

- 对基类对象能实施的操作也能作用于派生类对象。

- 在需要基类对象的地方可以用派生类对象去替代（派生类对象可以赋值或作为函数参数传给基类变量） 。

对于下面的两个类A和B

```c++
class A //基类
{	  int x,y;
	public:
	  void f() { x++; y++; }
	  ......
};
class B: public A //派生类
{	  int z;
	public:
	  void g() { z++; } 
     ......
};
```

下面的操作是合法的：

```c++
A a;
B b;
b.f(); //OK，基类的操作可以实施到派生类对象
a = b;  //OK，派生类对象可以赋值给基类对象，属于派生类
	      //但不属于基类的数据成员将被忽略
A *p = &b;  //OK，基类指针变量可以指向派生类对象
......
void func1(A *p); 
void func2(A &x);
void func3(A x);
func1(&b); func2(b); func3(b); //OK
```

下面的操作是不合法的：

```c++
A a;
B b;
a.g(); //Error，a没有g这个成员函数。
b = a;  //Error，它将导致b有不一致的成员数据
		//（a中没有这些数据）。
B *q = &a;  //Error，“q->g();”会修改不属于a的数据！
......
void func1(B *p); 
void func2(B &x);
void func3(B x);
func1(&a); func2(a); func3(a); //Error
```



### 派生类对象的初始化和消亡处理

派生类对象的初始化由基类和派生类共同完成：

- 从基类继承的数据成员由基类的构造函数初始化；

- 派生类的数据成员由派生类的构造函数初始化。

当创建派生类的对象时，

- 先执行基类的构造函数，再执行派生类构造函数。

- 默认情况下，调用基类的默认构造函数，如果要调用基类的非默认构造函数，则必须在派生类构造函数的成员初始化表中指出。

包含成员对象的对象消亡时，

- 先调用本身类的析构函数，执行完后会自动去调用基类的析构函数。

```c++
class A
{		int x;
	public:
		A() { x = 0; }
		A(int i) { x = i; }
};
class B: public A
{		int y;
	public:
		B() { y = 0; }
		B(int i) { y = i; }
		B(int i, int j):A(i) { y = j; }
};
......
B b1;  //执行A::A()和B::B()，b1.x等于0，b1.y等于0。
B b2(1);  //执行A::A()和B::B(int)，b2.x等于0，b2.y等于1。
B b3(1,2);  //执行A::A(int)和B::B(int,int)，b3.x等于1，
			//b3.y等于2。
```

对未提供任何构造函数的派生类，编译程序会隐式地为之提供一个默认构造函数，其作用是负责调用基类的构造函数。

对未提供析构函数的派生类，编译程序也会隐式地为之提供一个析构函数，该析构函数的作用是负责调用基类的析构函数。

如果一个类D既有基类B、又有成员对象类M，则

- 在创建D类对象时，构造函数的执行次序为：B->M->D

- 当D类的对象消亡时，析构函数的执行次序为：D->M->B

派生类拷贝构造函数：

- 派生类的隐式拷贝构造函数（由编译程序提供）将会调用基类的拷贝构造函数。

- 派生类自定义的拷贝构造函数在默认情况下则调用基类的默认构造函数。需要时，可在派生类自定义拷贝构造函数的“成员初始化表”中显式地指出调用基类的拷贝构造函数。 

```c++
class A {	...... };
class B: public A
{		......
	public:
B() { ...... }
		B(const B& b):A(b)  //调用A类的拷贝构造函数
		{	...... //用b对this的派生类成员进行初始化
			
		}
}; 
B b1;
B b2(b1); 
```



### 派生类对象的赋值操作

派生类隐式的赋值操作除了对派生类成员进行赋值外，还将调用基类的赋值操作对基类成员进行赋值。

派生类自定义的赋值操作符重载函数不会自动调用基类的赋值操作，需要在自定义的赋值操作符重载函数中显式地指出。

```c++
class A {	...... };
class B: public A
{		......
	public:
		B& operator =(const B& b)
		{	if (&b == this) return *this;  //防止自身赋值。
			 *(A*)this = b; //调用基类的赋值操作符对基类成员
						          //进行赋值。也可写成： 
						          //this->A::operator =(b); 
			...... //对派生类的成员赋值
			return *this;
		}
}; 
......
B b1,b2;
b1 = b2;
```



### 实例：一个公司中的职员类和部门经理类的设计。 

```c++
class Employee //普通职员类
{		String name; //String为字符串类。
		int salary;
	public:
		Employee(const char *s, int n=0):name(s) 
		{	salary = n; 
		}
		void set_salary(int n) { salary = n; }
		int get_salary() const { return salary; }
		String get_name() const { return name; }
};

const int MAX_NUM_OF_EMPS=20;
class Manager: public Employee //部门经理类
{		Employee *group[MAX_NUM_OF_EMPS];
		int num_of_emps;
	public:
		Manager(const char *s, int n=0): Employee(s,n) 
		{ num_of_emps = 0; 
		}
		bool add_employee(Employee *e);
		bool remove_employee(Employee *e); 
		int get_num_of_emps() { return num_of_emps; }
};

Manager m("Mark",4000); //创建一个经理对象Mark
cout << "Mark's salary is " << m.get_salary() << '.' << endl; //显示经理Mark的工资
Employee e1("Jack",1000),e2("Jane",2000); //创建职员对象Jack和Jane
m.add_employee(&e1); //把职员Jack纳入经理Mark的管理
m.add_employee(&e2); //把职员Jane纳入经理Mark的管理
cout<< "Number of employees managed by Mark is " << m.get_num_of_emps() << '.' << endl; //显示经理Mark的管理人数
m.remove_employee(&e1); //职员Jack脱离经理Mark的管理
......
```



## Lecture 15 消息的动态绑定

### 消息的多态性

消息的多态性体现为：相同的一条消息可以发送到不同类的对象，从而会得到不同的解释。

对于具有public继承关系的两个类，一条可以发送到基类对象的消息，也可以发送到派生类对象，如果在基类和派生类中都给出了对这条消息的处理，那么这条消息就存在多态性。

由于基类的指针或引用可以指向或引用基类对象，也可以指向或引用派生类对象：

- 当通过基类的指针或引用向它指向或引用的对象发送消息时，就存在对消息（成员函数调用）的绑定问题。

- C++默认采用的是静态的消息绑定：采用基类的消息处理。

```c++
class A
{	 int x,y;
  public:
	 void f();
};
class B: public A
{	 int z;
  public:
   	void f(); 
   	void g();
};
```

```c++
void func1(A& x)
{	......
	x.f(); //调用A::f
	......
}
void func2(A *p)
{	......
	p->f(); //调用A::f
	......
}
......
A a;
func1(a);
func2(&a);
B b;
func1(b);
func2(&b);
```



### 消息的动态绑定

- 一般情况下，需要在func1（或func2）中根据x（或p）实际引用（或指向）的对象来决定是调用A::f还是B::f，即采用动态绑定。

- 在C++中，在基类中用虚函数来指出动态绑定。

```c++
class A
{		int x,y;
	public:
		virtual void f(); //虚函数
};
class B: public A
{		int z;
	public:
   		void f(); 
   		void g();
};
```

```c++
void func1(A& x)
{	......
	x.f(); //调用A::f或B::f
	......
}
void func2(A *p)
{	......
	p->f(); //调用A::f或B::f
	......
}
......
A a;
func1(a); //在func1中调用A::f
func2(&a); //在func2中调用A::f
B b;
func1(b); //在func1中调用B::f
func2(&b); //在func2中调用B::f
```



### 虚函数

虚函数是指加了关键词virtual的成员函数。其格式为：

```c++
virtual <成员函数声明>;
```

虚函数有两个作用：

- 指定消息采用动态绑定。

- 指出基类中可以被派生类重定义的成员函数。



### 在派生类中重定义基类的成员函数

对于基类中的一个虚函数，在派生类中定义的、与之具有相同型构的成员函数是对基类该成员函数的重定义（或称覆盖，override）。

相同的型构是指：

- 派生类中定义的成员函数的名字、参数个数和类型与基类相应成员函数相同；

- 其返回值类型与基类成员函数返回值类型或者相同，或者是基类成员函数返回值类型的public派生类。

```c++
class A
{	public:
      A f();
      void g();
};
class B: public A
{	public:
	  A f(); //新定义的成员（与A的f无关，同名而已！）
	  void f(int); //新定义的成员
     void h(); //新定义的成员
};
```

```c++
class A
{	public:
      virtual A f();
      void g();
};
class B: public A
{	public:
	  A f(); //对A类中成员f的重定义。返回类型也可为B。
	  void f(int); //新定义的成员
     void h(); //新定义的成员。
};
```



### 对虚函数几点说明

只有类的成员函数才可以是虚函数，但静态成员函数不能是虚函数。

构造函数不能是虚函数，析构函数可以（往往）是虚函数。

只要在基类中说明了虚函数，在派生类、派生类的派生类、...中，同型构的成员函数都是虚函数（virtual可以不写）。

只有通过指针或引用访问类的虚函数时才进行动态绑定。

类的构造函数和析构函数中对虚函数的调用不进行动态绑定。



### 消息动态绑定的各种情况

```c++
class A
{  ......
  public:
	A() { f(); }
	~A() { f(); }
	virtual void f();
	void g();
	void h() { f(); g(); }
};
class B: public A
{  .......
 public:
   B() { ...... }
	~B();
	void f(); 
	void g(); 
};
```

```c++
......
A a;  //调用A::A()和A::f
a.f();  //调用A::f
a.g();  //调用A::g
a.h();  //调用A::h、A::f和A::g
//a消亡时会调用A::~A()和A::f

B b;  //调用B::B()、A::A()和A::f
b.f();  //调用B::f
b.g();  //调用B::g
b.h();  //调用A::h、B::f和A::g
//b消亡时会调用B::~B()、A::~A()和A::f
A *p;   //p是A类（基类）指针
p = &a; //p指向A类对象
p->f();  //调用A::f
p->g();  //调用A::g
p->h();  //调用A::h, A::f和A::g
p = &b;  //p指向B类对象
p->f();  //调用B::f
p->A::f(); //调用A::f
p->g();  //调用A::g，非虚函数采用静态绑定
p->h();  //调用A::h, B::f和A::g
p = new B;  //调用B::B(), A::A()和A::f
.......
delete p;  //只调用A::~A()和A::f ，
               //没调用B:~B()，为什么？
                 //没有把A的析构函数定义为虚函数！
```



### 通过基类指针访问派生类中新定义的成员

```c++
class A
{	      .......
   public:
		virtual void f();
};
class B: public A
{	      ......
    public:
		void f(); 
		void g(); 
};
A *p=new B;
......
p->f(); //OK
p->g(); //Error
((B *)p)->g(); //OK，不安全！p指向的可能不是B类对象！
```

```c++
//安全做法
B *q=dynamic_cast<B *>(p);
if (q != NULL) q->g();
//需要运行时刻的类型信息（RTTI）支持
```



### 何时需要定义虚函数？

基类中哪些成员函数需要设计成虚函数？

- 在设计基类时，有时虽然给出了某些成员函数的实现，但实现的方法可能不是最好，今后可能还会有更好的实现方法。

- 在基类中根本无法给出某些成员函数的实现，它们必须由不同的派生类根据实际情况给出具体的实现。（纯虚函数）



### 虚函数动态绑定的实现

```c++
class A
{	int x,y;
   public:
	virtual void f();
	virtual void g();
	void h();
};
class B: public A
{	int z;
   public:
	void f();
	void h();
};

void f(A *p) //可以接收A和B类对象
{ p->h(); //编译成：A::h(p);
   p->f();  //编译成：(*(*(VtblPtr*)p))(p);
   p->g(); //编译成：(*(*(VtblPtr*)p+1))(p);
}
```



## Lecture 16 抽象类

### 纯虚函数

纯虚函数是没给出实现的虚函数，函数体用“=0”表示， 例如：

```c++
class A
{	......
	public:
		virtual int f()=0; //纯虚函数
	......
};
```

纯虚函数需要在派生类中给出实现。



### 抽象类 

包含纯虚函数的类称为抽象类。

抽象类不能用于创建对象。 例如：

```c++
class A //抽象类
{	......
	public:
		virtual int f()=0; //纯虚函数
	......
};
......
A a;  //Error，A是抽象类
```

抽象类的作用是为派生类提供一个基本框架和一个公共的对外接口。



### 例：用抽象类为各种图形类提供一个基本框架

```c++
class Figure //抽象基类
{	public:
		virtual void draw() const=0;
		virtual void input_data()=0;
};
class Rectangle: public Figure
{		double left,top,right,bottom;
	public:
		void draw() const
		{	...... //画矩形
		}
		void input_data()
		{	cout << "请输入矩形的左上角和右下角坐标 (x1,y1,x2,y2) ：";
			cin >> left >> top >> right >> bottom;
		}
		double area() const 
	 { return (bottom-top)*(right-left); }
};

const double PI=3.1416;
class Circle: public Figure
{		double x,y,r;
	public:
		void draw() const
		{	...... //画圆
		}
		void input_data()
		{	cout << "请输入圆的圆心坐标和半径 (x,y,r) ：";
			cin >> x >> y >> r;
		}
		double area() const { return r*r*PI; }
};

class Line: public Figure
{		double x1,y1,x2,y2;
	public:
		void draw() const
		{	...... //画线
		}
		void input_data()
		{	cout << "请输入线段的起点和终点坐标 (x1,y1,x2,y2) ：";
			cin >> x1 >> y1 >> x2 >> y2;
		}
};
......
const int MAX_NUM_OF_FIGURES=100;
Figure *figures[MAX_NUM_OF_FIGURES];
int count=0;
```

#### 图形数据的输入：

```c++
for (count=0; count<MAX_NUM_OF_FIGURES;	count++)
{	int shape;
	do
	{	cout << "请输入图形的种类(0：线段，1：矩形，2：圆，-1：结束)：";
		cin >> shape;
	} while (shape < -1 || shape > 2);
	if (shape == -1) break;
	switch (shape)
	{	case 0: //线
			figures[count] = new Line;	break;
		case 1: //矩形
			figures[count] = new Rectangle; break;
		case 2: //圆
			figures[count] = new Circle; break;
 	}
	figures[count]->input_data(); //动态绑定到相应类的input_data
}
```

#### 图形的输出：

```c++
for (int i=0; i<count; i++) 
	figures[i]->draw(); 
	//通过动态绑定调用相应类的draw
```

即使今后增加了图形的种类，

- 上述代码（属于高层代码）不需要改动！

- 只需要增加相应的类（属于低层代码）就行。

- 这体现了多态带来的好处：实现高层代码的复用

### 用联合类型实现

```c++
struct Line { double x1,y1,x2,y2; };
struct Rectangle { double left,top,right,bottom; };
struct Circle { double x,y,r; };
union Figure
{	Line line;
	Rectangle rect;
	Circle circle; 
};
struct TaggedFigure
{	int shape;
	Figure figure; 
};
const int MAX_NUM_OF_FIGURES=100;
TaggedFigure *figures[MAX_NUM_OF_FIGURES];
void input_data(Line &line)
{ cout << "请输入线段的起点和终点坐标 (x1,y1,x2,y2) :";
   cin >> line.x1 >> line.y1 >> line.x2 >> line.y2;
}
void input_data(Rectangle &rect)
{ cout << "请输入矩形的左上角和右下角坐标 (x1,y1,x2,y2) ：";
   cin >> rect.left >> rect.top >> rect.right >> rect.bottom;
} 
void input_data(Circle &circle)
{ cout << "请输入圆的圆心坐标和半径 (x,y,r) ：";
   cin >> circle.x >> circle.y >> circle.r;
}
void draw(Line &line) { ...... } 
void draw(Rectangle &rect) { ...... }
void draw(Circle &circle) { ...... }
double area(Rectangle &rect) 
{ return (rect.bottom- rect.top)*(rect.right- rect.left); 
}
double area(Circle &circle)
{ return circle.r*circle.r*PI;
}
```

#### 图形数据的输入：

```c++
int count;
for (count=0; count<MAX_NUM_OF_FIGURES; count++)
{	int shape;
	do
	{	cout << "请输入图形的种类(0:线段,1:矩形,2:圆,-1:结束):";
		cin >> shape;
	} while (shape < -1 || shape > 2);
	if (shape == -1) break;
	figures[count] = new TaggedFigure; //空间利用效率不高！
	switch (shape)
	{ 	case 0: //线
			figures[count]->shape = 0;
			input_data(figures[count]->figure.line);
  			break;
	  	case 1: //矩形
			figures[count]->shape = 1;
			input_data(figures[count]->figure.rect);
 			break;
 		case 2: //圆形
			figures[count]->shape = 2;
			input_data(figures[count]->figure.circle);
  	 		break;
	 	} //end of switch
	} //end of for
```

#### 图形的输出：

```c++
for (int i=0; i<count; i++)
{	switch (figures[i]->shape)
		{ case 0:
					draw(figures[i]->figure.line);
					break;
			case 1:
					draw(figures[i]->figure.rect);
					break;
			case 2:
					draw(figures[i]->figure.circle);
 					break;
 		}
}
```

> 增加新的图形种类时，需要修改上述代码（增加分支）。



### 例：用抽象类实现抽象数据类型Stack 

抽象数据类型（abstract data type）是指只考虑类型的抽象性质，而不考虑该类型的实现。

例如，从抽象数据类型的角度来讲，栈类型是由若干具有线性关系的元素构成，它包含两个操作push和pop：

- `s.push(a).pop(x)`操作之后，条件`x == a`成立。

- 上面就是栈这个抽象数据类型所包含的全部内容，它与栈的内部是如何实现的无关。

C++把抽象数据类型与抽象数据类型的实现两者合二为一了，它们都用类来表示。这样就带来了问题：

- 对于一个用数组实现的栈类`ArrayStack`和一个用链表实现的栈类`LinkedStack`，虽然从抽象数据类型的角度看，它们是相同的类型，但在C++中它们却是不同的类型。

- 对于函数：`void f(T& st);`，如果要求它既可以接受`ArrayStack`类的对象，也可以接受`LinkedStack`类的对象，那么，它的参数T的类型怎么表示？

可以为`ArrayStack`和`LinkedStack`提供一个抽象基类：

```c++
class Stack
{	public:
		virtual void push(int i)=0;
		virtual void pop(int& i)=0;
};
```

```c++
class ArrayStack: public Stack
{		int elements[100],top;
  public:
   		ArrayStack() { top = -1; }
		void push(int i) { ......}
		void pop(int& i) { ...... }
};
class LinkedStack: public Stack
{		struct Node
		{	int content;
			Node *next;
		} *first;
	public:
		LinkedStack() { first = NULL; }
		void push(int i) { ......}
		void pop(int& i) { ...... }
};
```

```c++
void f(Stack *p)
{	......
	p->push(...);  //将根据p所指向的对象类来确定
			       //push的归属。
	......
	p->pop(...);  //将根据p所指向的对象类来确定
			      //pop的归属。
	......
}
int main()
{	ArrayStack st1;
	LinkedStack st2;
	f(&st1);  //OK
	f(&st2);  //OK
	......
}
```



### 用抽象类实现类的真正抽象作用

在C++中把抽象数据类型与抽象数据类型的实现合而为一都用类来表示。

由于在C++中使用某个类时必须要见到该类的定义，因此，使用者能够见到该类的非public成员，这样就有手段绕过类的访问控制而使用类的非public成员。

例如：

```c++
//A.h （类A的对外接口）
class A
{	int i,j;
  public:
	A();
	A(int x,int y);
	void f(int x);
};

//A.cpp （类A的实现，不公开）
#include "A.h"
void A::A() { ...... }
void A::A(int x,int y) { ...... }
void A::f(int x) { ...... }
......

//B.cpp （A类对象的某个使用者）
#include "A.h"
void func(A *p)  //绕过对象类的访问控制！
{	p->f(2); //Ok
	p->i = 1; //Error
	p->j = 2; //Error
	*((int *)p) = 1; //Ok，访问p所指向的对象的成员i
	*((int *)p+1) = 2; //Ok，访问p所指向的对象的成员j
}
```

如何防止上面的情况？

**用抽象类I_A给类A提供一个抽象接口**

```c++
//I_A.h （类A的对外接口）
class I_A
{ public:
   	  virtual void f(int)=0;
};

//A.cpp （类A的实现，不公开）
#include "I_A.h"
class A: public I_A
{	int i,j;
   public:
	A();
	A(int x,int y);
	void f(int x);
};
void A::A() { ...... }
void A::A(int x,int y) { ...... }
void A::f(int x) { ...... }
......

//B.cpp （A类对象的某个使用者）
#include "I_A.h"
void func(I_A *p)
{	p->f(2);  //Ok

	......  //这里不知道p所指向的对象有哪些数据成员，
		   //因此，无法访问它的数据成员
}
```



## Lecture 17 多继承

### 多继承的需要性

对于下面的两个类A和B：

```c++
class A
{		int m;
	public:
		void fa();
};
class B
{		int n;
	public:
		void fb();
};
```

如何定义一个类C，它包含A和B的所有成员，另外还拥有新的数据成员r和成员函数fc？

#### 用单继承实现：

```c++
class C: public A
{		int n,r; //把B类中的n复制过来
	public:
		void fb(); //把B类中的fb复制过来
		void fc();
};
```

或：

```c++
class C: public B
{		int m,r; //把A类中的m复制过来
	public:
		void fa(); //把A类中的fa复制过来
		void fc();
};
```

不足之处：

- 概念混乱：导致A和B之间增加了层次关系。 

- 易造成不一致：A中的m、fa（或B中的n、fb）与C中的m、fa（或n、fb）

- 不能完全实现子类型。

#### 用成员对象实现：

```c++
class C
{		A a;
		B b;
		int r;
	public:
		void fa() { a.fa(); }
		void fb() { b.fb(); }
		void fc();
};
```

不足之处：

- 不能实现子类型：程序中的A或B的对象不能用C的对象去替代，......。

#### 用多继承实现：

```c++
class C: public A, public B
{		int r;
	public:
		void fc();
};
```



### 多继承的类定义

多继承是指派生类可以有一个以上的直接基类。多继承的派生类定义格式为：

```c++
class <派生类名>: [<继承方式>] <基类名1>,

    [<继承方式>] <基类名2>, …

{ <成员说明表>

};
```

- 继承方式及访问控制的规定同单继承。

- 派生类拥有所有基类的所有成员。

- 基类的声明次序决定：
  - 对基类构造函数/析构函数的调用次序
  - 对基类数据成员的存储安排。

```c++
class A
{		int m;
	public:
		void fa();
};
class B
{		int n;
	public:
		void fb();
};
class C: public A, public B
{		int r;
	public:
		void fc();
};
......
C c;
c.fa(); //OK
c.fb(); //OK
c.fc(); //OK
```

构造函数的执行次序是：`A()、B()、C()`

> （A()和B()实际是在C()的成员初始化表中调用。）

可以把以public继承方式定义的多继承派生类的对象的地址赋给任何一个基类的指针，这时将会自动进行地址调整。



### 多继承中的名冲突问题

```c++
class A
{		......
	public:
		void f();
		void g();
};

class B
{		......
	public:
		void f();
		void h();
};

class C: public A, public B
{		......
	public:
		void func()
		{	f(); //Error，是A的f，还是B的f？ 
		}
};
......
C c;
c.f();  //Error，是A的f，还是B的f？
```

解决名冲突的办法是：基类名受限

```c++
class C: public A, public B
{		......
	public:
		void func()
		{	A::f(); //OK，调用A的f。
			B::f(); //OK，调用B的f。
		}
};
......
C c;
c.A::f(); //OK，调用A的f。
c.B::f(); //OK，调用B的f。
```



### 重复继承问题－－虚基类

下面的类D从类A继承两次，称为重复继承：

```c++
class A
{  int x;
	......
};
class B: public A { ... };
class C: public A { ... };
class D: public B, public C { ... };
D d;
```

上面D类的对象d将包含两个x：`B::x`和`C::x`

如果要求类D中只有一个x，则应把A定义为B和C的虚基类： 

```c++
class B: virtual public A {...};
class C: virtual public A {...};
class D: public B, public C {...};
D d;
```

这样，D类的对象d就只有一个x了。



### 虚基类构造函数的调用

对于间接包含虚基类的类：

- 虚基类的构造函数由该类的构造函数直接调用。(虚基类的构造函数由最新派生出的类的构造函数调用)

- 虚基类的构造函数优先非虚基类的构造函数执行。

```c++
class A
{  int x;
  public:
   A(int i) { x = i; }
};
class B: virtual public A
{  int y;
public:
   B(int i): A(1) { y = i; }
};
class C: virtual public A
{  int z;
  public:
   C(int i): A(2) { z = i; }
};
class D: public B, public C
{  int m;
public:
   D(int i, int j, int k): B(i), C(j), A(3) { m = k; }
};
class E: public D
{  int n;
  public:
   E(int i, int j, int k, int l): D(i,j,k), A(4) { n = l; }
};
......
D d(1,2,3);  //这里，A的构造函数由D调用，d.x初始化为3。
/*调用的构造函数及它们的执行次序是：
A(3)、B(1)、C(2)、D(1,2,3)
*/

E e(1,2,3,4);  //这里， A的构造函数由E调用，e.x初始化为4。
/*调用的构造函数及它们的执行次序是：
A(4)、B(1)、C(2)、D(1,2,3)、E(1,2,3,4) */
```



## Lecture 18 聚合/组合

### 继承不是类代码复用的唯一方式

在面向对象程序设计中，有些代码复用不宜用继承来实现：

- 继承除了支持代码复用外，还体现了类之间在概念上的一般与特殊关系（is-a-kind-of）。如果两个类之间没有这个关系，纯粹是为了代码复用而用继承把它们关联起来，这样会造成概念上的混乱。

- 例如：“飞机”类复用“发动机”类的功能就不宜用继承来实现。



### 类之间的整体与部分关系

类之间除了继承关系外，还存在一种整体与部分的关系（is-a-part-of），即一个类的对象包含了另一个类的对象。例如，“飞机”类与“发动机”类之间就是整体与部分的关系。

类之间的整体与部分的关系可以是

- 聚合（aggregation）关系

- 组合（composition）关系



### 聚合

在聚合关系中，被包含的对象与包含它的对象独立创建和消亡，被包含的对象可以脱离包含它的对象独立存在。

例如，一个公司与它的员工之间是聚合关系。

聚合类的成员对象一般是采用对象指针表示，用于指向被包含的成员对象，而被包含的成员对象是在外部创建，然后加入进来的。

```c++
class A
{	......
};
class B //B与A是聚合关系
{ A *pm; //指向成员对象
public:
   B(A *p) { pm = p; } //成员对象在聚合类对象外部创建，然后传入
   ~B() { pm = NULL; } //传进来的成员对象不再是聚合类对象的成员
   ......
};
......
A *pa=new A; //创建一个A类对象
B *pb=new B(pa); //创建一个聚合类对象，其成员对象是pa指向的对象
......
delete pb; //聚合类对象消亡了，其成员对象并没有消亡，
	    //但它已经不是聚合类对象的成员了
......
delete pa; //聚合类对象原来的成员对象消亡
```



### 组合

在组合关系中，被包含的对象不能脱离包含它的对象独立存在，被包含的对象由包含它的对象创建并随着包含它的对象的消亡而消亡。

例如，一个人与他的头、手和脚之间则是组合关系。

组合类的成员对象一般直接是对象，有时也可以采用对象指针表示，但不管是什么表示形式，成员对象一定是在组合类对象内部创建并随着组合类对象消亡。

```c++
class A
{ ......
};
class C //C与A是组合关系
{ A a;
public:
  ......
};
......
C *pc=new C; //创建一个组合类对象，其成员对象在组合类对象内部创建
.....
delete pc; //组合类对象与其成员对象都消亡了
```

```c++
class A
{ ......
};
class B //B与A是组合关系
{ A *pm; //指向成员对象
public:
  B() { pm = new A; } //成员对象随组合类对象在内部创建
  ~B() { delete pm; } //成员对象随组合类对象消亡
  ......
};
......
B *pb = new B; //创建一个组合类对象，其成员对象在组合类对象内部创建
.....
delete pb; //组合类对象与其成员对象都消亡了
```



### 例：利用一个线性表类实现一个队列类。

线性表由若干元素构成，元素之间有线性的次序关系。

> 除了两个元素外，每个元素都有且仅有一个直接前驱元素和一个直接后继元素；在除外的两个元素中，一个只有一个直接前驱元素，另一个只有一个直接后继元素。

```c++
class LinearList
{		......
	public:
		bool insert( int x, int pos ); 
		bool remove( int &x,  int pos ); 
		int element( int pos ) const; 
		int search( int x ) const; 
		int length( ) const;
};
```

队列（Queue）是一种特殊的线性表，插入操作在一端，删除操作在另一端。又称先进先出表（First In First Out，FIFO）。

#### Queue的实现1（组合）：

```c++
class Queue
{		LinearList list;
	public:
		bool en_queue(int i) 
		{ return list.insert(i,list.length());
		}
		bool de_queue(int &i) 
		{ return list.remove(i,1);
		}
};
```

#### Queue的实现2（继承）：

```c++
class Queue: private LinearList 
{	public:
		bool en_queue(int x)
		{  return insert(x,length());
		}
		bool de_queue(int &x)
		{  return remove(x,1);
		}
};
```

这里为什么不用public继承？

- 实际上，private继承已经退化成组合了！



### 继承与聚合/组合的比较

继承与封装存在矛盾，聚合/组合则否。

- 在继承方式中，一个类通过protected访问控制，向外界提供两种接口：
  - public：对象（实例）用户
  - public+protected：派生类用户

- 在聚合/组合方式的代码复用中，一个类对外只需一个接口：public。

继承的代码复用功能常常可以用组合来实现。

```c++
class A
{		......
	public:
		void f();
		void g();
};

//继承
class B: public A
{		......
  public:
		 void h();
		......
};

//组合
class B
{		......
		  A a;  
  public:
		 void f() { a.f(); }
		 void g() { a.g(); }
		 void h();
		......
};
```

继承更容易实现子类型：

- 在C++中，public继承的派生类往往可以看成是基类的子类型。

- 在需要基类对象的地方可以用派生类对象去替代。

- 发给基类对象的消息也能发给派生类对象。

具有聚合/组合关系的两个类不具有子类型关系！ 



## Lecture 19 泛型（类属）程序设计

### 面临的问题

在程序设计中，经常需要用到一些功能完全相同的程序实体，但它们所涉及的数据的类型不同。

例如，对不同元素类型的数组进行排序的函数：

```c++
void int_sort(int x[],int num);

void double_sort(double x[],int num);

void A_sort(A x[],int num);
```

这三个函数的实现是一样的（如都采用快速排序算法）。

再例如，元素类型不同的栈类：

```c++
class IntStack
{		int buf[100];
	public:
		void push(int);
		void pop(int&);
};

class DoubleStack
{	double buf[100];
   public:
	void push(double);
	void pop(double&);
};

class AStack
{	A buf[100];
   public:
	void push(A);
	void pop(A&);
};
```

这三个类的实现也是一样的（用数组表示元素）。

对于前面的三个排序函数和三个栈类，如果能分别只用一个函数和一个类来描述它们，这将会大大简化程序设计的工作！

以上需求可以用“泛型”来解决。



### 泛型（类属）

一个程序实体能对多种类型的数据进行操作或描述的特性称为类属或泛型（Generics）。

具有类属特性的程序实体通常有：

- 类属函数：一个能对不同类型的数据完成相同操作的函数。

- 类属类：一个成员类型可变、但功能不变的类。

基于具有类属性的程序实体进行程序设计的技术称为：泛型程序设计（或类属程序设计，Generic Programming）。



### 类属函数

类属函数是指一个函数能对不同类型的参数完成相同的操作。

C++提供了两种实现类属函数的机制：

- 采用通用指针类型的参数（C语言的做法）

- 函数模板



### 用通用指针参数实现类属排序函数

函数定义：

```c++
void sort(void *base, //需排序的数据（数组）内存首地址
	unsigned int count, //数据元素的个数
	unsigned int element_size, //一个数据元素所占内存大小（字节数）
 	int (*cmp)(const void *, const void *) ) //比较两个元素的函数
{   //不论采用何种排序算法，一般都需要对数组进行以下操作：	
     //取第i个元素
        (char *)base+i*element_size
     //比较第i个和第j个元素的大小 （利用调用者提供的回调函数cmp实现）
        (*cmp)((char *)base+i*element_size,
                    (char *)base+j*element_size)
     //交换第i个和第j个元素的位置
        char *p1=(char *)base+i*element_size,
	     *p2=(char *)base+j*element_size;
        for (int k=0; k<element_size; k++)
        {	char temp=p1[k]; p1[k] = p2[k]; p2[k] = temp;
        } 
} //上面的char用byte替代更好：typedef unsigned char byte;
```

类属排序函数的使用

```c++
int int_compare(const void *p1, const void *p2)  
//比较int类型元素大小。
{	if (*(int *)p1 < *(int *)p2) 
		return –1;
	else if (*(int *)p1 > *(int *)p2) 
		return 1;
	else 
		return 0;
}
......
int a[100]; 
......
sort(a,100,sizeof(int),int_compare);

int double_compare(const void *p1, const void *p2) 
//比较double类型元素大小。
{	if (*(double *)p1 < *(double *)p2) 
		return –1;
	else if (*(double *)p1 > *(double *)p2) 
		return 1;
	else 
		return 0;
}
......
double b[200]; 
......
sort(b,200,sizeof(double),double_compare);
int A_compare(const void *p1, const void *p2) 
    
//比较A类元素大小。
{	if (*(A *)p1 < *(A *)p2)  //类A需重载操作符：<
	   return –1;
	else if (*(A *)p1 > *(A *)p2)  //类A需重载操作符：>
	   return 1;
      else 
	   return 0;
}
A c[300]; 
......
sort(c,300,sizeof(A),A_compare);
```

用通用指针实现类属函数面临的问题：

- 比较麻烦，需要大量的指针操作。

- 容易出错！编译程序无法进行类型检查。



### 函数模板

在C++中，实现类属函数的另外一种途径是用函数模板。

函数模板是指带有类型参数的函数定义，其一般格式如下：

```c++
template <class T1, class T2, ...> //class也可以写成typename
<返回值类型> <函数名>(<参数表>)
{	......
}
```

- T1、T2等是函数模板的参数，它们的取值是某个类型。
- 函数的<返回值类型> 、<参数表>中的参数类型以及函数体中的局部变量的类型可以是：T1、T2等。

例如，下面是求最大值的函数模板：

```c++
template <class T> 
T max(T a, T b)
{ return a>b?a:b;
}
```



### 用函数模板实现类属排序函数

排序函数模板的定义：

```c++
template <class T> 
void sort(T elements[], unsigned int count)
{	
    ......
	//取第i个元素
	elements[i] 
        
    ......
	//比较第i个和第j个元素的大小
	elements[i] < elements [j] 
        
        
    ......
	//交换第i个和第j个元素
	T temp=elements [i];
	elements[i] = elements [j];
	elements[j] = temp;
	
    ......
}
```

排序函数模板的使用：

```c++
int a[100];

sort(a,100); //对int类型数组进行排序



double b[200]; 

sort(b,200); //对double类型数组进行排序



A c[300]; 

sort(c,300); //对A类型数组进行排序

//在类A中，需重载操作符：<
//可能还需要自定义拷贝构造函数和重载操作符=
```



### 函数模板的实例化

实际上，函数模板定义了一系列重载的函数。

要使用函数模板所定义的函数，首先必须要对函数模板进行实例化：

- 给模板参数提供一个具体的类型，从而生成具体的函数。

函数模板的实例化通常是隐式的：

- 由编译程序根据函数调用的实参类型自动地把函数模板实例化为具体的函数。

- 这种确定函数模板实例的过程叫做模板实参推导（template argument deduction）。

```c++
int a[100];
sort (a,100);  
//实例化：
void sort(int elements[], unsigned int count) { ...... }

double b[200]; 
sort (b,200);  
//实例化：
void sort(double elements[], unsigned int count) { ...... }

A c[300]; 
sort(c,300); 
//实例化：
void sort(A elements[], unsigned int count) { ...... }
```

有时，编译程序无法根据调用时的实参类型来确定所调用的模板实例函数，这时，需要在程序中显式地实例化函数模板。例如： 

```c++
template <class T> 
T max(T a, T b)
{ return a>b?a:b;
}
......
int x,y,z;
double l,m,n;
z = max(x,y); //调用：int max(int a,int b)
l = max(m,n); //调用：double max(double a,double b)
```

问题：max(x,m) 调用哪一个实例函数？

解决办法：

- 显式类型转换

  ```c++
  max((double)x,m); //调用：double max(double a,double b)
  ```

   或

  ```c++
  max(x,(int)m); //调用：int max(int a,int b)
  ```

- 再定义一个max的重载函数

  ```c++
  double max(int a,double b) { return a>b?a:b; }
  ```

- 显式实例化

  ```c++
  max<double>(x,m); 
  ```

  或 

  ```c++
  max<int>(x,m);
  ```



### 带非类型参数的函数模板

除了类型参数外，函数模板还可以带有非类型参数。例如，

```c++
template <class T, int size> //size为一个int型的普通参数
void f(T a)
{	T temp[size];
	......  
}
```

这样的函数模板在使用时需要显式实例化。例如，

```c++
f<int,10>(1);  //调用模板函数f(int a)，其中的size为10 
```



### 类模板

如果一个类的成员类型可变、但功能不变，则该类称为类属类。

在C++中，类属类用类模板实现。

类模板的格式为：

```c++
template <class T1,class T2,...> //class也可以写成typename
class <类名>
{	<类成员说明>
}
```

- T1、T2等为类模板的类型参数，在类成员的说明中可以用T1、T2等来作为它们的类型。 

例如，用类模板实现类属的栈类：

```c++
template <class T> 
class Stack
{		T buffer[100];
		int top;
	public:
		Stack() { top = -1; }
		void push(const T &x);
		void pop(T &x);
};
template <class T> 
void Stack <T>::push(const T &x) { ...... }
template <class T> 
void Stack <T>::pop(T &x) { ...... }
```



### 类模板的实例化

类模板定义了若干个类，在使用这些类之前，编译程序将会对类模板进行实例化。

类模板的实例化需要在程序中显式地指出。例如：

```c++
Stack<int> st1; //实例化int型栈类并创建一个相应类的对象
int x;
st1.push(10); st1.pop(x);

Stack<double> st2; //实例化double型栈类并创建一个相应类的对象
double y;
st2.push(1.2); st2.pop(y); 

Stack<A> st3; //实例化A类型栈类并创建一个相应类的对象
A a,b;
st3.push(a); st3.pop(b); //A中可能要重载赋值操作符！为什么？
```

注意：不同类模板实例之间不共享类模板中的静态成员。例如：

```c++
template <class T>
class A
{		static int x;
		T y;
		......
};
template <class T> int A<T>::x=0;
......
A<int> a1,a2; //a1和a2共享一个x。
A<double> a3,a4; //a3和a4共享另一个x。 
```



### 模板的复用

用模板实现的类属也属于一种多态，称为参数化多态：

- 一段带有类型参数的代码，给该参数提供不同的类型就能得到多个不同的代码，即，一段代码有多种解释。

使用一个模板之前首先要对其实例化（用一个具体的类型去替代模板的类型参数），而实例化是在编译时刻进行的，它一定要见到相应的源代码，否则无法实例化！因此，模板属于源代码复用。

一个模板可以有很多的实例，是否实例化模板的某个实例，这要由使用情况来决定。

```c++
// file1.h
template <class T> 
class S //类模板S的定义
{    T a;
  public:
      void f();
};
extern void func();

// file1.cpp
#include "file1.h"
template <class T> 
void S<T>::f() //类模板S中f的实现
{ ......
}
void func()
{ S<float> x; //实例化“S<float>”并创建该类的一个对象x。
   x.f(); //实例化“void S<float>::f()”并调用之。 
} 

// file2.cpp
#include "file1.h"
int main()
{ S<float> s1; //实例化“S<float>”并创建该类的一个对象s1
   s1.f(); //没有实例化“void S<float>::f()”，但调用之
   S<int> s2; //实例化“S<int>”并创建该类的一个对象s2 
   s2.f(); //没有实例化“void S<int>::f()”，但调用之
   func();
   return 0;
} 
```

两个模块都能通过编译，但在连接时出错，连接程序指出：

- file2中调用的`void S<int>::f()`不存在！

解决上述问题的通常做法是把模板的定义和实现都放在头文件中。

```c++
// file1.h
template <class T> 
class S //类模板s的定义
{  T a;
  public:
    void f();
};
template <class T> 
void S<T>::f() //类模板s的实现
{ ......
}
extern void func();
```

使用者通过包含这个头文件，把模板的源代码全包含进来，以备实例化所需。

```c++
// file1.cpp
#include "file1.h"
void func()
{ S<float> x; //实例化“S<float>”并创建该类的一个对象x。
   x.f(); //实例化“void S<float>::f()”并调用之。 
}

// file2.cpp
#include "file1.h"
int main()
{ S<float> s1; //实例化“S<float>”并创建该类的一个对象s1。
   s1.f(); //实例化“void S<float>::f()”并调用之。
   S<int> s2; //实例化“S<int>”并创建该类的一个对象s2。 
   s2.f(); //实例化“void S<int>::f()”并调用之。
   func();
   return 0;
} 
```

因此，模板是基于源代码的复用！



### 重复实例的处理

模板的复用会导致多模块的编译结果中存在相同的实例：

- 相同的函数模板实例

- 相同的类模板成员函数实例

相同代码的存在会造成目标代码庞大！

```c++
// file1.cpp
#include "file1.h"
void func()
{ S<float> x; //实例化“S<float>”并创建该类的一个对象x。
   x.f(); //实例化“void S<float>::f()”并调用之。 
}

// file2.cpp
#include "file1.h"
int main()
{ S<float> s1; //实例化“S<float>”并创建该类的一个对象s1。
   s1.f(); //实例化“void S<float>::f()”并调用之。
   S<int> s2; //实例化“S<int>”并创建该类的一个对象s2。 
   s2.f(); //实例化“void S<int>::f()”并调用之。
   func();
   return 0;
} 
```

`void S<float>::f()` 在两个模块（file1.cpp和file2.cpp）的编译结果中都存在！

如何处置这两个相同的实例？

- 由开发环境来解决：编译第二个模块的时候不生成这个实例。（代价大！）

- 由连接程序来解决：舍弃一个相同的实例。



### 类模板的友元函数

```c++
template <class T> class A; //类模板A的声明（f3的定义中要用到）
template <class T> void f3(A<T>& a) { ...... } //函数模板f3的定义
template <class T> //类模板
class A
{ T x,y;
  ......
  friend void f1(A<T>& a); //f1是多个重载的函数，它们与A的实例一对一友元！
  template <class T> friend void f2(A<T>& a); //f2的实例与A的实例多对多友元
  friend void f3<T>(A<T>& a); //f3的实例与A的实例一对一友元
};
void f1(A<int>& a) { ...... } //f1是A<int>的友元，但不是A<double>的友元！
template <class T> void f2(A<T>& a) {......} //函数模板f2的定义
......
A<int> a1; //实例化A<int>
A<double> a2; //实例化A<double>
f1(a1); //OK，调用f1(A<int>&)
f1(a2); //编译OK，链接错误! 调用的f1(A<double>&)不存在！
f2(a1); //实例化f2<int>，它是A<int>和A<double>的友元
f2(a2); //实例化f2<double>，它是A<int>和A<double>的友元
f3(a1); //实例化f3<int>，它是A<int>的友元，但不是A<double>的友元！
f3(a2); //实例化f3<double>，它是A<double>的友元，但不是A<int>的友元！
```



## Lecture 20 基于STL的编程

### 什么是STL？

除了从C标准库保留下来的一些功能外，C++还提供了一个基于模板实现的标准模板库（Standard Template Library，简称STL）。

- STL基于模板实现了一些面向序列数据的表示及常用的操作。

- STL支持了一种抽象的编程模式。

隐藏了一些低级的程序元素，如数组、链表、循环等。



### STL包含什么？

容器类模板

- 容器用于存储序列化的数据，如：向量、队列、栈、集合等。

算法（函数）模板

- 算法用于对容器中的数据元素进行一些常用操作，如：排序、统计等。

迭代器类模板

- 迭代器实现了抽象的指针功能，它们指向容器中的数据元素，用于对容器中的数据元素进行遍历和访问。

- 迭代器是容器和算法之间的桥梁：传给算法的不是容器，而是指向容器中元素的迭代器，算法通过迭代器实现对容器中数据元素的访问。这样使得算法与容器保持独立，从而提高算法的通用性。



### 基于STL编程的例子

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>
using namespace std;
void display(int x) { cout << ' ' << x; return; }
int main()
{	vector<int> v; //创建容器对象v，元素类型为int
	int x;
	cin >> x;
	while (x > 0) //生成容器v中的元素
	{	v.push_back(x); //往容器v中增加一个元素
		cin >> x;
	}
 	//利用算法max_element计算并输出容器v中的最大元素
	cout << "Max = " << *max_element(v.begin(),v.end()) 
		 << endl;
   //利用算法accumulate计算并输出容器v中所有元素的和
	cout << "Sum = " << accumulate(v.begin(),v.end(),0) 
		 << endl;
	//利用算法sort对容器v中的元素进行排序
	sort(v.begin(),v.end()); 
	//利用算法for_each输出排序结果
	cout << "Sorted result is:\n";
	for_each(v.begin(),v.end(),display);
	cout << '\n';
	return 0;
}
```



### 容器

容器用于表示由同类型元素构成的、长度可变的元素序列。

容器是由类模板来实现的，模板的参数是容器中元素的类型。

STL中包含了很多种容器，虽然这些容器提供了一些相同的操作，但由于它们采用了不同的内部实现方法，因此，不同的容器往往适合于不同的应用场合。



### STL的主要容器

`vector<元素类型>`

- 用于需要快速定位（访问）任意位置上的元素以及主要在元素序列的尾部增加/删除元素的场合。

- 在头文件vector中定义，用动态数组实现。

`list<元素类型>`

- 用于经常在元素序列中任意位置上插入/删除元素的场合。

- 在头文件list中定义，用双向链表实现。

`deque<元素类型>`

- 用于主要在元素序列的两端增加/删除元素以及需要快速定位（访问）任意位置上的元素的场合。

- 在头文件deque中定义，用分段的连续空间结构实现。

`stack<元素类型>`

- 用于仅在元素序列的尾部增加/删除元素的场合。

- 在头文件stack中定义，可基于deque、list或vector来实现。

`queue<元素类型>`

- 用于仅在元素序列的尾部增加、头部删除元素的场合。

- 在头文件queue中定义，可基于deque和list来实现。

`priority_queue<元素类型>`

- 它与queue的操作类似，不同之处在于：每次增加/删除元素之后，它将对元素位置进行调整，使得头部元素总是最大的。也就是说，每次删除操作总是把最大（优先级最高）的元素去掉。

- 在头文件queue中定义，可基于deque和vector来实现。

`map<关键字类型，值类型>`和`multimap<关键字类型，值类型>`

- 用于需要根据关键字来访问元素的场合。容器中每个元素是一个pair结构类型，该结构有两个成员：first和second，关键字对应first，值对应second，元素是根据其关键字排序的。

- 对于map，不同元素的关键字不能相同；

- 对于multimap，不同元素的关键字可以相同。

- 它们在头文件map中定义，常常用某种二叉树来实现。

`set<元素类型>`和`multiset<元素类型>`

- 它们分别是map和multimap的特例，每个元素只有关键字而没有值，或者说，关键字与值合一了。

- 在头文件set中定义。

`basic_string<字符类型>`

- 与vector类似，不同之处在于其元素为字符类型，并提供了一系列与字符串相关的操作。

- string和wstring分别是它的两个实例：`basic_string<char>`, `basic_string<wchar_t>`

- 在头文件string中定义。



### 容器的基本操作

容器类模板提供了一些成员函数来实现容器的基本操作，其中包括：

- 往容器中增加元素

- 从容器中删除元素

- 获取容器中指定位置的元素

- 在容器中查找元素

- 获取容器首/尾元素的迭代器

  ......

这些成员函数往往具有通用性（大部分容器类模板都包含它们）。

注意：如果容器的元素类型是一个类，则针对该类可能需要：

- 自定义拷贝构造函数和赋值操作符重载函数
  - 对容器进行的一些操作中可能会创建新的元素（对象的拷贝构造）或进行元素间的赋值（对象赋值）。

- 重载小于操作符（<）
  - 对容器进行的一些操作中可能要进行元素比较运算。



#### 例：利用容器vector来实现序列元素的存储与统计

```c++
#include <iostream>
#include <vector>
using namespace std;
int main()
{ vector<int> v; //创建一个vector类容器对象v
   int x;
   cin >> x;
   while (x > 0) //循环往容器v中增加正的int型元素
   {  v.push_back(x); //往容器v的尾部增加一个元素
      cin >> x;
   }
 
 	//Version 01
 
   int sum=0,max,min;
   max = min = v[0];
   for (int i=0; i<v.size(); i++) //遍历容器元素
   { sum += v[i];
      if (v[i] < min) min = v[i];
      else if (v[i] > max) max = v[i];
   }
   cout << "sum=" << sum << ",max=" 
          << max << ",min="  << min << endl;
   return 0;
}
```

```c++
   //Version 02
   int sum=0,max,min;
   max = min = v[0];
   for (int n: v) //遍历容器元素（C++11）
   { sum += n;
      if (n < min) min = n;
      else if (n > max) max = n;
   }
   cout << "sum=" << sum << ",max=" 
          << max << ",min="  << min << endl;
   return 0;
}
```



### 例：利用容器map来实现一个电话号码簿的功能

```c++
#include <iostream>
#include <map>
#include <string>
using namespace std;
int main()
{ map<string,int> phone_book; //创建一个map类容器，
				          //用于存储电话号码簿

  //创建电话簿
  phone_book["wang"] = 12345678; //通过[]操作和关键字
                                                      //往容器中加入元素
  phone_book["li"] = 87654321;
  phone_book["zhang"] = 56781234;
  ......
      //输出电话号码簿
  cout << "电话号码簿的信息如下：\n";
  for (pair<string, int> item: phone_book)
    cout << item.first << ": " << item.second << endl; 
                                         //输出元素的姓名和电话号码
  //查找某个人的电话号码
  string name;
  cout << "请输入要查询号码的姓名：";
  cin >> name;
  map<string,int>::const_iterator it; //创建一个不能修改所指向
				                //的元素的迭代器
  it = phone_book.find(name); //查找关键字为name的容器元素
  if (it == phone_book.end()) //判断是否找到
     cout << name << ": not found\n"; //未找到
  else
     cout << it->first << ": " << it->second << endl; //找到
  
  return 0;
}
```



### 迭代器

迭代器（iterator）实现了抽象的指针（智能指针）功能，它们用于指向容器中的元素，对容器中的元素进行访问和遍历。

在STL中，迭代器是作为类模板来实现的（在头文件iterator中定义），它们可分为以下几种类型：

- 输出迭代器（output iterator，记为：OutIt）
  - 可以修改它所指向的容器元素
  - 间接访问操作（*）
  - n++操作

- 输入迭代器（input iterator，记为：InIt）
  - 只能读取它所指向的容器元素
  - 间接访问操作（*）和元素成员间接访问（->）
  - n++、==、!=操作。

- 前向迭代器（forward iterator，记为：FwdIt）
  - 可以读取/修改它所指向的容器元素
  - 元素间接访问操作（*）和元素成员间接访问操作（->）
  - n++、==、!=操作

- 双向迭代器（bidirectional iterator，记为：BidIt）
  - 可以读取/修改它所指向的容器元素
  - 元素间接访问操作（*）和元素成员间接访问操作（->）
  - n++、--、==、!=操作

- 随机访问迭代器（random-access iterator，记为：RanIt）
  - 可以读取/修改它所指向的容器元素
  - 元素间接访问操作（*）、元素成员间接访问操作（->）和下标访问元素操作（[]）
  - n++、--、+、-、+=、-=、==、!=、<、>、<=、>=操作



### 各容器的迭代器类型

- 对于vector、deque以及basic_string容器类，与它们关联的迭代器类型为随机访问迭代器（RanIt）

- 对于list、map/multimap以及set/multiset容器类，与它们关联的迭代器类型为双向迭代器（BidIt）。

- queue、stack和priority_queue容器类，不支持迭代器！



### 迭代器之间的相容关系

在需要箭头左边迭代器的地方可以用箭头右边的迭代器去替代。

![image-20210513095112723](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210513095112723.png)

除了上面五种基本迭代器外，STL还提供了一些迭代器的适配器，用于一些特殊的操作，如：

- 反向迭代器（reverse iterator）：用于对容器元素从尾到头进行反向遍历，可以通过容器类的成员函数rbegin和rend可以获得容器的尾和首元素的反向迭代器。需要注意的是，对反向迭代器，++操作是往容器首部移动，--操作是往容器尾部移动。

- 插入迭代器（insert iterator）：用于在容器中指定位置插入元素，其中包括：
  - back_insert_iterator（用于在尾部插入元素）
  - front_insert_iterator（用于在首部插入元素）
  - insert_iterator（用于在任意指定位置插入元素）

- 它们可以分别通过函数back_inserter、front_inserter和inserter来获得，函数的参数为容器。

......



### 例：利用STL的容器list和迭代器实现求解约瑟夫问题

```c++
#include <iostream>
#include <list>
using namespace std;
int main()
{ int m,n; //m用于存储要报的数，n用于存储小孩个数
   cout << "请输入小孩的个数和要报的数：";
   cin >> n >> m;
   //构建圈子
   list<int> children; //children是用于存储小孩编号的容器
   for (int i=0; i<n; i++) //循环创建容器元素
     children.push_back(i); //把i（小孩的编号）从容器
				     //尾部放入容器
//开始报数
  list<int>::iterator current; //current为指向容器元素的迭代器
  current = children.begin(); //current初始化成指向容器的第一个元素
  while (children.size() > 1) //只要容器元素个数大于1，就执行循环
   {  for (int count=1; count<m; count++)  //报数，循环m-1次
      { current++; //current指向下一个元素
         //如果current指向的是容器末尾，current指向第一个元素
         if (current == children.end()) current = children.begin();
       }
       //循环结束时，current指向将要离开圈子的小孩
       current = children.erase(current);  //小孩离开圈子，current指向
					       //下一个元素
       //如果current指向的是容器末尾，current指向第一个元素
       if (current == children.end()) current = children.begin();
   } //循环结束时，current指向容器中剩下的唯一的一个元素，即胜利者
   //输出胜利者的编号
   cout << "The winner is No." << *current << "\n";
   return 0;
}

```

> 上面程序中的list也可以换成vector，但由于程序中需要经常在容器的任意位置上删除元素，而list容器的双向链表结构比较适合这个操作！



### 算法

在STL中，除了用容器类模板自身提供的成员函数来操作容器元素外，还提供了一系列通用的对容器中元素进行操作的函数模板，称为算法（algorithm）。

STL算法实现了对序列元素的一些常规操作，用函数模板实现的，主要包括：

- 调序算法

- 编辑算法

- 查找算法

- 算术算法

- 集合算法

- 堆算法

- 元素遍历算法

除了算术算法在头文件numeric中定义外，其它算法都在头文件algorithm中定义。

大部分STL算法都是遍历指定容器中某范围内的元素，对满足条件的元素执行某项操作。

算法的内部实现往往隐含着循环操作，但这对使用者是隐藏的。

- 使用者只需要提供：容器（迭代器）、操作条件以及可能的自定义操作，而算法的控制逻辑则由算法内部实现，这体现了一种抽象的编程模式。



### 算法与容器之间的关系

在STL中，不是把容器传给算法，而是把容器的某些迭代器传给它们，在算法中通过迭代器来访问和遍历相应容器中的元素。

这样做的好处是：使得算法不依赖于具体的容器，提高了算法的通用性。

- 虽然容器各不相同，但它们的迭代器往往具有相容关系，一个算法往往可以接受相容的多种迭代器。



### 算法接受的迭代器类型

一个算法能接收的迭代器的类型是通过算法模板参数的名字来体现的。例如：

```c++
template <class InIt, class OutIt>
OutIt copy(InIt src_first, InIt src_last, 
                OutIt dst_first)
{ ...... }
```

- src_first和src_last是输入迭代器，算法中只能读取它们指向的元素。

- dst_first是输出迭代器，算法中可以修改它指向的元素。

- 以上参数可以接受其它相容的迭代器。



### 算法的操作范围

用算法对容器中的元素进行操作时，大都需要用两个迭代器来指出要操作的元素的范围。例如：

```c++
void sort(RanIt first, RanIt last);
```

- first是第一个元素的位置

- last是最后一个元素的下一个位置

有些算法可以有多个操作范围，这时，除第一个范围外，其它范围可以不指定最后一个元素位置，它由第一个范围中元素的个数决定。例如：

```c++
OutIt copy(InIt src_first, InIt src_last, OutIt dst_first)
```

一个操作范围的两个迭代器必须属于同一个容器，而不同操作范围的迭代器可以属于不同的容器。



### 算法的自定义操作条件

有些算法可以让使用者提供一个函数或函数对象来作为自定义操作条件（或称为谓词），其参数类型为相应容器的元素类型，返回值类型为bool。

自定义操作条件可分为：

- `Pred`：一元“谓词”，需要一个元素作为参数

- `BinPred`：二元“谓词”，需要两个元素作为参数

例如，对于下面的“统计”算法：

```c++
size_t count_if(InIt first, InIt last, Pred cond);
```

```c++
bool f(int x) { return x > 0; }
vector<int> v;

...... //往容器中放了元素

cout<<count_if(v.begin(),v.end(),f); //统计v中正数的个数
```

再例如，对于下面的“排序”算法：

```c++
void sort(RanIt first, RanIt last); //按“<”排序

void sort(RanIt first, RanIt last, BinPred comp); //按comp返回true规定的次序
```

```c++
bool greater2(int x1, int x2) { return x1>x2; }
vector<int> v;

...... //往容器中放了元素

sort(v.begin(),v.end()); //从小到大排序
sort(v.begin(),v.end(),greater2); //从大到小排序
```



### 算法的自定义操作

有些算法可以让使用者提供一个函数或函数对象作为自定义操作，其参数和返回值类型由相应的算法决定。

自定义操作可分为：

- `Op`或`Fun`：一元操作，需要一个参数

- `BinOp`或`BinFun`：二元操作，需要两个参数

例如，对于下面的“元素遍历”算法：

```c++
Fun for_each(InIt first, InIt last, Fun f); 
```

```c++
void f(int x) { cout << ' ' << x; }
vector<int> v;

...... //往容器中放了元素

for_each(v.begin(),v.end(),f); //对v中的每个元素去调用函数f进行操作
```

再例如，对于下面的“累积”算法：

```c++
T accumulate(InIt first, InIt last, T val); //按“+”操作
T accumulate(InIt first, InIt last, T val, BinOp op); 
```

由op决定累积的含义。设元素为e1、e2、...、en，算法返回tn：t1=op(val,e1); t2=op(t1,e2); ... tn=op(tn-1,en); 

```c++
int f1(int partial, int x) { return partial*x; }
int f2(int partial, int x) { return partial+x*x; }
double f3(double partial, int x) { return partial+1.0/x; }
vector<int> v;
...... //往容器中放了元素
accumulate(v.begin(),v.end(),0); //所有元素和
accumulate(v.begin(),v.end(),1,f1); //所有元素的乘积
accumulate(v.begin(),v.end(),0,f2); //所有元素平方和
accumulate(v.begin(),v.end(),0.0,f3); //所有元素倒数和
```

再例如，对于下面的元素“变换/映射”算法：

```c++
OutIt transform(InIt src_first, InIt src_last, OutIt dst_first, Op f);
OutIt transform(InIt1 src_first1, InIt1 src_last1, InIt2 src_first2, OutIt dst_first, BinOp f);
```

```c++
int f1(int x) { return x*x; }
int f2(int x1, int x2) { return x1+x2; }
vector<int> v1,v2,v3,v4;
...... //往v1和v2容器中放了元素
transform(v1.begin(),v1.end(),v3.begin(),f1); //v3中的元素是v1相应元素的平方
transform(v1.begin(),v1.end(),v2.begin(),v4.begin(),f2); //v4中的元素是v1和v2相应元素的和
```



### STL算法的应用--在“学生”容器中做统计

学生的类型：

```c++
class Student
{  int no;
    string name;
    Sex sex;
    string birth_place;
    Major major; 
    ......
  public:
    int get_no() { return no; }
    string get_name() { return name; }
    int get_sex() { return sex; }
    Major get_major() { return major; }
    display();
    ......
};
```

建立“学生”容器：

```c++
vector<Student> students; //创建学生容器
while (...) //循环输入每个学生信息并放入容器
{ Student st;
   ...... //输入一个学生信息到st
   students.push_back(st); //把st数据放入容器
}
```

容器元素按“学号由小到大”排序：

```c++
bool compare_no(Student &st1, Student &st2)
    { return st1.get_no() < st2.get_no(); }
sort(students.begin(),students.end(), compare_no);
```

显示每个学生的信息：

```c++
void display(Student &st) //显示st的信息
{	st.display();	cout << endl; 
}
for_each(students.begin(),students.end(),display);
```

统计“计算机”专业的人数：

```c++
bool match_major(Student &st)
{ return st.get_major() == COMPUTER; 
}
cout <<  count_if(students.begin(),students.end(), match_major);
```

统计“物理”专业的人数呢？

```c++
bool match_major2(Student &st)
{ return st.get_major() == PHYSICS; 
}
cout <<  count_if(students.begin(),students.end(), match_major2);
```

再统计“XXX”专业的人数呢？

???

可利用函数对象来解决上面的问题

```c++
class MatchMajor
{	   Major major;
	public:
	   MatchMajor (Major m)
	   {	major = m;
	   }
	   bool operator ()(Student& st)
	   { return st.get_major() == major;
	   }
};

count_if(students.begin(),students.end(), MatchMajor(COMPUTER))；
count_if(students.begin(),students.end(), MatchMajor(PHYSICS))；
count_if(students.begin(),students.end(), MatchMajor(XXX))；
```

统计XXX专业男生/女生的人数:

```c++
class MatchMajorAndSex
{  Major major;
    Sex sex;
public:
    MatchMajorAndSex(Major m,Sex s)
    {	major = m;
	sex = s;
    }
    bool operator ()(Student& st)
    { return st.get_major() == major && st.get_sex() == sex;
    }
};

count_if(students.begin(),students.end(), MatchMajorAndSex(COMPUTER,FEMALE)) //计算机女生
count_if(students.begin(),students.end(), MatchMajorAndSex(PHYSICS,MALE)) //物理男生
```

还有其它统计需求怎么办？例如，

- 统计南京籍计算机专业的人数

- 统计南京籍计算机专业女生的人数

- .......

再定义其它的函数或函数对象？

- 太多了！

可以用λ表达式来解决！

- 匿名函数

- 编译器隐式地为之定义类（重载了函数调用操作符）和创建函数对象。

```c++
//统计“计算机专业女生”的人数	
cout << "计算机专业女生的人数是：" 
      << count_if(students.begin(),students.end(),
        [](Student &st) { return (st.get_major() == COMPUTER)
			     && (st.get_sex() == FEMALE); });

//统计出生地为"南京籍计算机专业"的学生人数
cout << "出生地为\"南京\"的学生人数是：" 
      << count_if(students.begin(),students.end(),
  [](Student &st) { return (st.get_major() == COMPUTER) 
          && (st.get_birth_place().find("南京")!= string::npos);});

//按“学号由小到大”对students的元素进行排序
sort(students.begin(),students.end(),
		[](Student &st1,Student &st2) {  
                                 return  st1.get_no()<st2.get_no();});
//按“姓名由小到大”对students的元素进行排序
sort(students.begin(),students.end(),
		[](Student &st1,Student &st2) {  
                           return st1.get_name()<st2.get_name();});

```



## Lecture 21 函数式程序设计

### 命令式程序设计范式（imperative programming）

过程式与面向对象程序设计属于命令式程序设计范式（imperative programming）

- 需要对“如何做”进行详细描述，包括操作步骤和状态变化。

- 它们与冯诺依曼体系结构一致，是使用较广泛的程序设计范式，适合于解决大部分的实际应用问题。



### 声明式程序设计范式（declarative programming）

除了命令式程序设计范式外，还有一类是声明式程序设计范式（declarative programming）

- 只需要对“做什么”进行描述，而如何做则由实现系统自动完成。

- 有良好的数学理论支持，易于保证程序的正确性，并且，设计出的程序比较精炼和具有潜在的并行性。

声明式程序设计范式的典型代表：

- 函数式程序设计
- 逻辑式程序设计



### 什么是函数式程序设计

函数式程序设计（functional programming）是指把程序组织成一组数学函数，计算过程体现为基于一系列函数应用（把函数作用于数据）的表达式求值。

函数也被作为值（数据）来看待，即函数的参数和返回值也可以是函数。

基于的理论是递归函数理论和lambda演算。



### 函数式程序设计的基本特征

- “纯”函数（引用透明）：以相同的参数调用一个函数总得到相同的值。（无副作用）

- 没有状态（数据不可变）：计算不改变已有数据，而是产生新的数据。（无赋值操作）

- 函数也是值（first-class citizen) ：函数的参数和返回值都可以是函数。（高阶函数）

- 递归是主要的控制结构：重复操作不采用迭代（循环）形式。

- 表达式的惰性（延迟）求值（Lazy evaluation）：需要的时候才计算。

- 潜在的并行性。



### 函数式程序设计语言

纯函数式语言：

- Common Lisp, Scheme, Clojure, Racket, Erlang, OCaml, Haskell, F#

对函数式编程提供支持的非函数式语言：

- Perl, PHP, C# 3.0, Java 8, Python, C++(11)



### 函数式程序设计的例子

计算：

```c++
a * b + c / d
```

- 命令式编程：

```c++
t1 = a * b;
t2 = c / d;
r = t1 + t2;
```

- 函数式编程：

```c++
multiply(...) { ... }
divide(...) { ... }
add(...) { ... }
... add(multiply(a,b),divide(c,d)) ...
```



计算一系列数的和：

```c++
a1 + a2 + a3 + ... + aN
```

- 命令式编程：

```c++
int a[N] = {a1, a2, a3,..., aN},sum=0;
for (int i=0; i<N; i++) sum += a[i];
cout << sum;
```

- 函数式编程：

```c++
int sum(int x[], int n)
{ if (n==1) return x[0];
     else return x[0]+sum(x+1,n-1);
}
int a[N] = {a1, a2, a3,..., aN};
cout << sum(a,N);
```



计算字符串的逆序：

```c++
"abcd" --> "dcba"
```

- 命令式编程：

```c++
void reverse(char str[])
{ int len=strlen(str);
   for (int i=0; i<len/2; i++)
   {  char t=str[i];
      str[i] = str[len-i-1]; str[len-i-1] = t;
   }
}
```

- 函数式编程：

```c++
string reverse(string str) 
{ if (len(str) == 1) 
 　	return str;
  else 
	 return concat(reverse(substr(str,1,len(str)-1)),substr(str,0,1));
}
```

> 注意：不是写成函数就是函数式编程！



计算一系列数中偶数的个数。

- 命令式编程： 

```c++
int a[N] = {a1, a2, a3,..., aN},count=0;
for (int i=0; i<N; i++) 
   if (a[i]%2 == 0) count++;
cout << count;
```

- 函数式编程

```c++
bool f(int x) { return x%2 == 0; } 
vector<int> v={a1, a2, a3,..., aN};
cout << count_if(v.begin(),v.end(),f); 
```



### 函数式程序设计的基本手段

递归：实现重复操作，不采用迭代方式（循环）。

尾递归：递归调用是递归函数的最后一步操作。（优化）

filter/map/reduce（过滤/映射/规约）

- 过滤：把一个集合中满足某条件的元素选出来，构成一个新的集合。

- 映射：分别对一个集合中每个元素进行某种操作，结果放到一个新集合中。

- 规约：对一个集合中所有元素进行某个操作后得到一个值。

bind（绑定）：给一个函数的某些参数指定固定的值，返回一个只含有未指定值的参数所构成的函数。（偏函数）

currying（柯里化）：把接受多个参数的函数变换成接受单一参数（原函数的第一个参数）的函数，该函数返回一个接收剩余参数的函数。



### 递归

例如，求第n个fibonacci数

- 命令式方案（迭代）

```c++
int fib_1=1,fib_2=1,n=10;
for (int i=3; i<=n; i++)
{	int temp=fib_1+fib_2;
	fib_1 = fib_2; fib_2 = temp;
}
cout << fib_2 << endl;
```

- 函数式方案1（递归）

```c++
int fib(int n)
{	if (n == 1 || n == 2)  return 1;
	else  return fib(n-2)+fib(n-1);
}
cout << fib(10) << endl;
```



### 尾递归

函数式方案2（尾递归）

```c++
int fib(int n, int a, int b)
{	if (n == 1)  return a;
	else  return fib(n-1,b,a+b); 
}
cout << fib(10,1,1) << endl;
```

- 便于编译程序优化：
  - 由于递归调用是本次调用的最后一步操作，因此，递归调用时可重用本次调用的栈空间。

  - 可以自动转成迭代。



### 尾递归自动转迭代

```c++
int fib(int n, int a, int b)
{	 while (true)
   { if (n == 1)  return a;
	    else  //return fib(n-1,b,a+b);
      { int t1=n-1,t2=b,t3=a+b;
         n = t1; a = t2; b = t3;
      }
   }
}
```

一般形式：

```c++
T f(T1 x1, T2 x2, ...)
{  ......
    ... return f(m1,m2,...);
    ......
    ... return f(n1,n2,...);
    ......
}
```

转换成：

```c++
T f(T1 x1, T2 x2, ...)
{  while (true)
    { ......
       ... { T1 t1=m1; T2 t2=m2; ... 
              x1 = t1; x2 = t2; ... continue;} 
       ......
       ... { T1 t1=n1; T2 t2=n2; ... 
              x1 = t1; x2 = t2; ... continue;}
       ......
    }
}
```



### Filter操作

例如，求某整数集合中的所有正整数：

```c++
vector <int> nums={1,-2,7,0,3}, positives;
copy_if(nums.begin(),nums.end(),
	back_inserter(positives),[](int x){return x>0;});
for_each(positives.begin(),positives.end(),
	[](int x){ cout << x <<','; });
```



### Map操作

例如，计算某整数集合中各整数的平方;

```c++
vector <int> nums={1,-2,7,0,3}, squares;
transform(nums.begin(),nums.end(), 
	back_inserter(squares), [](int x){return x*x;});
for_each(squares.begin(), squares.end(),
              [](int x){cout<<x<<',';});
```



### Reduce操作

```c++
vector <int> nums={1,-2,7,0,3};
int sum = accumulate(nums.begin(), nums.end(),
			0, [](int a,int x) {return a+x;});
cout << sum;
```



### 运用过滤/映射/规约操作来实现：输出学生中所有女生的名字和平均年龄：

```c++
vector<Student> students,students_2;
vector<string> names;
copy_if(students.begin(),students.end(),
	     back_inserter(students_2),
   	[](Student &st) { return st.get_sex()==FEMALE;});

transform(students_2.begin(),students_2.end(),
		back_inserter(names),
	[](Student &st) { return st.get_name(); });

sort(names.begin(),names.end());
for_each(names.begin(),names.end(),
	[](string& s) { cout << s << endl; });

cout << accumulate(students_2.begin(),students_2.end(),0,
[](int a,Student &st) {return a+st.get_age();})/students_2.size();
```



### bind操作

对于一个多参数的函数，在某些应用场景下，它的一些参数往往取固定的值。

可以针对这样的函数，生成一个新函数，该新函数不包含原函数中已指定固定值的参数。（partial function application，偏函数）

偏函数可缩小一个函数的适用范围，提高函数的针对性。

例如，对于下面的print函数：

```c++
void print(int n,int base); //按base指定的进制输出n
```

由于它常常用来按十进制输出，因此，可以基于`print`生成一个新函数`print10`，它只接受一个参数`n`，`base`固定为`10`：

```c++
#include <functional>
using namespace std;
using namespace std::placeholders;
function<void(int)> print10=bind(print,_1,10);
print10(23); //print(23,10);
```

- `Fty2 bind(Fty1 fn, T1 t1, T2 t2, ..., TN tN);`
  - `fn`为N个参数的函数（或函数对象）；
  - `t1～tN`是为函数fn指定的参数值，其中的ti可以是绑定的值，也可以是未绑定的值。
  - 对未绑定的值用_1、_2、...等来表示，其中的数字表示未绑定值的参数在新函数参数表中的位置；

  - bind返回由所有未绑定值的参数所构成的新函数。



例如：

```c++
#include <functional>
using namespace std; 
using namespace std::placeholders; 
                              //_1、_2、...的定义所在的名空间
void product(double x, double y, double z)
{  cout << x << "*" << y << "*" << z << endl;
}
......
function<void(int,int)> f2=
bind(product,_1,4,_2);//bind返回一个带两个参数的函数
f2(3,5); //把bind返回的函数作用于(3,5)，输出：3*4*5
```

```c++
#include <functional>
using namespace std; 
using namespace std::placeholders; 
                              //_1、_2、...的定义所在的名空间
void product(double x, double y, double z)
{  cout << x << "*" << y << "*" << z << endl;
}
......
function<void(int,int)> f2=
bind(product,_2,4,_1);//bind返回一个带两个参数的函数
f2(3,5); //把bind返回的函数作用于(3,5)，输出：5*4*3
```

例如：

```c++
#include <functional>
using namespace std; 
using namespace std::placeholders; 
bool greater2(double x, double y) { return x > y; }
auto is_greater_than_42=bind(greater2, _1, 42);
auto is_less_than_42=bind(greater2, 42, _1);
cout << is_greater_than_42(45); //true
cout << is_less_than_42(45); //false
```



### Currying操作

把函数f(x,y)变成单参数函数h(x)，h返回一个函数g(y)：

```c++
#include <functional>
using namespace std;
int f(int x,int y) 
{ return x+y; 
}
function<int (int)>  h(int x) //返回值是个函数对象
{ return [x](int y)->int { return x+y; };
   //等价于：return bind(f,x,_1);
}
......
cout << f(1,2);
cout << h(1)(2);
```

例如，利用上面的函数h可以为函数f生成一个特定的函数：

```c++
function<int (int)> add5=h(5); //add5(y)=5+y
......
int a,b,c;
......
... add5(a) ... //返回a+5
... add5(b) ... //返回b+5
... add5(c) ... //返回c+5
```



## Lecture 22 输入/输出（面向对象途径）

### 输入/输出（I/O）概述

输入/输出（简称I/O）是程序的一个重要组成部分：

- 程序运行所需要的数据往往要从外设（如：键盘、文件等）得到

- 程序的运行结果通常也要输出到外设（如：显示器、打印机、文件等）中去。

在C++中，输入/输出操作不是语言定义的成分，而是由具体的实现作为标准库的功能来提供。

在C++中，输入/输出是一种基于字节流的操作：

- 在进行输入操作时，可把输入的数据看成逐个字节地从外设流入到计算机内存。

- 在进行输出操作时，则把输出的数据看成逐个字节地从内存流出到外设。

在C++的标准库中，提供了：

- 基于字节的I/O操作

- 基于C++基本数据类型数据的I/O操作，在这些操作的内部实现了基本数据类型与字节流之间的转换。

另外，还可以对操作符“<<”和“>>”进行重载，以实现对自定义类型的数据（对象）进行输入/输出操作。



### C++输入输出的实现途径

过程式——通过从C语言保留下来的函数库中的输入/输出函数来实现。

- `printf`

- `scanf`

- ......

面向对象——通过C++的I/O类库中的类/对象来实现。

- `ostream`、`cout`，`<<`

- `istream`、`cin`，`>>`

- ......



### `printf`、`scanf`的缺陷

- `printf`和`scanf`是两个带可变参数的函数。
  - 不是强类型：编译时刻不进行参数类型检查，会导致与类型相关的运行错误！（类型不安全）

例如，

```c++
int i;
double j;
scanf("i=%d,j=%lf",&i,&j); //i=12,j=34.5   
printf("i=%d, j=%f\n",i,j);  //i=12, j=34.5
```

当格式串描述与数据不一致时会导致运行时刻的错误：

```c++
scanf("i=%d,j=%d",&i,&j); //类型不一致
printf("i=%d, j=%d\n",i,j); //类型不一致
scanf("i=%d",&i,&j); //个数不一致
printf("i=%d, j=%f\n",i);  //个数不一致
```



### `cout`、`cin`的优势

不需要单独指定数据的类型和个数，编译时刻根据数据本身来决定操作的类型和个数，这样可避免与类型和个数相关的错误！

```c++
int i;
double j;
cout << "i=";  
cin >> i; 
cout << "j="; 
cin >> j;
cout << "i=" << i << ",j=" << j << endl;
```



### I/O的分类

面向控制台的I/O

- 从标准输入设备（如：键盘）获得数据

- 把程序结果从标准输出设备（如：显示器）输出

面向文件的I/O

- 从外存文件获得数据

- 把程序结果保存到外存文件中

面向字符串变量的I/O

- 从程序中的字符串变量中获得数据

- 把程序结果保存到字符串变量中

- 现在已被STL中的string替代



### C++的I/O类库中基本的类

![image-20210527092502460](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210527092502460.png)

在进行输入/输出时，首先创建某个I/O类的对象，然后，可以调用该对象类的成员函数进行基于字节流的输入/输出操作。

`istream`类和`ostream`以及它们的派生类分别重载了操作符“>>”（抽取）和“<<”（插入），用它们可以进行基本类型数据的输入/输出操作。例如：

- 输入

```c++
istream in(...);
	in >> x; //x是一个变量
	in >> y; //y是一个变量
//或
	in >> x >> y;
```

- 输出

```c++
ostream out(...);
	out << e1; //e1是一个表达式
	out << e2; //e2是一个表达式
//或
	out << e1 << e2;
```



### 面向控制台的I/O 

在I/O类库中预定义了四个I/O对象，可以直接利用这些对象进行控制台的输入/输出操作： 

- `cin`（`istream`类的对象）：对应着计算机系统的标准输入设备。（通常为键盘）

- `cout`（`ostream`类的对象）：对应着计算机系统的标准输出设备。（通常为显示器）

- `cerr`和`clog`（`ostream`类的对象）：对应着计算机系统用于输出特殊信息（如程序错误信息）的设备。（通常也对应着显示器，但不受输出重定向的影响）。`cerr`为不带缓冲的，`clog`为带缓冲的。

在进行控制台输入/输出时，程序中需要有下面的包含命令：

```c++
#include <iostream>
```



### 控制台输出操作

基于插入操作符（<<）的基本数据类型数据的输出

```c++
#include <iostream>
using namespace std;
......
int x;
float f;
char ch;
int *p=&x;
......
cout << x ; //输出x的值。
cout << f; //输出f的值。
cout << ch; //输出ch的值。
cout << "hello"; //输出字符串"hello"。
cout << p; //输出变量p的值，即，变量x的地址。
或
cout << x << f << ch << "hello" << p;
```



### 输出格式控制

为了对输出格式进行控制，可以通过输出一些操纵符（manipulator）来实现，例如：

```c++
#include <iostream>
#include <iomanip>  //操纵符声明的头文件。
using namespace std;
.....
int x=10;
cout << hex << x << endl; //以十六进制输出x的值，然后换行。
```

#### 常用输出操纵符

| 操纵符                                                 | 含义                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| `endl`                                                 | 输出换行符，并执行`flush`操作                                |
| `flush`                                                | 使输出缓存中的内容立即输出                                   |
| `dec`                                                  | 十进制输出                                                   |
| `oct`                                                  | 八进制输出                                                   |
| `hex`                                                  | 十六进制输出                                                 |
| `setprecision(int n)`                                  | 设置浮点数的精度（由输出格式决定是有效数字的个数还是小数点后数字的位数） |
| `setiosflags(long flags) / resetiosflags(long flags) ` | 设置/重置输出格式，flags的取值可以是：`ios::scientific`（以指数形式显示浮点数），`ios::fixed`（以小数形式显示浮点数），等等。 |

除了通过插入操作符进行基本数据类型数据的输出外，也可以用`ostream`类的成员函数来进行基于字节的输出操作。

例如：

```c++
//输出一个字节。
ostream& ostream::put(char ch); 
cout.put('A');
//输出p所指向的内存空间中count个字节。
ostream& ostream::write(const char *p,int count);
char info[100];  
int n;
......
cout.write(info,n); 
```



### 控制台输入操作

基于抽取操作符（>>）的基本数据类型数据的输入

```c++
#include <iostream>
using namespace std;
......
int x;
double y;
char str[10];
cin >> x; cin >> y; cin >> str;
//或者
cin >> x >> y >> str;
```

在输入的各个数据之间用空白符（`空格`、`\t`、`\n`）分开：

- 输入前先跳过空白符

- 输入过程中碰到空白符或当前数据类型不允许的字符结束

可以通过一些操纵符来控制输入的行为，例如:

```c++
char str[10];
cin >> setw(10) >> str; //把输入的字符串和一个'\0'放入str中，最多输入9个字符。
```

除了抽取操作符“>>”外，还可以使用`istream`类的成员函数来进行基于字节的输入操作。

例如：

```c++
//输入一个字节。
istream::get(char &ch); 
//输入count个字节至p所指向的内存空间中。
istream::read(char *p,int count);
//输入一个字符串。输入过程直到输入了count-1个字符或
//遇到delim指定的字符为止，并自动加上一个'\0'字符。
istream::getline(char *p, int count, char delim=’\n’); 
```



### 操作符“>>”和“<<”的重载 

标准库中重载的操作符“>>”和“<<”只能对基本数据类型的数据进行输入/输出。

可以针对自定义的类进一步重载操作符“>>”和“<<”，从而实现能用它们对某个类的对象进行输入/输出操作。 

#### 例：插入操作符“<<”的重载 

```c++
class A
{		int x,y;
	public:
		......
	friend ostream& operator << (ostream& out, const A &a);
};
ostream& operator << (ostream& out, const A &a)
{	out << a.x << ',' << a.y;
	return out;
}
.....
A a1,a2;
cout << a1 << endl << a2 << endl;
```



#### 派生类输出操作符“<<”的实现

```c++
class A
{		int x,y;
	public:
		......	
	friend ostream& operator << (ostream& out, const A &a);
};
ostream& operator << (ostream& out, const A& a)
{	out << a.x << ',' << a.y;
	return out;
}
class B: public A
{		double z;
	public:
		......
};
......
A a; 
B b;
cout << a << endl 
cout << b << endl; //只输出了b.x和b.y
```

```c++
class A
{		int x,y;
	public:
		......	
	friend ostream& operator << (ostream& out, const A &a);
};
ostream& operator << (ostream& out, const A& a)
{	out << a.x << ',' << a.y;	return out;
}
class B: public A
{		double z;
	public:
		......
	friend ostream& operator << (ostream&,const B&);
};
ostream& operator << (ostream& out, const B& b)
{	out << (A&)b << ',' << b.z;	return out;
}
......
B b;
cout << b << endl; //OK
A *p=new B;
cout << *p << endl; //只输出了p->x和p->y
```

```c++
class A
{		int x,y;
	public:
		......	
		virtual void display(ostream& out) const
		{	out << x << ',' << y ; }
};
ostream& operator << (ostream& out, const A& a)
{	a.display(out); //动态绑定到A类或B类对象的display。
	return out;
}
class B: public A
{		double z;
	public:
		......
		void display(ostream& out) const
		{	A::display(out); out << ',' << z ;   	}
};
A a; B b;
cout << a << endl << b << endl; //OK
```



### 面向文件的I/O

需求：

- 程序运行结果有时需要永久性地保存起来，以供其它程序或本程序下一次运行时使用。

- 程序运行所需要的数据也常常要从其它程序或本程序上一次运行所保存的数据中获得。

用于永久性保存数据的设备称为外部存储器（简称：外存），如：

- 磁盘、磁带、光盘等。

在外存中保存数据的方式通常有两种：

- 文件

- 数据库



### 文件的基本概念

在C++中，把文件看成是由一系列字节所构成的字节串，对文件中数据的操作（输入/输出）通常是逐个字节顺序进行，因此称为流式文件。 

每个打开的文件都有一个内部（隐藏）的位置指针，它指出文件的当前读写位置。

进行读/写操作时，每读入/写出一个字节，文件位置指针会自动往后移动一个字节的位置。



### 文件数据的存储方式 

文本方式（text）

- 只包含可显示的字符和有限的几个控制字符（如：‘\r’、‘\n’、‘\t’等）的编码。

- 例如，以文本方式存储整数1234567 ：
  - 把字符1、2、3、4、5、6、7的ASCII码（共7个字节）依次写入文件。

- 一般用于存储具有“行”结构的文本数据。（可用记事本等软件打开察看）

二进制方式（binary） 

- 包含任意的没有显式含义的纯二进制字节。

- 例如，以二进制方式存储整数1234567 ：
  - 把1234567的int型机内表示`00 12 D6 87`（共4个字节）依次写入文件。

- 一般用于存储任意结构的数据。



### 文件的读写过程

在外存（如磁盘）中，每个文件都有一个名字（文件名）（操作系统一般采用树型的目录结构来管理外存中的文件）。

对文件数据进行读写的过程：

- 打开文件：把程序内部的一个表示文件的变量/对象与外部的一个具体文件关联起来，并创建内存缓冲区。

- 文件读/写：存取文件中的内容。

- 关闭文件：把暂存在内存缓冲区中的内容写入到文件中，并归还打开文件时申请的内存资源（包括内存缓冲区）。  

在利用I/O类库中的类进行文件的输入/输出时，程序中需要包含下面的头文件： 

```c++
#include <iostream> 
#include <fstream>
```



### 文件输出操作

打开文件：创建一个`ofstream`类的对象，并建立与外部文件之间的联系。

- 直接方式：

  ```c++
  ofstream out_file(<文件名> [,<打开方式>]);
  //例如：
  ofstream out_file("d:\\myfile.txt",ios::out);
  ```

- 间接方式：

  ```c++
  ofstream out_file;
  out_file.open(<文件名> [,<打开方式>]);
  //例如：
  out_file.open("d:\\myfile.txt",ios::out);
  ```

打开方式 

- `ios::out`
  - 打开一个外部文件用于写操作。
  - 如果外部文件已存在，则首先把它的内容清除；否则，先创建该外部文件。
  - `ios::out`是默认打开方式。

- `ios::app`
  - 打开一个外部文件用于添加（文件位置指针在末尾）操作。
  - 如果外部文件不存在，则先创建该外部文件。

- `ios::out`或`ios::app`分别与`ios::binary`的按位或（|）
  - `ios::out | ios::binary` 或 `ios::app | ios::binary`
  - 表示按二进制方式打开文件。（默认的是文本方式）
  - 对以文本方式打开的文件，当输出的字符为'\n'时，在某些平台上（如：DOS和Windows平台）将自动转换成'\r'和'\n'两个字符写入外部文件。



### 判断打开操作是否成功

打开文件时，必须要对文件打开操作的成功与否进行判断。判断文件是否成功打开可以采用以下方式： 

```c++
if (!out_file.is_open())  //或：out_file.fail() 
				            //或：!out_file
{ ...... //失败处理
} 
```



### 输出数据

文件成功打开后，可以使用插入操作符“<<”或`ofstream`类的一些成员函数来进行文件数据的输出操作，例如： 

```c++
int x=12;
double y=12.3;
......
//以文本方式输出数据
ofstream out_file("d:\\myfile.txt",ios::out);
if (!out_file) exit(-1);
out_file << x << ' ' << y << endl; //12 12.3
```

或：

```c++
//以二进制方式输出数据
ofstream out_file("d:\\myfile.dat",ios::out|ios::binary);
if (!out_file) exit(-1);
out_file.write((char *)&x,sizeof(x)); //4个字节
out_file.write((char *)&y,sizeof(y)); //8个字节
```

```c++
struct Student
{ int no;
   char name[10];
   int scores[5];
} s1={161220042,"张三",{90,95,85,75,95}};
//以文本方式输出数据
ofstream out_file("d:\\students.txt",ios::out);
if (!out_file) exit(-1);
out_file << s1.no << ' ' << s1.name;
for (int i=0; i<5; i++)
  out_file << ' '<< s1.scores[i];
out_file << endl; 
//以二进制方式输出数据
ofstream out_file("d:\\students.dat",ios::out|ios::binary);
out_file.write((char *)&s1,sizeof(s1));
```

文件输出操作结束时，要使用ofstream的成员函数close关闭文件：

```c++
out_file.close(); 
```

关闭文件的目的：

- 把文件内存缓冲区的内容写到磁盘文件中！

程序正常结束时，系统也会自动关闭打开的文件。



### 文件输入操作

打开文件：创建一个ifstream类的对象，并与外部文件建立联系。例如：

```c++
ifstream in_file(<文件名> [,<打开方式>]); 
//或
ifstream in_file;
in_file.open(<文件名> [,<打开方式>]); 
```

打开方式

- `ios::in`，打开一个外部文件用于读操作。（默认）

- 也可以把`ios::in`与`ios::binary`通过按位或操作（|）实现二进制打开方式。默认为文本方式。

- 对以文本方式打开的文件，当文件中的字符为连续的'\r'和'\n'时，在某些平台上（如：DOS和Windows平台）读入时将自动转换成一个字符'\n'输入。

- 打开文件时要判断打开是否成功。
  - 判断方式与文件输出打开操作的判断一样。

文件成功打开后，可以使用抽取操作符“>>”或ifstream类的一些成员函数来进行文件输入操作，例如： 

```c++
int x;
double y;
......
//以文本方式输入数据
ifstream in_file("D:\\myfile.txt",ios::in);
if (!in_file) exit(-1);
in_file >> x >> y; 

//以二进制方式输入数据
ifstream in_file("D:\\myfile.dat",ios::in|ios::binary);
if (!in_file) exit(-1);
in_file.read((char *)&x,sizeof(x)); 
in_file.read((char *)&y,sizeof(y)); 
```

注意：从文件输入必须要知道文件中数据的存储方式和格式！

```c++
struct Student
{ int no;
   char name[10];
   int scores[5];
} s1;
//以文本方式输入数据
ifstream in_file("d:\\students.txt",ios::in);
if (!in_file) exit(-1);
in_file >> s1.no >> s1.name;
for (int i=0; i<5; i++)
  in_file >> s1.scores[i];
//以二进制方式输入数据
ifstream in_file("d:\\students.dat",ios::in|ios::binary);
in_file.read((char *)&s1,sizeof(s1));
```

文件输入操作结束后，要使用`ifstream`的一个成员函数close关闭文件：

```c++
in_file.close(); 
```

读取数据过程中有时需要判断是否正确读入了数据（尤其是在文件末尾处）。

判断是否正确读入了数据，可以调用ios类的成员函数fail来实现：

```c++
bool ios::fail() const;
```

- 该函数返回true表示文件操作失败；返回false表示操作成功。



#### 例：从文件读入一系列整型数

```c++
//文件中的数据形式

1 2 3 4\n
------------------
1\n
2\n
3\n
4\n
------------------
1 2 3 4
------------------
1\n
2\n
3\n
4
```

```c++
......
ifstream in_file("d:\\myfile.txt",ios::in);
if (!in_file) exit(-1);
int x; 
in_file >> x; //读入第一个整型数
while (!in_file.fail())
{ ...... //使用x的值
    in_file >> x; //读入下一个整型数
}
in_file.close();
......
```



### 有关文件读写的几点注意

以文本方式输出的文件要以文本方式输入；以二进制方式输出的文件要以二进制方式输入！

以文本方式读写的文件要以文本方式打开；以二进制方式读写的文件要以二进制方式打开！

以二进制方式存取文件不利于程序的兼容性和可移植性。例如，

- 在不同计算机平台上，整型数的各字节在内存中的存储次序可能不一样。

- 在不同的编译环境下，同样的结构类型数据的尺寸（字节数）可能不一样。

- ......



### 打开既能输入、又能输出的文件 

如果需要打开一个既能读入数据、也能输出数据的文件，则需要创建一个`fstream`类的对象。 

文件内部有两个位置指针，一个用于是读，另一个用于写。

在创建`fstream`类的对象并建立与外部文件的联系时，文件打开方式应为下面之一：

- `ios::in|ios::out`（可在文件任意位置写）

- `ios::in|ios::app`（只能在文件末尾写） 



### 文件的随机存取

为了能够随机读写文件中的数据，可以显示地指出读写的位置。

下面的操作用来指定文件内部读指针的位置：

```c++
istream& istream::seekg(<位置>)；//指定绝对位置
istream& istream::seekg(<偏移量>,<参照位置>); //指定相对位置
streampos istream::tellg(); //获得指针位置
```

下面的操作来指定文件内部写指针的位置：

```c++
ostream& ostream::seekp(<位置>)；//指定绝对位置
ostream& ostream::seekp(<偏移量>,<参照位置>); //指定相对位置
streampos ostream::tellp(); //获得指针位置 
```

<参照位置>可以是：`ios::beg`（文件头），`ios::cur`（当前位置）和`ios::end`（文件尾）。



## Lecture 23 异常处理

### 异常概述

程序的错误通常包括：

- 语法错误：指程序的书写不符合语言的语法规则。

  例如：

  - 使用了未定义或未声明的标识符

  - 左右括号不匹配
  - ......

  这类错误可由编译程序发现。

- 逻辑错误（或语义错误）：指程序设计不当造成程序没有完成预期的功能。

  例如：
  - 把两个数相加写成了相乘
  - 排序功能未能正确排序
  - ......

  这类错误可通过对程序进行静态分析和动态测试发现。

- 运行异常：指程序设计对程序运行环境考虑不周而造成的程序运行错误。

  例如：

  - 对于“x/y”操作，给y输入了“零”。

  - 由内存空间不足导致的内存访问错误：

  ```c++
  int *p=new int; //动态分配空间，可能失败！
  *p = 10; //如果上面new操作失败，p可能为空指针！
  ```
  - 输入数据的数量超过存放它们的数组的大小，导致数组下标越界。
  - 多任务环境可能导致的文件操作错误。

  ...... 

> 在程序运行环境正常的情况下，运行异常的错误是不会出现的。
>
> 导致程序运行异常的情况是可以预料的，但它是无法避免的。
>
> 为了保证程序的鲁棒性（Robustness），必须在程序中对可能出现的异常进行预见性处理。

例如，下面程序的鲁棒性不高！

```c++
void f(char *filename)
{	ifstream file(filename);
	int x;
	file >> x; //如果打开filename指定的文件
			//失败，将会出现运行异常!
   ... x ...  //异常时，x的值不正确！
	......
}
```



### 异常处理的策略

就地处理

- 在发现异常错误的地方处理异常

异地处理

- 在其它地方（非异常发现地）处理异常



### 异常的就地处理

常用做法是调用C++标准库中的函数exit或abort终止程序执行（在`cstdlib`或`stdlib.h`中声明）

- `abort`立即终止程序的执行，不作任何的善后处理工作。

- `exit`在终止程序的运行前，会做关闭被程序打开的文件、调用全局对象和static存储类的局部对象的析构函数（注意：不要在这些对象类的析构函数中调用exit）等工作。

例如：

```c++
void f(char *filename)
{	ifstream file(filename);
	if (file.fail())
  { cerr << "文件打开失败\n";
	  exit(-1);	
  }
  int x;
  cin >> x;
  ......
}
```

>  不管abort还是exit，都“not user-friendly”



### 异常的异地处理

发现异常时，在发现地（如在被调用的函数中）有时不知道如何处理这个异常，或者不能很好地处理这个异常，要由程序的其它地方（如函数的调用者）来处理。

- 例如，前面的函数f中打开文件失败，这时可以由调用者重新提供一个文件来解决。

如何实现异常的异地处理？

一种解决途径：

- 通过函数的返回值，或指针/引用类型的参数，或全局变量把异常情况通知函数的调用者，由调用者处理异常。

- 例如：

  ```c++
  int f(char *filename)
  { ifstream file(filename);
     if (file.fail()) 
       return -1; //把错误情况告诉调用者
     int x;
     cin >> x;   
     ......
     return 0;
  }    
  
  int main()
  {  char str[100];
      ......
      int rc=f(str)；
      if (rc== -1)
      { ...... //处理异常
      }
  	 else
  	 { ...... //正常情况
  	 }
      ......
  }
  ```

该途径的不足：

- 通过函数的返回值返回异常情况会导致正常返回值和异常返回值交织在一起，有时无法区分。

- 通过指针/引用类型的参数返回异常情况，需要引入额外的参数，给函数的使用带来负担。

- 通过全局变量返回异常情况会导致使用者忽视这个全局变量的问题。（不知道它的存在）

- 程序的可读性差！程序的正常处理与异常处理混杂在一起。

另一种解决异常的异地处理途径：

- 通过语言提供的结构化异常处理机制进行处理。



### C++结构化异常处理机制

把有可能遭遇异常的一系列操作（语句或函数调用）构成一个try语句块。

如果try语句块中的某个操作在执行中发现了异常，则通过执行一个throw语句抛掷（产生）一个异常对象，之后的操作不再进行。

抛掷的异常对象将由程序中能够处理这个异常的地方通过catch语句块来捕获并处理之。 

```c++
void f(char *filename)
{ ifstream file(filename);
   if (file.fail()) 
     throw filename; //产生异常对象，报告错误情况
   int x;
   cin >> x;
   ......
   return 0;
} 
int main()
{  char str[100];
    ......
    try { f(str); }
    catch (char *fn) //捕获异常
    { ...... //处理异常
    }
    ...... //正常情况
}
```



### try语句

try语句块的作用是启动异常处理机制。格式为：

```c++
try
{ <语句序列>
}
```

上述的<语句序列>中可以有函数调用。例如，

```c++
......
try
{ f(str);
}
......
```



### throw语句

throw语句用于在发现异常情况时抛掷（产生）异常对象。格式为：

```c++
throw <表达式>;
```

>  <表达式>为任意类型的C++表达式（void除外）。

例如:

```c++
void f(char *filename)
{ ifstream file(filename);
  if (file.fail())
  { throw filename; //产生异常对象（一个字符串指针）
  }
  ......
}
```

执行throw语句后，接在其后的语句将不再继续执行，而是转向异常处理（由某个catch语句给出）。



### catch语句

catch语句块用于捕获throw抛掷的异常对象并处理相应的异常。格式为：

```c++
catch (<类型> [<变量>])
{ <语句序列>
}
```

- <类型>用于指出捕获何种异常对象，它与throw所产生的异常对象的类型匹配规则与函数重载的绑定规则类似；

- <变量>用于存储异常对象，它可以缺省，缺省时表明catch语句块只关心异常对象的类型，而不考虑具体的异常对象。

catch语句块要紧接在某个try语句的后面。

例如：

```c++
char filename[100];
cout << “请输入文件名：” << endl;
cin >> filename;
try
{	f(filename);//如果在函数f中抛掷了char *类型的异常，
                 //则程序转到try后面的catch(char *str)处理。
}
catch (char *str)
{	cout << str << “不存在!”<< endl;
	cout << “请重新输入文件名：” << endl;
	cin >> filename;
	f(filename);
}
```

一个try语句块的后面可以跟多个catch语句块，用于捕获不同类型的异常对象并进行处理。例如：

```c++
void f()
{ ......
   ...throw 1;
   ......
   ...throw 1.0;
   ......
   ...throw "abcd";
   .......
}

int g()
{ ......
   try
   { f();
   }
   catch (int) //处理函数f中的throw 1;
   { <语句序列1>
   }
   catch (double) //处理函数f中的throw 1.0
   { <语句序列2>
   }
   catch (char *) //处理函数f中的throw "abcd"
   { <语句序列3>
   }
   <非catch语句>
}
```

如果在try语句块的<语句序列>执行中没有抛掷（throw）异常对象，则其后的catch语句不执行，而是继续执行try语句块之后的非catch语句。

如果在try语句块的<语句序列>执行中抛掷了（throw）异常对象，

- 如果该try语句块之后有能够捕获该异常对象的catch语句，则执行这个catch语句中的<语句序列>，然后继续执行这个catch语句之后的非catch语句。

- 如果该try语句块之后没有能够捕获该异常对象的catch语句，则按嵌套的异常处理规则进行处理。



### 异常处理的嵌套 

try语句是可以嵌套的：

- 在try语句块的语句序列执行过程中还可以包含try语句块。

当在内层的try语句的执行中产生了异常，则首先在内层try语句块之后的catch语句序列中查找与之匹配的处理，如果内层不存在能捕获相应异常的catch，则逐步向外层进行查找。 

如果抛掷的异常对象在程序的函数调用链上没有给出捕获，则调用系统的terminate函数进行标准的异常处理。默认情况下，terminate函数将会去调用abort函数。

```c++
void f()
{ try
  { g();
  }
  catch (int)
  {	......
  }
  catch (char *)
  { ......
  }
} 
```

```c++
void g()
{ try
  { h();
  }
  catch (int)
  { ......
  }
  ......
  ... throw 2; //由f捕获并处理
}
```

```c++
void h()
{	......
	... throw 1; //由g捕获并处理
	......
	... throw "abcd"; //由f捕获并处理
	......
}
```



### 异常处理实现机制（示意）

每个函数都有一个catch表。

每进入一个try，都会把其后的所有catch入口地址记录在相应函数的catch表中。

执行throw时，

- 顺着函数调用链去搜索catch入口；

- 对之前函数调用的栈空间进行退栈处理；

- 转到搜索到的catch入口。

![image-20210610085427396](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210610085427396.png)



### 例：一种处理除数为0的异常错误

```c++
#include <iostream>
using namespace std;

int divide(int x, int y)
{ if (y == 0) throw 0;
   return x/y; 
}

void f() //其中用到两个数相除操作
{	int a,b;
	try
	{ 	cout << "请输入两个数：";
		cin >> a >> b;
		int r=divide(a,b);
		cout << a << "除以" << b << "的商为：" << r << endl;
	}
	catch(int)
	{	cout << "除数不能为0，请重新输入两个数：";
		cin >> a >> b;
		int r=divide(a,b);
		cout << a << "除以" << b << "的商为：" << r << endl;
	}
    ......
}

int main()
{	try
	{ f();
	}
	catch (int)
	{	cout << "请重新运行本程序！"<< endl;
	}
	return 0;
} 
```

> 如何做到程序不终止，一直到获得正确的数据为止？



### 基于断言的程序调试

一个处于开发阶段的程序可能会含有一些错误（逻辑错误或异常）。

- 通过测试可以发现程序存在错误。

- 通过调试可以对错误进行定位。

除了利用调试工具以外，一种常用的调试手段是：

- 在程序中的某些地方加上一些输出语句，在程序运行时把一些调试信息（如变量的值）输出到显示器。

这种调试手段存在以下问题：

- 调试者需要对输出的值做一定的分析才能知道程序是否有错。

- 在开发结束后，去掉调试信息有时是一件很繁琐的工作。



### 断言（assertion）

实际上，在调试程序时输出程序在一些地方的某些变量或表达式的值，其目的是为了确认程序运行到这些地方时状态是否正确。

上述目的可以在程序的一些关键或容易出错的点上插入一些断言来表达。

- 断言（assertion）是一个逻辑表达式，它描述了程序执行到断言处应满足的条件。

- 如果条件满足则程序继续执行下去，否则程序异常终止。

在程序开发阶段，断言既可以用来帮助开发者发现程序的错误，也可以用于错误定位。



### 宏assert

C++标准库提供的一个宏assert（在头文件cassert或assert.h中定义），可以用来实现断言。其格式为：

```c++
assert(<表达式>);
```

- <表达式>一般为一个关系/逻辑表达式

assert执行时，

- 如果<表达式>的值为true，程序继续正常执行。

- 如果<表达式>的值为false，则它会:
  - 首先，显示出相应的表达式、该assert所在的源文件名以及所在的行号等诊断信息；

  - 然后，调用库函数abort终止程序的运行。



例如，下面的宏assert调用表示程序执行到该宏调用处变量x的值应等于1：

```c++
assert(x == 1);
```

当程序执行到该调用处，如果x的值不等于1，则它会显示下面的信息并终止程序的运行：

```
Assertion failed: x == 1, file XXX, line YYY
```

- 其中的XXX表示相应调用所在的源文件名，YYY表示调用所在的源程序中的行号。

assert也可用于发现异常错误。例如，

```c++
int divide(int x, int y)
{ assert(y != 0);
  return x/y; 
}
```



### 宏assert的实现

宏assert是通过条件编译预处理命令来实现的，其实现细节大致如下：

```c++
//cassert 或 assert.h
......
#ifdef  NDEBUG
#define assert(exp)  ((void)0)
#else
#define assert(exp)  ((exp)?(void)0:<输出诊断信息并调用库函数abort>)
#endif
......
```

宏assert只有在宏名`NDEBU`G没定义时才有效，这时，程序一般处于开发、测试阶段。程序开发结束提交时，应该让宏名NDEBUG有定义，然后重新编译程序，这样，assert就不再有效了。

宏名`NDEBUG`在哪儿定义？

- 在编译命令中指出。例如：

```bash
cl <源文件1> <源文件2> ... -D NDEBUG ...
```

- 在开发环境中的“项目|...属性|C/C++|预处理器|预处理器定义”中指出。（VC++2015）

![image-20210610090414312](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210610090414312.png)

![image-20210610090424618](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210610090424618.png)



## Lecture 24 事件（消息）驱动的程序设计

### Windows简介

Windows是一种基于图形界面的多任务操作系统。

- 系统中可以同时运行多个应用程序。

- 每个应用程序通过各自的“窗口”与用户进行交互。 

- 用户通过鼠标的单击/双击/拖放、菜单选择以及键盘输入来与程序进行交互。

Windows的功能以两种方式提供：

- 工具（应用程序）：资源管理器、记事本、画图、......，供用户（人）使用。

- 函数库：作为Windows的应用程序接口（API），以C语言函数形式提供（在windows.h等头文件中申明），供应用程序使用。



### Windows应用程序的类型

单文档应用

- 只能对一个文档的数据进行操作。

- 必须首先结束当前文档的所有操作之后，才能进行下一个文档的操作。 

多文档应用

- 同时可以对多个文档的数据进行操作。

- 不必等到一个文档的所有操作结束，就可以对其它文档进行操作，对不同文档的操作是在不同的子窗口中进行的。

对话框应用

- 以对话框的形式操作一个文档数据。

- 对文档数据的操作以各种“控件”来实现。

- 程序以按<确定>或<取消>等按钮来结束。



### 事件驱动的程序结构

Windows应用程序采用的是一种基于事件（消息）驱动的交互式流程控制结构：

- 程序的任何一个动作都是由某个事件激发的。

- 事件可以是用户的键盘、鼠标、菜单等操作。

每个事件都会向应用程序发送一些消息，例如：

- WM_KEYDOWN/WM_KEYUP（键盘按键）

- WM_CHAR（字符）

- WM_LBUTTONDOWN/WM_LBUTTONUP/WM_LBUTTONDBLCLK/WM_MOUSEMOVE （鼠标按键）

- WM_COMMAND（菜单）

- WM_PAINT（窗口内容刷新）

- WM_TIMER（定时器消息）

  ......

每个应用程序都有一个消息队列。

- 系统会把属于各个应用程序的消息放入各自的消息队列。

大部分的消息都关联到某个窗口。

- 每个窗口都有一个消息处理函数。

应用程序不断地从自己的消息队列中获取消息并调用相应窗口的消息处理函数来处理获得的消息。

- 这个“取消息-处理消息”的过程构成了消息循环。

- 当取到某个特定消息（如：WM_QUIT）后，消息循环结束。

```c++
主程序：
    //初始化
    ......
//进入消息循环
   while (取消息)
	{  ......
       处理消息 
	}
```

![image-20210610092433419](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210610092433419.png)

消息处理函数：

```c++
......
```

注意：每个消息的处理时间不宜太长，否则会造成程序“假死”现象（程序不响应其它消息）。



### 基于Windows API的事件驱动程序设计（过程式）

每个应用程序都必须提供一个主函数WinMain，其主要功能是：

- 注册窗口类（窗口的基本信息）：
  - 窗口类的名字、窗口的基本风格、消息处理函数、图标、光标、背景颜色以及菜单等。
  - 每类窗口（不是每个窗口）都需要注册。

- 创建应用程序的主窗口（其它窗口等到需要时再创建）。

- 进入消息循环，直到接收到WM_QUIT消息时，消息循环结束。

```c++
WinMain(...)
{  //注册窗口类
    RegisterClass(...);
//创建主窗口
    CreateWindow(...);
//进入消息循环
   while (GetMessage(...)) //消息队列
	{  ......
      DispatchMessage(...); 
	}
}
```

```c++
//消息处理函数：
WindowProc(...,message,...)
{ switch(message)
   { case ...:
      case ...:
      ......
   }
}
```

#### 示例

```c++
//Windows应用程序的主函数
#include <windows.h> //Windows所提供的API声明文件。
int APIENTRY WinMain(HINSTANCE hInstance, //本实例标识（Handle）
                      	 HINSTANCE hPrevInstance, //上一个实例标识
                      	 LPSTR lpCmdLine, //命令行参数	
			            int  nCmdShow ) //主窗口显示方式
{	//注册窗口类（下面是个示意，函数参数实际为一个结构WNDCLASS）
	RegisterClass(..., WindowProc, "my_window_class"); //示意
	......
	//创建并显示主窗口
	HWND hWnd;
	hWnd=CreateWindow("my_window_class",…,x,y,width,height,...); 
	ShowWindow(hWnd, nCmdShow);
	......
    //消息循环，直到接收到WM_QUIT消息
	while (GetMessage(&msg, NULL, 0, 0)) //从消息队列中取消息。
	{	......
		DispatchMessage(&msg); //把消息发送到程序相应的窗口。
	}
	return msg.wParam;
}
```

```c++
//窗口的消息处理函数
LRESULT CALLBACK WindowProc(HWND hWnd, //窗口标识
 	  			                UINT message, //消息标识
				                WPARAM wParam, //消息的参数1
				                LPARAM lParam ) //消息的参数2
{	switch (message)
	{	case WM_KEYDOWN:
		   ...wParam... //wParam为按键在键盘上的位置（扫描码）
		   ......
		case WM_CHAR: //字符键消息
		    ...wParam... //wParam是按键字符的编码
		    ......
		case WM_COMMAND: //菜单消息
		    switch (wParam) //wParam是菜单项的标识
		    { case ID_FILE_OPEN:
			......
		       case ID_START_TIMER:
			SetTimer(hWnd,1,1000,NULL);
			......
		       ......
		    }
		    ......	
        case WM_LBUTTONDOWN: //鼠标左键按下消息
		    ...lParam... //lParam为鼠标在窗口中的位置
		    ......
		case WM_PAINT: //窗口刷新消息
		   ......
		case WM_TIMER: //定时器消息
		   ......
		case WM_CLOSE: //请求关闭窗口消息
		   DestroyWindow(hWnd); //撤销窗口
		   break;
		case WM_DESTROY: //窗口被关闭消息
		   PostQuitMessage(0); //往本应用的消息队列中放入WM_QUIT
		   break;
		default: //由系统进行默认的消息处理
		    return DefWindowProc(hWnd,message,wParam, lParam);
  	}
  	return 0;
}
```



### 消息处理函数应是可再入的

消息处理函数中还可以自己生成新的消息，方式有两种：

- PostMesaage（消息放入消息队列）

- SendMessage（直接调用消息处理函数）

SendMessage可能导致消息处理函数的一次执行还未结束，另一次执行就开始的现象，这可能会引起数据的不一致错误！

因此，消息处理函数应该是一个可再入（可重入）函数（reentrant function）：

- 函数需要的数据要通过参数来传递。

- 函数不能有static存储类的局部变量。

- 在函数中访问全局变量也可能导致函数不可再入。

可用VC++的应用向导建立基于Windows API的事件驱动的应用程序基本框架：

![image-20210610093237231](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210610093237231.png)



### 资源（Resource）

每个Windows应用程序，除了程序代码外，还包含一些资源描述：

- 菜单：菜单ID、菜单项ID/显示文字。

- 对话框：对话框ID和尺寸；对话框中各个控件的ID、类型、尺寸与位置等。

- ......

资源描述有规定的格式，存储在相应的资源文件（.rc）中，经编译后将作为Windows应用程序的一部分被链接到应用程序的目标文件中。

资源可以用VC++的资源编辑器来进行可视化编辑。

可用VC++的资源编辑器来生成资源：

![image-20210610093413208](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210610093413208.png)



### 面向对象的事件驱动程序设计

基于Windows API的事件驱动程序设计是一种基于过程抽象的程序设计范式。

通过调用API函数编写程序的粒度太细、太繁琐，开发效率不高。

如何以更大粒度的程序元素（如，对象）来开发事件驱动的应用程序？



### Windows应用程序中的对象

窗口对象

- 显示程序的处理数据。

- 处理Windows的消息、实现与用户的交互。

- 窗口对象类之间可以存在继承和组合关系。 

文档对象

- 管理在各个窗口中显示和处理的数据。

- 文档对象与窗口对象之间可以存在关联关系。

应用程序对象

- 管理属于它的窗口对象和文档对象。

- 实现消息循环。

- 它与窗口对象及文档对象之间构成了组合关系。

  ......

对于每一个Windows应用

- 应用程序对象和主窗口对象只有一个

- 子窗口对象和文档对象则可以有多个，它们在程序运行的不同时刻创建。

例如，对于一个多文档的Windows应用程序，

- 在程序开始运行时，首先创建应用程序对象；

- 由应用程序对象来创建主窗口对象；

- 在程序运行过程中，用户选择主窗口菜单项“文件|打开”，创建一个文档对象以及相应的子窗口对象。



### Microsoft Foundation Class library（MFC，微软基础类库）

如何为上述的对象设计类？

MFC是微软公司提供的支持以面向对象范式进行Windows应用程序开发的一个基础类库（Microsoft Foundation Class library）。

- MFC提供了一些类来描述应用中对象的基本功能，应用程序可以通过继承这些类来实现各自的特殊功能。

- MFC还提供了一种基于“文档－视”结构的应用框架。

![image-20210610142052161](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210610142052161.png)

![image-20210610142101508](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210610142101508.png)

![image-20210610142115266](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210610142115266.png)



### MFC提供的主要类

基本窗口类（CWnd）

- 实现窗口的基本功能：
  - 一般的消息处理、窗口大小和位置管理、菜单管理、坐标系管理、滚动条管理、剪贴板管理、窗口状态管理、窗口间位置关系管理，等等。

- 是其它窗口类的基类。

框架窗口类

- 提供对标题栏、菜单栏、工具栏、状态栏以及属于它的子窗口的管理功能。

- CFrameWnd：提供了单文档应用主窗口的基本功能

- CMDIFrameWnd：提供了多文档应用主窗口的基本功能

- CMDIChildWnd：提供了多文档应用子窗口的基本功能

视类（CView）

- 实现程序数据的显示功能以及操作数据时与用户的交互功能。

- 视窗口（简称视）通常位于单文档应用主窗口（CFrameWnd）和多文档应用子窗口（CMDIChildWnd）的客户区（可显示区）。

文档类（CDocument）

- 对程序要处理的数据进行管理，包括磁盘文件输入/输出。

应用类（CWinApp）

- 提供了对Windows应用程序的各部分进行组合和管理的功能，其中包括实现消息循环等。

对话框类（CDialog）

- CDialog类封装了对话框的基本功能，它是所有对话框类的基类。

绘图类

- 实现图形绘制的基本功能。包括：CDC、CPen、CFont、CBrush等。

文件输入/输出类

- CFile类：实现了基于字节流的文件I/O。

- CArchive类：实现了对基本数据类型以及MFC中从CObject继承的类对象的文件I/O。

常用数据类型类

- CPoint、CRect、CSize、CString、CArray、CList、CMap、...

  ......

> MFC提供了以面向对象的方式进行Windows应用程序开发所需要的类。
>
> 但如何使用这些类来构建和组织程序，比较灵活，存在多种方式。
>
> 应用框架是一种能够提高开发效率的一种途径。



