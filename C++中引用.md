# C++中引用

### 2.1语法：

```c
int & a = b;//变量b的别名是a
```



**注意事项：引用必须初始化，引用在初始化后，不可以改变**

```c++
int a = 10,c=20;
//int &b;错误，必须初始化
int &b = a
b = c //赋值操作，而不是更改引用，此时变量a的内容是20
```



### 2.2引用做函数参数

**作用：函数传参时，可以利用引用的技术让形参修饰实参**

```c++
//地址传递
void swap01(int *a,int *b)
{
	    int temp = *a;
    	*a = *b;
        *b = temp;
}

//引用传递
void swap02(int &a,int &b)
{
    int temp = a;
    int a = b;
    int b= temp;
}

int main()
{
    int a=10;
    int b=20;
    swap01(&a,&b);//地址传递，形参会修饰实参
    swap02(a,b);//引用传递，形参会修饰实参
}
```



### 2.3引用做函数的返回值

作用：引用是可以作为函数的返回值存在的

**注意：不要返回局部变量的引用**

用法：函数调用作为左值

```c++
int& test01() //返回引用
{
    int a = 10;//局部变量存放在栈中
    return a;
}

//函数调用可以作为左值
int& test02()
{
    static int a = 10;//静态变量存放在全局区中，由系统在程序结束后来释放全局区的变量
    return a;
}

int main()
{
    int &ref = test01();
    
    cout<<"ref = "<<ref <<endl;//第一次结果正确，是因为编译器做了保留
    cout<<"ref = "<<ref <<endl;//第二次结果错误，因为a的内存已经释放掉
    
   
    int &ref2 = test02(); 
    cout<<"ref2 = " <<ref2 <<endl;	//结果是10
    cout<<"ref2 = " <<ref2 <<endl;	//结果是10
    
    test02() = 1000; //相当于调用test02()函数后返回的静态变量a赋值为1000，即a = 1000; 

    cout<<"ref2 = " <<ref2 <<endl;	//结果是1000
    cout<<"ref2 = " <<ref2 <<endl;	//结果是1000
       
}
```



### 2.4 引用的本质

引用的本质：**引用的本质在c++内部实现是一个指针常量**

实例：

```c++
//发现是引用，转换为 int * const ref = &a;
void func(int& ref)
{
	ref = 100; //ref是引用，转换为*ref = 100;
}

int main()
{
    int a = 10;
    
    //自动转换为 int * const ref = &a;指针常量是指针指向不可以修改，这也说明了引用为什么不可以更改
    int& ref = a;
    ref = 20; //内部发现ref是引用,自动转换为 *ref = 20;
    
    cout<<"a:" <<a <<endl;
    cout<<"ref:" <<ref <<endl;
    
    func(a);
    return 0;
}
```



### 常量引用

作用：常量引用主要是用来修饰形参，防止误操作

在函数形参列表中，可以加const形式形参，防止形参改变实参

```c++

void showValue(const int& val)
{
    //函数体中无法修改val值
    cout<<"val = "<<val<<endl;
}

int main()
{
    int a = 10;
    //int & ref = 10; 出错，引用必须引用一块合法的内存空间
    const int & ref = 10;//加上const之后，编译器将代码修改 int temp = 10; int& ref = temp;
    //ref = 20 ; //出错，加入const之后变为只读，不可以修改
    
    int b = 100;
    showValue(b);
    
    return 0;
}
```

