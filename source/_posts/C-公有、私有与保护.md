---
title: C++
date: 2023-11-10
tags:
- C++
categories:
- C++
aside: false
---

**类：** 由数据成员（属性）和成员函数（方法）组成。数据成员是描述类的属性的一些数据，成员函数是描述类的行为的一些数据。比如人类，每个人都有自己的姓名、年龄、出生日期、体重等, 为人类的属性部分, 此外, 人能够吃饭、睡觉、行走、说话等属于人类所具有的行为。
**类的访问权限：** 公有（public）、私有（private）和保护（protected），能不能访问直白的说就是，你可不可以去使用类中成员（某个函数或者数据）
**公有成员：** 类内和类外都可访问。
**私有成员：** 类内可访问（个人认为只能被本类中的成员函数访问），类外不可访问。友元可访问。
**保护成员：** 类内可访问，类外不可访问。友元可访问。但是派生类可以访问（派生类可以当作类外的一种特殊形式，不在类内但是与该类有关联）。
**友元：** 友元可以是一个函数，该函数称为友元函数。友元也可以是一个类，该类被称为友元类。比如在A类里声明一个友元函数B或者友元类B，那么该函数B或者类B就可以访问类A里面的所有成员）
**继承：** 当创建一个类时，不需要重新编写新的数据成员和成员函数，只需指定新建的类继承了一个已有的类的成员即可。这个已有的类称为基类（父类），新建的类称为派生类（子类）。例如：哺乳动物是动物，狗是哺乳动物，因此，狗是动物。狗是哺乳动物的派生类，动物是哺乳动物的基类。
| 访问 | public | protected | private |
| :----: | :----: | :----: | :----: |
| 同一个类| yes | yes | yes |
| 派生类 | yes | yes | no |
| 外部的类 | yes | no | no |

举个例子：

``` bash
#include <iostream>
using namespace std;

class A         
{
    friend class B;
public: 
    void print() {cout<< x <<endl;}
protected:
    int y;
    int getnum(int i) {
        x += i; 
        return x;
    }
private: 
    int x; 
};

class B         
{
public: 
    void set(int i){a.x = i;}    //友元访问私有
    void getnum(){a.getnum(2);}  //友元访问保护
    void print(){a.print();}    //访问公有
protected: 
    //这里只能由B类的成员访问，除非B作为其余类的基类，或者友元
private: 
    A a;   //这里由于B为A的友元，所以可以访问A类所有成员。
    int z;  //只能由B类的成员访问，除非B作为友元在其他类声明
};

class C : public A
{
public:
    void func(){y = 6;}      //继承访问保护
    //void func1(){x = 2;}    //这是不可以的，继承不能访问私有
protected: 
    //这里只能由C类的成员访问，除非C作为其余类的基类，或者友元
private: 
    int m;  //只能由C类的成员访问，除非B作为友元在其他类声明
};

int main() 
{ 
    B b;
    b.set(5);     //在类外访问公有函数set，其余的公有函数都可以访问，注意这里是类外，不能访问保护和私有
    b.getnum();   
    b.print();  
    return 0;
}
```

指针的理解：

``` bash
#include <iostream>
using namespace std;
void main()
{
    int a = 10; 
    int *b ;
    b = &a;
    cout << "a的地址：" << &a << " a的值：" << a << endl
    << "指针b的值：" << b << " 指针b解地址：" << *b << endl
    << " 指针b的地址：" << &b << endl;
}
//我的电脑输出：
a的地址：0000001688BFFBD4    a的值：10
b的值：0000001688BFFBD4  指针b的解地址：10
指针b的地址：0000001688BFFBF8
//这样解释很明显了吧，
```