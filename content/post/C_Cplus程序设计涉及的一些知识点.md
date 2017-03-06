+++ 
title = "C_Cplus程序设计涉及的一些知识点" 
date = "Tue, 30 Aug 2016 10:47:00 GMT" 
tags = ["C++"] 
categories = ["Language"]
description = "C++中的一些小知识点（printf压栈顺序，指针引用，动态内存以及变量的存储位置，构造函数，this指针等问题的回顾） " 
+++ 



### c中的printf函数

```
main() {
	int b = 3;
	int arr[]= {6,7,8,9,10};

	int * ptr = arr;
    *(ptr++) += 123;
    printf("%d,%d\n",*ptr,*(++ptr));
    return 0;
}

```
> 输出 ：8,8

> 原因：printf函数计算参数是**从右往左压栈的**


###  c和c++的关系
- c++ 语言支持函数重载，c语言不支持函数重载；
- 函数在C++编译后在库中的名字和C语言的不同，假设某个函数原型是void foo（int x， int y）。该函数被C编译器编译后，在库中的名字是_foo,而在C++编译器中则会产生像_foo_int_int之类的名字。
- C++提供了C连接交换指定符 extern "c" 解决名字匹配的问题。

###  指针和引用

区别：

- 非空区别，在任何情况下都不能使用指向空值的引用。一个引用必须总是指向某些对象；
- 合法性，在使用引用之前不需要测试它的合法性，相反，指针则应该总是被测试，防止其为空；
- 可修改区别，指针可被重新赋值以指向另一个不同对象。但是引用则总是指向在初始化被指定的对象，以后不能改变，但是指向的对象的其内容可以改变
- 应用区别，有存在不指向任何对象的可能和不同时刻会指向不同的对象等情况时，使用指针；如果总是指向一个对象并且指向一个对象以后就不会改变指向，那么应该使用引用。

###  传递动态内存

```
void swap3(int *p, int *q) {
	int *tmp;
    tmp = p;
    p = q;
    q = tmp;//仅仅交换了指针的值，并没有改变两块内存的值
}

void swap4(int *p, int *q) {
	int tmp;
    tmp = *p;  //获取p指向内存的值
    *p = *q;
    *q = tmp;
}

void swap5(int &p, int &q) {
	int tmp;
    tmp = p; //获取p指向内存的值
    p = q;
    q = tmp;
}
```

//------------

```
    int t1 = 1,t2 = 2;
    cout << t1 << ""<< t2 << endl;
    swap3(&t1,&t2); //仅仅交换了指针的值，并没有改变两块内存的值
    cout << t1 << ""<< t2 << endl;
    // 后面两个函数分别用指针和引用交换成功
    swap4(&t1,&t2);
    cout << t1 << ""<< t2 << endl;
    swap5(t1,t2);
    cout << t1 << ""<< t2 << endl;
```

###  malloc 申请内存

```
void getMemory_err(char *p, int num) {
p = (char *)malloc(sizeof(char )* num);
        // 没有return 指针，不能传递动态内存
}

//返回指针变量
char *pGetMemory(char *p, int num) {
p = (char *)malloc(sizeof(char )* num);
        return p;
}

// 用指针的指针，
void getMemory(char **p, int num) {
*p = (char *)malloc(sizeof(char )* num);
}
```

//------

```
char *str = NULL;
getMemory(&str,10);
strcpy(str,"hello");

char *str2 = NULL;
str2 = pGetMemory(str2,10);
strcpy(str2,"test");

cout<< *str <<endl;
cout<< str << endl;
cout<< &str << endl;

cout<< *str2 <<endl;
cout<< str2 << endl;
cout<< &str2 << endl;
```

output:

```
    h
    hello
    012FF774
    t
    test
    012FF768
```

###  字符串变量存储问题

```
int a = 0; //全局初始化区
char *p1; //全局未初始化区
int _tmain(int argc, _TCHAR* argv[]) {
int b; //栈
char s[] = "abc"; //s在栈上， 常量区会存储一份"abc\0"，用于共享，s[]初始化的时候拷贝过来放到栈上
char *p2; //栈
char *p3 = "123456"; //123456\0在常量区，p3在栈上。
const char *m = "123456"; //m在栈上， “123456” 在常量区
static int c =0; //全局（静态）初始化区
p1 = (char *)malloc(10);
p2 = (char *)malloc(20);
//分配得来得10和20字节的区域就在堆区。
strcpy(p1, "123456"); //123456\0放在常量区，编译器可能会将它与p3所指向的"123456" 优化成一个地方。

cout << (m == p3 ? 1: 0) << endl;
// out 1,
cout << (p1 == p3? 1: 0) << endl;
// out 0;
cout << p1 << "———" << p3 << endl; // out 123456---123456

return 0;
}

```

//---

```
char *strA() {
	char str[] = "hello world";
    cout << "in str[] "<<endl;
    cout << "before: "<< str << endl;
    str[0] = 't';
    cout <<"After:" << str << endl;
    return str;
}

char *pStrA_err() {
	char *str = "hello world";
    cout << "in *str "<<endl;
    cout << "before: "<< str << endl;
    //*str = 'P'; // !!!!!!------runtime error 字符串常量保存在只读数据段
    cout <<"After:" << str << endl;
    return str;
}
char *pStrA(){
    static char *str = "hello world";
    cout << "in *str "<<endl;
    cout << "before: "<< str << endl;
    //*str = 'P'; // !!!!!!------runtime error  ,字符串常量保存在只读数据段
    cout <<"After:" << str << endl;
    return str;
}

```

main:

```
char *c = strA();
char *c2 = pStrA_err();
char *c3 = pStrA();

cout << "After fun return:" << "--c:" << c << "--c2:" << c2 << "--c3" << c3 << endl;
```

output:

```
    in str[]
    before: hello world
    After:tello world
    in *str
    before: hello world
    After:hello world
    in *str
    before: hello world
    After:hello world
    After fun return:--c:@?--c2:hello world--c3hello world
```


**结论：**
> - 字符串数据保存在常量区，只读的数据段，不能直接修改
> - 函数内数组是局部变量，在函数内通过str[0] 可以修改。return之后，局部变量释放，


###  构造函数

```
class tA{
public:
        int _a;
        tA(){
                _a = 1;
        }
        void print(){
                printf("%d",_a);
        }
};
class tB :public tA{
public:
        int _a;
        tB(){
                _a = 2;
        }

};
```

//--

```
tB b;
tB b;
b.print(); // 在构造B类之前，先调用A的构造函数，在A类中_a =1,
printf("%d",b._a); // B类中 _a = 2;
return 0;
    
```

>output:        12

>在构造B类之前，先调用A的构造函数

###  malloc/free ＆＆new/delete
- malloc/free 是c++/c语言的标准库函数，new/delete是c++的运算符，它们都可以用于申请和释放动态内存
- 由于malloc/free是库函数而不是运算符，不在编译器控制权限之内，不能执行构造函数和析构函数
- c++语言中new运算符， 能完成动态内存分配和初始化工作； delete 运算符完成清理和释放内存工作

###  指针和句柄
- 句柄和指针其实是两个截然不同的概念。
- windows系统用句柄标记系统资源，隐藏系统的信息，它是一个32bit的uint。
- 指针则标记某个物理内存的地址
- 句柄可以看做是指向指针的指针。

###  this 指针
- 本质上是一个函数参数，只是编译器隐藏起形式，语法层面的参数，this指针只能在成员函数中使用，全局函数，静态函数都不能使用this指针。 在python中直接显性给出
- this指针在成员函数的开始前构造，在成员的结束后清除
- this指针并不占用对象的空间， 只会占用参数传递时的栈空间，或者直接占用一个寄存器
- this指针会因编译器不同而有不同的存放位置，可能是堆，栈，也可能是寄存器。
- this指针只有在成员函数中才有定义。因此，在获得一个对象后，也不能通过对象使用this指针。所以，我们无法知道一个对象的this指针的位置（只有在成员函数中才有this指针的位置）

----------------
参考：

程序员面试宝典（第五版）



