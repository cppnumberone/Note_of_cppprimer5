# 变量

##  定义

基本形式：类型说明符紧跟**一个或者多**个变量名组成的列表，**变量名逗号隔开，分号结尾**；

表中每个变量名的类型都由最前面的类型说明符指定，定义时可赋初值；

```c++
int sum = 0，value, 
	unit_sold = 0;  // sum, value, unit_sold 都是int, sum, unti_sold 初值为0
Sale_item item;  //item 类型是Sales_item
```

[^对象]:有某种数据类型的内存空间，不严格区分是类还是内置类型，也不区分是否命名或是否只读；

## 初始化

用于初始化变量的值可以是任意复杂的表达式，一次定义多个变量，对象的名字随着定义就可以马上使用，即在同一条定义语句中可以用先定义的变量去初始化后定义的变量：

```C++
double price = 199.9, discount = price * 0.16;  // 可以用前一个变量初始化后一个
double salePrice = applyDiscount(price, discount);  // 可以是复杂的表达式
```

初始化语句

```C++
int units_sold = 0;
int units_sold = {0};
int units_sold{0};
int units_sold(0);
//正确，以上初始化方式都可以

long double ld = 3.1416926;
int a{ld}, b{ld};  //错误: 初始化列表的方式存在丢失信息的风险，会报错
int c(ld), d = ld;  //正确：确实丢失部分值 
```

默认初始化

定义时没有指定初值，变量会被赋予“默认值”：

> 内置类型的变量默认值由定义位置决定，定义于任何函数体之外的变量初始化为0；定义在函数体内的内置类型变量不会被初始化；
>
> 每个类格子决定初始化对象的方式；如：string类规定如果没有初值则生成一个空串

## 声明与定义

声明使得的名字为程序所知，一个文件想要使用别处定义的名字就必须包含对那个名字的声明；

定义负责创建和名字关联的实体；

**变量声明规定了变量的类型和名字，定义与之相同，除此之外，定义还申请的存储空间；**

如何声明变量：

```C++
extern int i;  //声明但不定义i，声明不申请内存
int j;  //声明并定义j
```

任何包含显示初始化的声明即为定义，给extern关键字标记的变量赋初值会抵消extern的作用，有初值就不再是声明；

**变量能且只能被定义一次，但能被多次声明；**



[^标识符]:C++标识符可以是字母、数字下划线，必须以字母或者下划线开头，大小写敏感；

> tips:
>
> 变量名一般小写：index;
>
> 用户自定义的类名一般大写字母开头：Sale_item;
>
> 多个字母要区分：student——loan or studentLoan

## 作用域

当第一次使用变量时再定义它；

允许内层作用域重新定义外层作用域已有的名字；

# 引用&指针

## 引用（左值引用）

引用为对象起了另一个名字，通过将声明符写成&d的形式定义引用类型：

```C++
int ival = 1024;
int &refVal = ival;  // refVal指向ival(是ival的另一个名字)
int &refVal;  // 错误：必须初始化

// 引用即别名，定义之后所有操作都是在绑定的对象上进行的
refVal = 2;  // 2赋给ival
int ii = refVal;  // 即 ii = ival
int &refVal3 = refVal;  //正确：将refVal3绑定到ival
int i = refVal;  //正确：即 int i = ival； 

// 允许在一条语句中定义多个引用，每个引用标识符都必须以&开头

int i = 1024, i2 = 2048;
int &r = i, r2 = i2;  //r 是引用，绑定i，r2是int
int i3 = 1024, &ri = i3;  //正确
int &r3 = i3, &r4 = i3;  //正确：r3和r4都是i3的引用

// 引用不能绑定字面值或者某个表达式的计算结果

int &refVal4 = 10;  //wrong
double dval = 3.14;
int &trgVal5 = dval;  //wrong: 此处引用初值必须是int
```

**1.引用初始化之后无法重新绑定另一个对象，因此必须初始化**

**2.引用本身不是一个对象，不能定义引用的引用**

## 指针

指针本身就是一个对象，允许对指针进行拷贝或者赋值，指针的对象可以改变，且无需在定义时赋初值，未被初始化的块作用域指针没有赋初值将会有一个不确定的值（全局作用域指向NULL？）；

定义指针类型的方法是将声明符写作 *d的形式，若在一条语句中定义了几个指针变量，则每个变量前面都必须有星号；

```C++
int *ip1, *ip2; //ip1, ip2都是指向int类型的指针
double dp, *dp2; //dp2是指向double类型对象的指针，dp是double类型对象
```

指针中存放某个对象的地址，想获得该地址要使用取地址符号&:

```C++
int ival = 42;
int *p = &ival;  //p存放变量ival的地址，即p是指向ival的指针 
```

**引用不是对象，不能定义指向引用的指针**

除了某些特殊情况，所有指针类型都要与它所指向的对象严格匹配：

```C++
double dval;
double *pd = &dval;  //正确:初始值是double型对象的地址
double *pd2 = pd;  //正确：初始值是指向double对象的指针

int *pi = pd;  //错误：类型不匹配
pi = &dval;  //错误：试图把double对象的地址赋给int型指针
```

> 指针的值应属于下列四种状态之一：
>
> 1. 指向一个对象；
> 2. 指向紧邻对象所占空间的下一个位置；
> 3. 空指针，意味着指针没有指向任何对象；
> 4. 无效指针，除上诉情况之外的值；

**拷贝无效指针会发生严重错误**

利用指针访问对象：

```C++
int ival = 42;
int *p = &ival;  //p存放着变量ival的地址，或者说p是指向变量ival的指针
cout << *p;  //由符号*得到指针p所指的对象，输出42

*p = 0;  //对指针解引用的结果赋值，就是给指针所指对象赋值
cout << *p;  //0
```

空指针不指向任何对象，生成空指针的方法：

```C++
int *p = nullptr;  //等价于 int *p = 0;
int *p2 = 0;
// 需要先 #include cstdlib
int *p3 = NULL;  //等价与 int *p3 = 0;

int zero = 0;
pi = zero;	//错误:不能直接把int变量赋值给指针，即便变量值为0
```

**最好使用nullptr，尽量避免使用NULL**

**赋值永远是改变等号左侧的对象**

指针拥有合法值时能用在条件表达式中：

```C++
int ival = 1024;
int *pi = nullptr;  //pi合法，是一个空指针
int *pi2 = &ival;  //pi2合法,存放ival地址

if(pi):  //pi值为0，条件为false
	//...
if(pi2):  //pi2指向ival,值不为0，条件为true
	//...
```

**任何非0指针对应的条件值都为true**

**同类型的指针可以用（==）以及（！=）进行比较，指向同一个地址时相等**

void*：

```C++
double obj = 3.14, *pd = &obj;  //right
//void*可以存放任何类型对象的地址
viod *pv = &obj;  //obj可以是任意类型的对象
pv = pd;  //pd可以是任意类型的指针
```

**不能直接操作void*指针所指向的对象（可以和别的指针比较，可以作为函数的输入或输出）**

一条定义语句可以定义出不同类型的变量：

```C++
int i = 1024, *p = &i, &k = i;
```

*仅仅修饰某个变量，并不作用与数据类型：

```C++
int* p1, p2;  //p1是个int指针，p2仅仅只是int变量
//尽量将修饰符与变量写在一起，看作复合类型
int *p1, *p2;
```

指向指针的指针：

```C++
int ival = 1024;
int *pi = &ival;  //pi指向一个int类型的数
int *ppi = &pi;  //ppi指向一个int类型的指针
```

![image-20220812151013631](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20220812151013631.png)

**通过二级指针访问原始数需要解引用两次**

指向指针的引用：

```C++
//不能定义指向引用的指针
int i = 42;
int *p;
int *&r = p;  //r是一个对指针p的引用

r = &i;  //令p指向i
*r = 0;  //等价于 *p = 0 & i = 0
```

从右往左读，离变量最近影响最直接：

即 *&p  先读引用  说明p是个引用

# const 限定符

const对象一旦创建其值就不能再改变，因此const对象必须初始化（like 引用），初始化同样可以是任意复杂的表达式：

```C++
const int i = get_size();  //right
const int j = 42;  //right
const int k;  //wrong:k没有初始化

//可以利用const对象去初始化另一个对象

int i = 42;
const int ci = i;
int j = ci;  //right
```

多个文件间const的使用：

```C++
//file_1.cc 定义并初始化一个常量，该常量能被其他文件访问
extern const int bufSize = fcn();
//file_1.h 头文件
extern const int bufSize;  //与file_1.cc中定义的bufSize是同一个
```

## 引用与const

```C++
const int ci = 1024;
const int &ri = ci;  //正确：引用及其对应的对象都是常量
ri = 42;  //错误:ri是一个对常量的引用
int &r2 = ci;  //错误:试图让一个非常量引用指向一个常量引用
```

在初始化常量引用时允许用任意表达式作为初始值，只要该表达式的结果能转换成引用的类型即可（**尤其，允许为一个常量引用绑定非常量的对象、字面值，甚至是表达式**）:

```C++
int i = 42;
const int &r1 = i;  //允许将const int& 绑定到一个普通的int对象上
const int &r2 = 42;  //正确：r1是一个常量引用
int &r5 = 42;  //错误
const int &r3 = r1 * 2;  //正确：r3是一个常量引用
int &r4 = r1 * 2;  //错误：r4是一个普通的非常量引用

// 特殊情况
double dval = 3.14;
const int &ri = dval;
//因为编译器将上诉代码理解为了：
double dval = 3.14;
const int temp = dval;  //临时整型常量
const int &ri = dval;

// 不能将指向常量的指针赋值给普通指针
int i = 0, *p = nullptr;
const int *cp = i;  // 正确
p = cp;  //错误，不能将const int *类型的值分配给int*
```

**对 const 的引用可能引用一个非 const 的对象**

常量引用对于引用的对象本身是不是常量没有作出限定，也就是说对象也可以是一个非常量：

```C++
int i = 42;
int &r1 = i;  //普通引用绑定i
const int &r2 = i;  //r2是常量引用，不允许通过r2修改i的值
r1 = 0;  //正确
r2 = 0;  //错误
```

## 指针与const

### 指向const对象的指针

要想存放常量对象的地址，只能使用指向常量的指针，**指向常量的指针不能用于改变所指对象的值**：

```C++
const double pi = 3.14;
double *ptr = &pi;  //错误：ptr是一个普通的指针
const double *cptr = &pi;  //正确：cptr 可以指向一个 双精度常量
*cptr = 42;  //错误：不能给*cptr赋值

//允许一个指向常量的指针指向一个非常量对象
double dval = 3.14;
cptr = &dval;
```

### const 指针

**指针是对象，而引用不是，因此允许把指针本身定义为常量，常量指针必须初始化，初始化后的值不能再改变。**

把*放在const之前说明指针本身是一个常量，即不变的是指针本身而不是指向的值：

```C++
int errNumb = 0;
int *const curErr = &errNumb;  //curErr是一个常量指针,将一直指向errNumb
const double pi = 3.14159;
const double *const pip = &pi;  //pip是一个指向常量的常量指针，将一直指向pi，并且无法通过pip改变pi的值

*pip = 2.72;  //错误： pip是一个指向常量的指针

if(*curErr){
    errorHandler();
    *curErr = 1;  //正确:能够通过curErr改变errNumb的值
}

```

指针本身是一个常量（即*const）并不代表不能通过指针修改指向对象的值

[^顶层const与底层const]:**用顶层const表示指针本身是个常量，底层const表示指针所指的对象是一个常量**。更一般的，顶层const可以表示任意的对象是常量，底层const则与指针以及引用等复合类型的基本类型部分有关



## 常量表达式与 constexpr

常量表达式（const expression）是指值不会改变并且在编译过程就能得到计算结果的表达式:

```C++
const int max_files = 20;  //max_files是常量表达式
const int limit = max_files + 1;  //limit是常量表达式
int staff_size = 27;  //staff_size的值是个普通int，可以改变，不是常量表达式
const int sz = get_size();  //sz的值要在运行时才能得到，不是常量表达式
```

允许将变量声明为constexpr类型以便由编译器来验证变量的值是否是常量表达式，**声明为constexpr的变量一定是一个常量**，而且必须用常量表达式初始化：

```C++
constexpr int mf = 20;
constexpr int limit = mf + 1;
constexpr int sz = size();  //只有当size是一个constexpr函数时这条语句才正确(足够简单到编译就能得到结果)
```

当在constexpr声明中定义了一个指针，限定符constexpr仅对指针有效，与指针所指对象无关：

```C++
const int *p = nullptr;  //p是一个指向整型常量的指针
constexpr int *q = nullptr;  //q是一个指向整数的常量指针，即能通过q更改所指对象的值
```

# 处理类型

## 类型别名

有两种方式可以定义类型别名：

```C++
typedef double wage;  //wage是double的同义词
typedef wage base, *p;  //base是double的同义词，p是doulbe*的同义词
//含有typedef的声明语句定义的不再是变量而是类型别名
```

新标准规定（使用**别名声明**来定义类型的别名）：

```C++
using SI = Sales_item;  //SI是Sales_item的同义词
```

using作为关键字，作用是把等号左侧的名字规定成等号右侧类型的别名

类型别名与类型的名字等价，只要是类型的名字能出现的地方，就能使用类型别名：

```C++
wage jourly, weekly;  //等价于 double hourly,weekly；
SI item； //等价于Sales_item item 
```

**指针、常量与类型别名**

如果某个类型别名指代的是复合类型或者常量，会产生意想不到的后果：

```C++
typedef char *pstring;
const pstring cstr = 0;  //cstr是指向char的常量指针
const pstring *ps; //ps是一个指向char的常量指针
```

## auto 类型说明符

编程中常常在声明某个变量时就需要将对应表达式的值用于初始化这个变量，然而要知道表达式值的类型往往很难做到，auto用于解决这个问题，让**编译器代替我们去分析表达式所属类型**，auto通过初始值来推断变量的类型，因此**auto定义的变量必须有初始值**：

```c++
auto item = val1 + val2;  //item初始化为val1和val2相加的结果

// auto也能在一条语句中声明多个变量，但是该语句中所有变量的初始基本数据类型必须一样
auto i = 0, *p = &i; //正确：i是整数,p是整形指针
auto sz = 0, pi = 3.14;  //错误：sz和pi的类型不一样

//auto推断出的类型有时会和初始的类型并不完全一样，编译器会适当地改变结果类型使其更符合初始化规则
int i = 0, &r = i;
auto a = r;  // a是一个整数（r是i的别名，而i是一个整数）

//auto 一般会忽略顶层const，保留底层const
const int ci = i, &cr = ci;
auto b = ci;  //b是一个整数（顶层const被忽略了）
auto c = cr;  //c是一个整数
auto d = &i;  //d是一个整型指针
auto e = &ci; //e是一个指向整数常量的指针，即 const int *e = &ci;（对常量对象取地址是一种底层const）

//如果希望auto类型是顶层const：
const auto f = ci;  //ci的推演类型是int，f是const int

//可以将引用的类型设为auto
auto &g = ci;  //g是一个整型常量引用
auto &h = 42;  //错误：不能为非常量引用绑定字面值
const auto &j = 42;  //正确：可以为常量引用绑定字面值

auto k = ci, &l = i;  //k是整数，l是整型引用
auto &m = ci, *p = &ci;  //m是对整型常量的引用，p是指向整型常量的指针
auto &n = i, *p2 = &ci;  //错误：i的类型是int而&ci的类型是const int
```

## decltype 类型指示符

从表达式中推断要定义的变量类型，但不想用表达式的值初始化该变量；

decltype的作用是选择并返回操作数的数据类型；

```c++
decltype(f()) sum = x;  //sum的类型就是函数f的返回类型
// 编译器并不实际调用函数f，而是使用当调用发生时f的返回类型作为sum的类型，即sum的类型就是假如f调用会返回的那个类型
```

decltype 处理顶层const和引用的方式与auto不同：

```C++
const int ci = 0, &cj = ci;  //x的类型是const int
decltype(ci) x = 0;  //x的类型是const int
decltype(cj) y = x;  //y的类型是const int &，y绑定到变量x
decltype(cj) z;  //错误：z是一个引用，必须初始化

//如果decltype使用的表达式不是一个变量，则decltype返回表达式结果对应的类型
//decltype的结果可以是引用类型
int i = 42, *p = &i, &r = i;
decltype(r + 0) b;  //正确：加法的结果是int，因此b是一个（未初始化的）int
decltype（*p） c;  //错误：c是int&,必须初始化

//decltype的表达式如果是加上了括号的变量，结果将是引用
decltype((i)) d;  //错误:d是int&,必须初始化
decltype(i) e;  //正确：e是一个未被初始化的int
```

如果decltype中的表达式内容是解引用操作，则decltype将得到引用类型。即：**decltype((variable))（双层括号）的结果永远是引用，而decltype(variable)结果只有当variable本身就是一种引用时才是引用。**



## 自定义数据结构

C++允许以类的形式自定义数据类型

```C++
struct Sales_data{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
}；
//类内部定义的名字必须唯一，但可以与类外定义的名字重复
//类体右侧表结束的花括号必须写一个分号，因为类体后面可以紧跟变量名以示对该类型对象的定义：

struct Sales_data{/*...*/}accum, trans, *salesptr;

//与上一条等价,maybe更好一些
struct Sales_data{/*...*/};
Sales_data accum, trans, *salesptr;

//使用Sales_data类例子
//程序的输入是两条交易记录：
//0-201-78345-x 3 20.00
//0-201-78345-x 2 25.00
//实现两次交易相加

int main(){
    Sales_data data1, data2;
    //读入数据
    double price = 0;
    cin >> data1.bookNo >> data1.units_sold >> price;
    data1.revenue = price * data1.units_sold;
    cin >> data2.bookNo >> data2.units_sold >> price;
    data2.revenue = price * data2.units_sold;
    //检查ISBN是否相同
    if(data1.bookNo == data2.bookNo){
    //若相同则求总和
        unsigned totalCnt = data1.units_sold + data2.units_sold;
        unsigned totalRevenue = data1.revenue + data2.renvenue;
        cout << data1.bookNO << " " << totalCnt << " " << totalRevenue << " ";
        if(totalCnt != 0){
            cout << totalRevenue/totalCnt << endl;  //平均售价
        }else{
            cout << "(no. sales)" << endl;
        }
    }else{
        cerr << "data must refer to the same ISBN" << endl;
        return -1;
    }
}
```

**最好不要把对象的定义和类的定义放在一起**

每个对象都有自己的一份数据成员拷贝，修改一个对象的数据成员，不会影响到其他Sales_data的对象；

可以为数据成员提供一个**类内初始值**，创建对象时，类内初始值用于初始化数据成员，没有初始值的成员将被默认初始化（bookNo将被初始化为空字符串）；

类一般不定义在函数体内，通常被定义在头文件中，类所在的头文件名字应该与类名相同(eg: 应该把Sales_data类定义在名为Sales_data.h的头文件中)

**头文件一旦改变，相关的源文件必须重新编译以获取更新过的声明**

## 预处理器

功能1：遇到#include会用指定头文件代替

功能2：头文件保护符

```C++
#ifndef SALES_DATA_H
#define SALES_DATA_H
#includee<string>
struct Sales_data{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
#endif
```

整个程序中的预处理变量包含头文件保护符必须唯一，通常的做法是基于头文件中类的名字来构建保护符的名字，以确保唯一性。**一般把预处理变量的名字全部大写**。

# 命名空间using

作用域操作符（::）的作用：编译器应从操作符左侧名字所示的作用域中寻找右侧名字。

有了using声明之后就无无须前缀(::)

```C++
using namespace::name;

// 一旦声明,就可以直接访问命名空间中的名字
#include<iostream>
using std::cin;

int main(){
    int i;
    cin >> i;
    cout << i;  //错误：没有对应的using声明，必须使用完整的名字
    std::cout << i;
    return 0;
}

//全部包含
using namespace std;
```

**头文件不应包含using声明**

# 标准库类型string

标准库类型string表示可变长的字符序列。

```C++
#include<string>
using namespace std::string
```



## 定义和初始化string对象

```C++
//初始化string对象常用方式
string s1;  //默认初始化，s1是一个空字符串
string s2 = s1;  //s2是s1的一个副本，等价于 string s2(s1);
string s3 = "hiya";  //s3是该字符串字面值的副本, 等价于 string s3("value")；
string s4(10, 'c');  //s4的内容是cccccccccccccccccc
```

**string副本拷贝不包含最后的空字符串**

如果使用等号(=)初始化一个变量，实际上执行的是**拷贝初始化**，编译器把等号右侧的初始值拷贝到新创建的对象中去。如果不使用等号，则执行的是**直接初始化**。

```C++
// 对于多个值进行初始化非要用拷贝初始化(毫无优势)
string s8 = string(10, 'c');  //显示的创建一个临时对象
// 等价于:
string temp(10, 'c');
string s8 = temp;
```

## string 对象上的操作

<,<=,>,>=  都是利用字符在字典中的顺序进行比较，且对字母的大小写敏感

```C++
// 可以使用IO操作符读写string对象
int main(){
    string s;
    cin >> s;  //将string对象读入s，遇到空白停止，string对象会自动忽略开头的空白，并从第一个真正的字符读起，比如输入“    hello world!” 则输出的将是“hello”
    cout << s << endl;
    return 0;
}


// 读取未知数量的string对象
int main(){
    string word;
    while(cin >> word)
        cout << word << endl;  //逐个输出单词，每个单词后面都紧跟一个换行
    return 0;
}


// 使用getline()读取一整行
// 能够保留输出时的空白符
// getline()的参数是一个输入流和一个string对象，函数从给定的输入流中读入内容，直到遇到换行符为止（会读入换行符），然后把所读内容存入string对象中（不存换行符）
int main(){
    string line;
    //每次读入一整行，直至到达文件末尾
    while(getline(cin, line))
        cout << line << endl;  //line中不包含换行符
    return 0;
}


// string中的empty和size操作
// 改写做到只输出非空string行
while(getline(cin, line))
    if(!line.empty())
        cout << line << endl;
// size函数控制输出条件
while(getline(cin, line))
    if(line.size() > 70)
        cout << line << endl;
// string.size() 返回的是string::size_type 不同于int或者unsigned，是一个无符号整数


// 比较string对象
// string比较特点：
//    1.如果两个string长度不同，且较短string的每个字符都与较长string对应位置相同，则短string小于长string
//    2.如果两个string对象在某些对象的位置不一致，则string对象比较的结果其实是string中第一对相异字符比较的结果

string str = "Hello";
string phrase = "Hello World";
string slang = "Hiya";
// str < phrase < slang


// 为string对象赋值
string st1(10, 'c'), st2;
st1 = st2;  //用st2的副本替换st1的内容， 即st1,st2都是空字符串


//string对象相加
string s1 = "hello, ", s2 = "world\n";
string s3 = s1 + s2;  //s3的内容： hello, world\n
s1 += s2;  // 等价于 s1 = s1 + s2

//字面值与string相加
string s1 = "hello", s2 = "world";
string s3 = s1 + ", " + s2 "\n";
string s7 = "hello" + ", " + "world";  //错误，不能将字面值直接相加
```

**切记：字符串字面值和string是不同的类型**

C++标准库兼容C的标准库，C的头文件形如name.h在C++中命名为cname

**范围for语句**，这种语句遍历给定序列中的每个元素并对序列的每个值执行某种操作，其语法形式为：

```C++
for(declaration : expression)
    statement

//每次迭代，declaration部分的变量会被初始化为expression部分的下一个元素值
//把string对象中的字符每行一个输出出来：
string str("some string");
// 每行输出str中的一个字符
for(auto c : str)  //对于str中的每个字符
    cout << c << endl;  //输出当前字符，后面紧跟一个换行


//统计字符串中标点符号的数量
string s("Hello World!!!");
decltype(s.size()) punct_cnt = 0;
for(auto c : s){
    if(ispunct(c))
        punct_cnt++;
}
cout << punct_cnt << "adasdsswa" << in << s << endl;
```

**如果想要改变string对象中字符的值，必须把循环变量定义成引用类型**

```C++
string s("Hello World!!!");
//改为大写
for(auto &c : s)
    c = toupper(c);
cout << s << endl;
```

**string下标运算符接受的输入参数是string::size_type类型的值**，该参数表示要访问的字符的位置，**返回值是该位置上字符的引用**，string下标从0计数，下标必须大于等于0且小于s.size()

```C++
if(!s.isempty())
    cout << s[0] << endl;  //输出第一个字符

//另一种改为大写的形式
for(decltype(s.size()) index = 0; index != s.size()  && !isspace(s[index]); ++index)
    s[index] = toupper(s[index]);

//依照输入的十进制输出对应的十六进制（简单版，范围为0-16）

const string hexdigits = "0123456789ABCDEF";
cout << "0-16"
    << "separated by space.hit enter when finish:"
    <<endl;
string result;
string::size_type n;
while(cin >> n){
    if(n < hexdigits.size())
        result += hexdigits[n];
    else{
        cout << "range error!" << endl;
    }
}
cout << result << endl;
```

## 标准库类型vector

表示对象的集合，其中所有对象的类型都相同，每个对象都有一个与之对应的索引，索引用于访问对象。vector被称作**容器**，要使用vector，必须包含适当头文件：

```C++
#include<vector>
using std::vector;
```

使用vector模板：

```C++
vector<int> ivec;  //ivec保存int类型的对象
vector<Sales_item> Sales_vec;  //保存Sales_item类型的对象
vector<vector<string>> file;  //该向量的元素是vector对象
```

> vector是模板而非类型，由vector生成的类型必须包含vector中元素的类型，例如vector<int>

**不存在包含引用 的vector，因为引用不是对象** 

定义和初始化vector对象：

```C++
vector<T> v1;  //默认初始化v1，v1是一个空的vector,类型为T
vector<T> v2(v1);  //v2包含v1所有元素的副本， T的类型必须一致
vector<T> v2 = v1; //同上式等价
vector<T> v3(n, val);  //n个重复元素，每个元素都是val
vector<T> v4(n);  //v4包含n个重复执行了值初始化的对象
vector<T> v5{a, b, c, ...};  //手动初始化
vector<T> v5 = {a, b, c, ...};  //等价于上式
```

如果vector对象的元素是内置类型，比如int，则元素初始值自动设为0，如果元素是某种类的类型，比如string，则元素会类默认初始化：

```C++
vector<int> ivec(10);  //10个元素，每个都初始化为0
vectir<string> svec(10);  //10个元素，每个都是空string对象
```

如果vector对象中的元素类型不支持默认初始化，我们就必须提供初始的元素值

向vector对象中添加元素：

```C++
vector<int> v2;  
for(int i=0; i != 100; ++i)  //输入0-99总共100个整数
    v2.push_back(i);  //依次把整数值放到v2尾端

//从标准输入中读取单词作为vector的元素
string word;
vector<string> text;
while(cin >> word)
    text.push_back(word);
```

**循环体内部包含有向vector对象添加元素的语句时则不能使用范围for**

几种较为重要的vector操作：

```C++
vector<T> v;
v.empty();  //如果v不含有任何元素，返回真；
v.size();  //返回v中元素的个数
v.push_back(t);  //向v的尾端添加一个值为t的元素
v[n];  //返回v中第n个位置上元素的引用
v1 = v2;  //用v2中的元素拷贝替换v1中的元素
v1 = {a,b,c...};  //用列表中元素的拷贝替换v1中的元素
v1 == v2;  //v1和v2相等当且仅当他们的元素数量相同且对应位置的元素值都相同
v1 != v2;  
<, >, <=, >=;  //字典顺序比较
```

使用范围for语句处理vector对象(like string)：

```C++
vector<int> v{1,2,3,4,5,6,7,8,9};
for(auto &i : v)
    i *= i;
for(auto i : v)
    cout << i << ' ';
cout << endl;
```

要使用size_type，需首先指定它由那种类型定义：

```c++
vector<int>::size_type  //正确
vector::size_type  //错误
```

**两个整数相除还是整数，余数被自动忽略**

可以通过计算得到vector的索引值：

```C++
//以10分为一个分数段统计成绩的数量：
vector<unsigned> scores(11, 0);
unsigned grade;
while( cin >> grade){
    if(grade <= 100)
        ++score[grade/10];
}
```

## 迭代器

**类似于指针类型，迭代器提供了对对象的间接访问，就迭代器而言，其对象时容器中的元素或string对象中的字符。**

**和指针不同之处在于，获取迭代器不是使用取地址符，有迭代器的类型同时拥有返回迭代器的成员**

比如，这些类型都拥有名为begin和end的成员，其中begin成员负责返回第指向第一个元素（或第一个字符）的迭代器：

```c++
//b表示v的第一个元素，e表示v尾元素的下一个位置
auto b = v.begin(), e = v.end();
```

end元素则负责返回指向容器（或者string对象）"尾元素的下一个位置"的迭代器，通常被称为**尾后迭代器**，仅是个标记，表示已经处理完了所有的元素

标准迭代器的运算：

```C++
*iter;  //返回迭代器iter所指元素的引用
iter->mem;  //解引用iter并获取该元素的名为mem的成员,等价于(*iter).mem
++iter;
--iter:  //令迭代器指向下/上一个元素
iter1 == iter2;  
iter1 != iter2;
```

**试图解引用一个非法迭代器或者尾后迭代器都是未被定义的行为**

```C++
//改string的第一个字符为大写，迭代器版本
string s("some thing");
if(s.begin() != s.end()){
    auto it = s.begin();
    *it = toupper(*it); 
}


//移动迭代器
for(auto it = s.begin(); it != s.end() && !isspace(*it); ++it)
    *it = toupper(*it);
```

**因为end返回的迭代器并不实际指示某个元素，所以不能对其进行递增或解引用的操作**

新函数cbegin()和cend()返回值都是const_iterator类型，即只能读不能改

解引用迭代器可获得迭代器所指的对象，如果该对象的类型恰好是类，就有可能进一步访问它的成员。

```C++
//检查字符串vector的元素是否为空
auto it = vector.begin();
(*it).empty();
```

**箭头运算符（->)**把解引用和成员访问两种操作结合起来，也就是说，it->mem和（*it）.mem表达的意思相同

```C++
//依次输出text的每一行直至遇到第一个空白行为止
for(auto it = text.cbegin(); it != text.end() && !it->empty(); ++it)
    cout << *it << endl; 
```

迭代器操作vector对象的限制：

> 1.不能在范围for循环中向vector对象添加元素;
>
> 2.任何一种可能改变vector对象容量的操作，比如push_back，都会使该vector对象的迭代器失效；

**谨记，但凡使用了迭代器的循环体，都不要向迭代器所属的容器添加元素**

迭代器运算：

```C++
iter + n;  //迭代器加上一个整数值仍是迭代器，新位置与原来相比前移若干元素
iter - n;  //
iterl += n; //迭代器加法的复合赋值
iterl-= n;
iter1 - iter2;  //两个迭代器相减结果是他们之间的距离。参与运算的两个迭代器必须指向同一个容器的元素或者尾元素的下一个位置
<, >, <=, >=;  //参与运算的两个迭代器必须指向同一个容器的元素或者尾元素的下一个位置

//一些例子
//得到vi中间元素的一个迭代器
auto mid = vi.begin() + vi.size() / 2;
//处理前半部分元素
if(it < mid){
    /*...*/
}
```

使用迭代器完成二分搜索：

```C++
//假定text是个有序表
// beg 和 end 表示搜索范围
//sought为要找到的值
auto beg = text.begin();
auto end = text.end();
auto mid = text.begin() + text.size() / 2;
while(mid != end && *mid != sought){
    if(sought < *mid){
        end = mid;
    }
    else{
        beg = mid + 1;
    }
    mid = beg + (end - beg) / 2;
}
```

## 数组

与vector数组的大小确定不变，不能随意的向数组中增加元素。因为数组的大小固定，在损失一定灵活性的同时，对某些特殊的应用运行性能较好。

**在不清楚元素个数的情况下，请使用vector**

数组的初始化：

```C++
unsigned cnt = 42;  //常量表达式
constexpr = unsigned sz = 42;  //常量表达式
int arr[10];  //10个整数
int *parr[42];  //42个整型指针
string bad[cnt];  //错误:cnt不是常量表达式
string strs[get_size()];  //仅当get_size()是constexpr时正确
```

**默认情况下，数组的元素会被默认初始化**

定义数组的时候必须指定类型，不允许使用auto，不存在引用的数组

数组使用列表初始化时，可以忽略维度，如果指明了维度，则初始化的的总数量不应超过指定的大小，如果维度大于提供的初始值数量，则提供的初始值初始化靠前的元素，剩下的元素默认初始化

```C++
string a4[3] = {"hi", "bye"}; //等价于 a4[] = {"hi", "bye", ""}
int a5[2] = {0,1,2}  //wrong
```

字符数组的特殊性：

```C++
char a3[] = "C++";  //自动添加表示字符串结束的空字符
const chaar a4[6] = "Daniel"  //错误:空间不足，需要存放空字符串
```

数组不允许拷贝和赋值

```C++
int a[] = {0, 1, 2};  //含有3个整数的数组
int a2[] = a;  //错误：不允许使用一个数组初始化另一个数组
a2 = a;  //错误：不能把一个数组直接赋值给另一个数组
```

虽然有些编译器支持某些标准特性之外的操作(**即所谓的编译器扩展**)，比如说支持数组的赋值，但一般来说，**最好避免使用非标准特性，因为含有非标准特性的程序很可能在其他编译器上无法正常工作**。

可以定义一个存放指针的数组，可以定义数组的指针及数组的引用：

```C++
int *ptrs[10];  //含十个整型指针的数组
int &refs[10] = /* ?*/;
int (*Parray)[10] = &arr;  //Parray指向一个含有10个整数的数组
int (&arrRef)[10] = arr;  //arrRef引用一个含有10个整数的数组
```

数组元素也能使用范围 for或下标运算符来访问：

```C++
//以10分为一个分数段统计成绩的数量(数组实现)：
unsigned scores[11] = {};
unsigned grade;
while(cin >> grade){
    if(grade <= 100)
        ++scores[grade/10];
}
```

与vector和string一样，当需要遍历数组的所有元素时，最好的办法也是使用范围for语句：

```C++
for(auto i : scores)
    cout << i << " ";
cout << endl;
```

使用数组的时候，编译器一般会把它转化为指针，对数组的元素使用取地址符就能得到指向该元素的指针：

```C++
string nums[] = {"one", "two", "three"};  //数组的元素是string对象
string *p = &nums[0];  //p指向nums的第一个元素

//在很多用到数组名字的地方，编译器都会自动地将其替换为一个指向数组首元素的指针
string *p2 = nums;  //等价于*p2 = &nums[0]
int ia[10];
auto ia2(ia);  //ia2是一个整形指针，指向ia的第一个元素
//编译器执行上式时类似： auto ia2(&ia[0])
ia2 = 42;  //错误，ia2是一个指针

//当使用decltype时不会发生上述转换
decltype(ia) ia3 = {0,1,2,...};
ia3 = p;  //错误，ia3时一个数组
ia3[4] = i;  //正确
```

指向数组元素的指针拥有更多的功能，vector和string的迭代器支持的功运算，数组的指针全都支持。

```C++
int arr[] = {0,1,2,3,4,5,6,7,8,9};
int *p = arr;  //p指向arr的第一个元素
++p;  //p指向arr[1]
int *e = &arr[10];  //指向arr尾元素的下一位置的指针（获取尾后地址）
```

C++11引进了标准库中的begin和end函数，和容器中的两个同名成员功能类似，但这两个函数并不是成员函数（因为数组不是类类型），所以正确使用形式应该是将数组作为参数：

```C++
int ia[] = {0,1,2,3,4,5,6,7,8,9};
int *beg = begin(ia);
int *end = end(ia);
```

指向数组的指针运算：

```C++
constexpr size_t sz = 5;
int arr[sz] = {1,2,3,4,5};
int *ip = arr;
int *ip2 = ip + 4;  //ip2指向arr的尾元素arr[4]
int *p = arr + sz;  //正确：指向尾后；但使用警告：不要解引用
int *p2 = arr + 10; //错误：p2值未定义，arr只有5个元素

//和迭代器一样，两个指针相减得到的是他们之间的距离，同样，两个指针必须指向同一个数组当中的元素
auto n = end(arr) - begin(arr);  //arr中元素的数量
```

只要两个指针指向同一个数组的元素，就能利用关系运算符对其进行比较：

```C++
int *b = arr, *e = arr + sz;
whil(b < e){
    //use b 
    b ++;
}
```

**如果两个指针分别指向两个不相关的对象，则不能比较他们**

指针加上一个整数所得的结果还是一个指针，假设结果指针指向了一个元素，则允许解引用该结果指针：

```C++
int ia[] = {0,2,4,6,8};
int last =  *(ia + 4);  //正确的， last = 8
```

只要指针指向的是数组中的元素(或者数组中尾元素的下一个位置)，都可以执行下标运算：

```C++
int *p = &ia[2];
int j = p[1];  //p[1]等价于*(p + 1),即ia[3]
int k = p[-2];  //p[-2]即ia[0]
```

允许使用数组来初始化vector对象，只需指明要拷贝区域的首元素地址和尾后地址就可以：

```C++
int int_arr[] = {0,1,2,3,4,5};
vector<int> ivec(begin(int_arr), end(int_arr));
//用于初始化vector对象的值也可能仅是数组的一部分
vector<int> subVec(int_arr + 1, int_arr + 4 )  //拷贝[1]\[2]\[3]三个元素
```

多维数组的初始化：

```C++
int ia[3][4] = {
    {0,1,2,3},
    {4,5,6,7},
    {8,9,10,11}
}  //等价于： int ia[3][4]={1,2,3,4,5,6,7,8,9,10,11};

//显式地初始化每行的首元素
int ia[3][4] = {{0}, {4}, {8}};
//显式地初始化首行
int ia[3][4] = {0, 1, 2, 3};
ia[2][3] = arr[0][0][0];  //用arr首元素给ia[2][3]赋值
int (&row)[4] = ia[1];  //row是ia第二行的引用

//使用范围for语句语句处理多维数组
size_t cnt = 0;
for(auto &row : ia)
    for(auto &col : row){
        col = cnt;
        ++cnt;
    }
    
```

**要使用范围for语句处理多维数组，除了最内层的循环外，其它所有的循环变量都应该是引用类型**

指向多维数组的指针

```C++
int ia[3][4];
int (*p)[4] = ia;  //等价于 int (*p)[4] = &ia[0];
p = &ia[2];

//使用auto和decltype就能避免数组前面加指针
//输出ia中每个元素的值
for(auto p = ia; p != ia + 3; ++p){
    for(auto q = *p; q != *p + 4; ++q)
        cout << *q << ' ';
    cout << endl;
}

//使用标准库的begin 和 end 实现同样功能
for(auto p = begin(ia); p != end(ia); ++p){
    for(auto q = begin(*p); p != end(*p); ++q){
		cout << *q << ' ';
    cout << endl;
    }
}

//类型别名简化多维数组的指针
using int_array = int[4];
typedef int int_array[4];
//上面两行等价
for(int_array *p = ia; p != ia + 3; ++p){
    for(int *q = *p; q != *p + 4; ++q)
        cout<< *q << ' ';
    cout << endl;
}
```

# 表达式

表达式由一个或多个**运算对象**组成，对表达式求值将得到一个**结果**，字面值和变量是最简单的**表达式**，其结果就是字面值和变量的值，把一个**运算符**和一个或多个运算对象组合起来可以生成较复杂的表达式。

作用于一个对象的是**一元运算符**(*,&)，作用于两个运算对象的运算符是**二元运算符**(+,==)，函数调用是一种特殊的运算符，它对运算对象的数量没有限制，一些符号既能作为一元运算符，也能作为二元运算符。

当运算符作用于类类型的运算对象时，用户可以自行定义其含义。这种自定义的过程称为**重载运算符**，当使用重载运算符时，**我们可以定义运算对象的类型和返回值的类型，但运算对象的个数、运算符的优先级以及结合律都是无法改变的**。

当一个对象被用作右值时，用的是对象的值（内容），当对象被用作左值时，用的是对象的身份（在内存中的位置）。

```C++
//一元正负号对运算对象取值后，会返回提升后的副本
int i = 1024;
int k = -i;  // k = -1024
bool b = true;
bool b2 = -b;  // b2仍然是true,b先转换为int，值为1，再取负，-1，不等于0
```

**整数相除(/)直接丢掉小数部分；取余(%)运算参与的对象必须是整数类型**

取余的结果如果不为0，则余数符号与被除数相同，整数相除的符号：相同为正，不同为负

关系运算符满足左结合律，将几个关系运算符连写在一起会由意想不到的结果：

```C++
if(i < j < k);   //先计算i<j，之后将i<j的结果跟k比较，k大于1就为真
//正确表达
if(i < j && j < k);

//bool与int的比较
if(val == true); //只有当val等于1时才为真
//上式等价于
if(val == 1);
```

**赋值运算符满足右结合律**

```C++
int i = 0,j;
j = ++i;  //j = 1, i = 1;  前置版本得到递增之后的值
j = i++;  //j = 1, i = 2;  后置版本得到递增之前的值
```

除非必须，否则不用递增递减运算符的后置版本

后置版本的应用：

```C++
auto pbeg = v.begin();
while(pbeg != v.end() && *pbeg >= 0)
    cout << *pbeg++ << endl;
```

sizeof运算符的两种表达形式及部分例子：

```C++
//表达形式
sizeof(type);
sizeof expr;

//示例
Sale_data data, *p;
sizeof(Sale_data);  //存储Sale_data类型对象所占的空间大小
sizeof data;  //data的类型大小，同上
sizeof p;  //指针本身所占空间大小
sizeof *p;  //所指对象所占空间大小
sizeof data.revenue;  //revenue成员对应类型的大小
sizeof Sale_data::revenue;  //上式的另一种表达
```

利用sizeof求数组元素个数：

```C++
constexpr size_t sz = sizeof(ia)/sizeof(*ia);
int arr2[sz];  //即返回一个常量表达式
```

逗号运算符含有两个运算对象，从左到右依次求值，首先对左侧求值，之后将求值结果丢弃，真正结果是右侧表达式的值。

常用于for循环：

```C++
vector<int>::size_type cnt = ivec.size();
// 将把从size到1的值赋给ivec的元素
for(vector<int>::size_type ix = 0; ix != ivec.size(); ++ix, --cnt)
    ivec[ix] = cnt;
```

**如果两种类型可以互相转换，那么他们之间就是关联的**

```C++
int ival = 3.514 + 3;  //隐式转换，int会转为double型进行相加，之后执行初始化double被转为int，小数丢掉
```

**如果算术表达式中的一个运算对象是无符号类型，另一个是带符号类型，且无符号类型不小于带符号类型，则带符号类型会转为无符号**

强制类型转换：

（本质上是非常危险的）

一个命名的强制类型转换具有如下形式：

```C++
cast-name<type>(expression);
```

其中type是转换的目标类型而expression是要转换的值。如果type是引用类型，则结果是左值。cast-name 是 static_cast、dynamic_cast、const_cast、reinterpret_cast中的一种。

```C++
//不包含底层const时可以用static_cast
double slope = static_cast<double>(j) / i;

//找回viod*指针的值
viod *p = &d;
double *dp = static_cast<double*>(p);

//const_cast只能改变对象的底层const
const char *pc;
//将常量对象转为非常量
char *p = const_cast<char*>(pc);
```

**const_cast不能用于改变表达式的类型，仅能改变对象的const**

使用reinterpret_cast非常危险，类型会改变，并且编译器不会给出警告

# 语句

## 简单语句

最简单的语句就是空语句，仅含有一个单独的分号：

```C++
;
```

用在for 或者 while：

```C++
while(cin >> s && s != sought)
    ;
```

表达式语句：

```C++
ival + 5;
int i = ival;  
```

第一行表达式完全无用，求值结果会被丢掉；

## 语句作用域

可以在块结构内定义变量，定义在块中的变量仅内部可见，一旦语句结束，变量就超出作用范围

## 条件语句

if语句：

```c++
if(condition)
    satatement1;
else 
    statement2;
```

悬垂else：

```C++
if(grade % 10 >= 3)
    if(grade % 10 > 7)
        lettergrade += '+';
else
    leettergrade += '-';
```

这里的else实际上跟内部的if匹配（else总是选最近的if匹配，并不是缩进），加上花括号就可以了；

swich语句示例：

```C++
//统计输入中aei的数量
unsigned aCnt = 0, eCnt = 0, iCnt = 0;
char ch;
while(cin >> ch){
    swich(ch){
        case 'a':
        	++aCnt;
        	break;
        case 'e':
        	++eCnt;
        	break;
        case 'i':
        	++iCnt;
        	break;
    }
}
```

## 迭代语句

### while

```C++
while(condition)
    statement;
```

只要condition的求值结果为真就一直执行，condition不能为空，如果condition第一次求值为false，则一次都不执行statement.

### 传统for

```C++
for(init-statement; condition; expression)
    statement;
```

init-statement负责初始化一个值，只要condition的求值结果为真就一直执行，condition不能为空，如果condition第一次求值为false，则一次都不执行statement。expression负责修改初始化的变量。

init-statement可以定义多个对象，但只能有一条声明语句，即所有变量的基础类型必须完全相同。

```C++
for(decltype(v.size()) i = 0, sz = v.size(); i != sz; ++i)
    v.push_back(v[i]);                         
```

for()循环中三个部分都可以用空语句代替（省略）。

### 范围for

可以用于遍历容器或者其他序列的所有元素。

```C++
for(declaration : expression)
    statement;
```

expression 表示的必须是一个序列，比如用花括号括起来的初值列表、数组、vector或者string等。

declaration定义一个变量，序列中的每个元素都能转换成该变量的类型。确保类型相容最简单的办法就是使用auto。

```C++
vector<int> v = {0,1,2,3,4,5,6,7,8,9};
// 范围变量必须是引用类型，这样才能执行写操作
for(auto &r : v)
    r *= 2;
```

### do while

与while类似，不同之处在于会在求condition之前首先执行一次statement。

### 跳转

#### break

负责终止**离他最近**的while、do while、for、和switch语句，并从这些语句之后的第一条语句开始执行。

break只能出现在迭代和switch语句中。

#### continue

continue终止最近的循环中的当前迭代并立即开始下一次迭代。

#### goto

无条件从goto语句跳转到另一函数内的另一条语句。

*不要在程序中使用goto，难懂又难修改*

```C++
goto label;
```

label是用于标识一条语句的标识符，带标签的语句是特殊语句，在他之前会有一个标识符以及一个冒号：

```C++
label: return;
```

标签独立于变量和标识符，可以跟其他实体使用同一个名字而不会互相干扰。**goto语句和控制权转向的那条带标签的语句必须位于同一个函数内**。

### try语句块以及异常处理

异常处理机制包括：

throw 表达式：异常检测部分使用throw表达式来表示它遇到了无法处理的问题；

try 语句块：异常处理部分使用try语句块处理异常。try语句块以关键词try，并以一个或多个catch子句结束，try语句块抛出的异常通常会被某个catch子句处理；

#### throw

throw后面通常紧跟一个分号以及一个表达式，表达式的类型就是抛出异常的类型。

```C++
Sale_item item1, item2;
cin >> item1 >> item2;
if(item1.isbn() == item2.isbn()){
    cout << item1 + item2 << endl;
    return 0;
}else{
    cerr << " Data must refer to same ISBN" <<endl;
    return -1;
}

//真实的程序应该把对象相加的代码和用户交互代码分开，可以改为：
if(item1.isbn() != item2.isbn())
    throw runtime_error("Data must refer to same ISBN");

cout << item1 + item2 << endl;
```

抛出异常将终止当前函数，并把控制权转移到处理该异常的代码。

#### try语句块

继续上诉例子：

```C++
while(cin >> item1 >> item2){
    try{
        // 执行添加两个sales_item的代码
        // 如果添加失败，代码抛出一个runtime_error异常
        if(item1.isbn() != item2.isbn())
    	throw runtime_error("Data must refer to same 			ISBN");
		cout << item1 + item2 << endl;
    }catch(runtime_error err){
        cout << err.what()
            << "\n Try Again? enter y or n" << endl;
        char c;
        cin >> c;
        if(!cin || c == 'n')
            break; //跳出while循环
    }
}
```

每个标准库异常类都定义了名为what的成员函数，这些函数没有参数，返回C风格字符串（const char*)，其中return_error类的一个成员函数返回的是初始化一个具体对象时所用的string对象的副本。

**寻找处理代码的过程与函数调用链相反，当异常被抛出时，首先搜索抛出该异常的函数，如果没找到匹配的catch子句，终止该函数，并在调用该函数的函数中继续寻找。如果仍旧没有找到，以此类推，沿着执行路径逐层返回，直到找到适当类型的catch子句,如果都没找到，则会转屌标准库函数terminate，非正常终止当前程序的执行**

# 函数

## 函数基础

我们通过调用运算符执行函数，调用运算符是一对圆括号，作用于一个表达式，该表达式是函数或者指向函数的指针：圆括号内是一个用逗号隔开的实参列表，用实参初始化函数的形参，调用表达式的类型就是函数的返回类型。

**函数的形参列表可以为空，但不能省略：**

```C++
void f1(){}
void f2(void){}
```

函数的返回类型不能是数组类型或者函数类型，但可以是指向数组或者函数的指针。

**我们把仅存在于块执行期间的对象称为自动对象。**

static类型：局部静态变量在程序执行路径第一次经过定义语句时初始化，并且直到程序终止时才被销毁，在此期间即使对象所在的函数结束执行也不会影响。

```C++
size_t count_calls(){
    static size_t ctr = 0;
    return ++ctr;
}
int main(){
    for(size_t i = 0; i != 10; i++){
        cout << count_calls() << endl;
    }
    return 0;
}
//这段程序输出从1到10的数字
```

内置类型的局部静态变量初始值默认为0；

函数的名字必须在使用前声明，函数只能定义一次，但是可以声明多次。

函数声明无需函数体，用一个分号代替即可，函数的声明没有函数体，因此也就无需形参的名字。

**建议在头文件中声明函数，在源文件中定义函数**

## 参数传递

如果形参是引用类型，它将绑定到对应的实参上；否则，将实参的值拷贝后赋给形参。

### 传值参数

当初始化一个非引用类型的变量时，初始值被拷贝给变量，此时对变量的改动不会影响初始值。

### 指针形参

指针的行为和其他非引用类型一样，当执行指针拷贝操作时，拷贝的是指针的值，拷贝之后，两个指针是不同的指针。因为指针使我们可以间接的访问所☞对象，所以通过指针可以修改所指对象的值。

```C++
void reset(int *ip){
    * ip = 0;  // 改变指针所指对象的值
    ip = 0;	   //只改变了ip的局部拷贝，实参未被改变
}
```

### 传引用指针

通过引用形参，允许函数改变一个或多个实参的值

```C++
void reset(int &i){
    i = 0; // 改变了i所引对象的值
}
```

拷贝大的类类型对象或者容器过于低效，甚至有的类类型根本就不支持拷贝操作。

```C++
bool isshorter(const string &s1, const string &s2){
    return s1.size() < s2.size();
}
//避免拷贝string类型的对象，当无需修改实参值时可以把形参定义为对常量的引用。
```

**当函数无需修改引用形参的值时最好使用常量引用**

### const形参与实参

当用实参初始化形参时会忽略形参的顶层const，当形参有顶层const时，传给它常量对象或者非常量对象都是可以的

```C++
void fcn(const int i){}
//fcn能够读取i，但是不能对i写值。
```

问题会出在重载部分：

```C++
void fcn(const int i){}
void fcn(int i){}  // 错误，重复定义了fcn(int)
```

参数传递的初始化规则：

```C++
int i = 0;
const int ci = i;
string::size_type ctr = 0;
reset(&i); //调用形参类型为int *的 reset函数
reset(&ci); //错误：不能用const int*类型的对象初始化int*类型的形参，类型不匹配
reset(i); //调用形参类型为int& 的reset函数
reset(ci); //错误：不能用const int 类型的对象 初始化普通引用
reset(42); //错误：int&类型的对象不能用常量初始化
reset(ctr); //错误：类型不匹配，ctr是无符号类型
```

**尽量使用常量引用，普通引用会误导使用者，且如上所示，使用普通引用会极大限制函数所能接受的实参类型。**

### 数组形参

数组的两个特殊性质:

1）不允许拷贝数组；2）使用数组时会将其转化为指针；

因此当我们为函数传递一个数组时，实际上传递的是指向数组首元素的指针。

我们仍然可以把形参写成类似数组的形式：

```C++
void print(const int*);
void print(const int[]);
void print(const int[10]);
```

上面三个函数等价，当编译器处理对print的调用时，只检查传入的参数是否是const int*类型

```C++
int i = 0, j[2] = {0, 1};
print(&i);  //right
print(j);  //right
```

当我们给print函数传递一个数组时，实参自动转化为指向数组首元素的指针，数组的大小对函数的调用没有影响。

为了防止越界，通常使用以下三种方式管理数组形参：

1）数组本身包含一个结束标记，比如C风格字符串，储存在字符数组中，并且最后一个字符后面跟着一个空字符；

2）使用标准库范式，即传递指向数组首元素和尾后元素的指针；

3）显示传递表示数组大小的形参；

C++允许将变量或者形参定义为数组的引用：

```C++
void print(int (&arr)[10]){
    for(auto elem : arr)  //范围for需要知道arr大小
        cout << elem << endl;
}
//注意(&arr)[10]；没有()则会变成引用的数组，即十个元素全是引用
```

由于引用类型的形参要写明大小，当我们在形参显示写明数组的大小之后，就会限制函数的使用，比如上式，当我们传递大小不为10的数组时，就会报错；

传递多维数组：

```C++
// matrix 指向数组的首元素，该数组的元素是由10个整数构成的数组
void print(int (*matrix)[10], int rowSize){}
```

当使用数组的语法定义函数时，编译器会忽略掉第一个维度，所以最好不要把它包括在形参里：

```C++
void print(int matrix[][10], int rowSize){}
```

上式虽然看起来是一个二维数组，实际上形参是指向含有十个整数的数组的指针；

### main

传递给main的参数：

```C++
//当我们在命令行输出: prog -d -o ofile data0
int main(int argc, char *argv[]){}
//argv是个指向char的指针的数组：
argv[0] = "prog";
argv[1] = "-d";
...
argv[5] = 0;
// 要意识到argv的首元素通常保存程序名，因此第一个可选实参从argv[1]开始
```

### 含有可变形参的函数

为了能编写处理不用数量实参的函数，C++提供了两种方法：
1）如果所有实参类型相同，可以传递一个名为 initializer_list的标准库模型；

2）如果实参的类型不同，则要编写可变参数模板；

*C++还有一种特种的形参（省略符），可以用于传递可变数量的形参，一般仅用于与C函数交互的接口程序*

### initializer_list

函数数量未知但是实参类型相同。

![image-20230227115805044](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230227115805044.png)

​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          

定义initializer_list对象必须说明表中所含元素类型:

```C++
initializer_list<string> ls;
initializer_list<int> li;
```

**initializer_list对象的元素永远是常量值，无法改变元素值**

```C++
//输出错误信息的函数
void error_msg(initializer_list<string> li){
    for(auto beg = li.begin(); beg != li.end(); beg++)
        cout << *beg << " ";
    cout << endl;
}
```

调用方式：

```c++
//expected 和 actual 是 string对象
if(expected != actual)
    error_msg({"functionX", expected, actual});
else
    error_msg({"functionX", "okay"});
```

**本质上就是将形参初始化在一个列表当中**

### 省略符形参

**省略符形参只能出现在形参列表的最后一个位置**

```c++
void foo(para_List, ...);
void foo(...);
```

## 返回类型 and return

return 的两种形式：

```c++
return;
return expression;
```

### 无返回值函数

无返回值的return只能用在void作为返回类型的函数中（不要求非要有return，会隐式执行）；

当想要提前退出void时，可以在函数中间使用return；

### 有返回值的函数

函数返回类型不是void时，则return必须返回一个值；return返回值必须与函数返回类型相同，**或者能隐式的转为函数的返回类型**

return返回值时会拷贝一个临时副本，当返回的是引用时，则不会拷贝，直接返回引用；

**不要返回局部对象的引用和指针，显然因为不在作用域：**

```c++
const string &manip(){ //返回类型是常量引用
    string ret;
    if(!ret.empty())
        return ret;
    else
        return "empty";
}
```

调用一个返回引用的函数会得到左值，如果是非常量引用，我们还能对其赋值：

```c++
char &get_val(string &str, string::size_type ix){
    return str[ix];
}
int main(){
    string s("a value");
    cout << s << endl;
    get_val(s, 0) = 'A';
    cout << s << endl;
    return 0;
}
```

函数可以返回花括号包围的值的列表：

```c++
vector<string> process(){
    //expected 和 actual 是 string 对象
    if(expected.empty())
        return {};
    else if(expected == actual)
        return{"fuctionX", "okay"};
    else
        return { "functionX", expected, actual};
}
```

**我们允许main没有return语句直接结束，编译器会隐式地插入一条return 0的语句**

### 返回数组指针

因为数组不能被拷贝，所以函数只能返回数组的指针或者引用(使用类型别名简化)：

```c++
typedef int arrT[10];  //arrT是一个类型别名，它表示的类型是含有十个整数的数组
using arrT = int[10];  //等价上式
arrT* func(int i);  //func返回一个指向含有十个整数的数组的指针
```

当不使用类型别名时声明一个返回数组指针的函数：

```c++
int arr[10];  //arr 是一个含有十个整数的数组
int *p1[10];  //p1 是一个含有十个指针的数组
int (*p2)[10] = &arr;  //p2是一个指针，它指向含有十个整数的数组
//当我们声明一个返回数组指针的函数时：
Type(*function(parameter_list))[dimension]{
    ...
}
```

C++ 11提出了一种简化上诉func声明的方法，使用**尾置返回类型**，任何函数都能使用尾置返回类型，但是对于返回类型比较复杂的函数最有效：

```c++
//func 接受一个int类型的实参，返回一个指针，指针指向含有10个整数的数组
auto func(int i) -> int(*)[10];
```

使用decltype也是一种方式：

```c++
int odd[5];
int even[5];
decltype(odd) *arrPtr(int i){
    return(i % 2) ? &odd : &even;
}
```

## 函数重载

### 重载和const形参

当同一个作用域内的几个函数名字相同但形参列表不同，我们将之称为重载函数；这些函数接受的形参类型不一样，但是执行的操作非常的类似。当调用这些函数时，编译器会根据传递的实参类型推断想要的是哪个函数：

```c++
int j[2] = {0, 1};
print("hello world");
print(j, end(j) - begin(j));
print(begin(j), end(j));
```

**main函数不能重载**

**不允许两个函数除了返回类型外其它所有的要素都相同：**

```c++
//每对声明的是同一个函数
Record lookup(const Account &acct);
Record lookup(const Account &);  //省略形参名字

typedef Phone Telno;
Record lookup(const Phone&);
Record lookup(const Telno&)
```

同样，一个拥有顶层const的形参无法和另一个没有顶层const的形参区分开来：

```c++
Record lookup(Phone);
Record lookup(const Phone);  //重复声明了Record lookup(Phone)

Record lookup(Phone*);  
Record lookup(Phone* const)//重复声明了Record lookup(Phone*)
```

另一方面，如果形参是某个类型的指针或引用，则通过区分其指向的是常量对象还是非常量对象可以实现重载：

```C++
Record lookup(Account&);  //函数作用于普通引用
Record lookup(const Account&);  //新函数，作用于常量引用

Record lookup(Account*);   //函数作用于普通指针
Record lookup(const Account*)//新函数，作用于指向常量的指针
```

上式中编译器可以通过实参是否是常量来推断应该调用哪个函数。

### const_cast 和重载

```C++
const string &shorterString(const string &s1, const string &s2){
    return s1.size() <= s2.size() ? s1 : s2;
}
```

这个函数无论传递的实参是不是const类型，返回的结果都是const string 的引用，它可以修改为：

```C++
string &shorterString( string &s1, string &s2){
    auto &r =  &shorterString(const_cast<const string&>(s1)), const_cast<const string&>(s2);
    return const_cast<string&>(r);
} 
```

上式首先将实参强制转化成对const的引用，然后调用shorterString函数的const版本，得到一个const string & 的返回结果，由于该引用实际绑定在一个非常量实参上，因此函数最后将其转为非常量引用。

### 重载函数的调用

当调用重载函数时可能出现的结果：

> 1）编译器找到一个与实参最佳匹配的函数，并生成调用该函数的代码；
>
> 2）找不到任何一个函数与调用的实参匹配，此时编译器发出无匹配的错误信息；
>
> 3）有多于一个函数可以匹配，但是每一个都不是明显的最佳选择。此时也将发生错误，称为二义性调用；

**当在内层作用域中声明名字，将隐藏外层作用域中声明的同名实体。同时，在不同的作用域中无法重载函数名**

## 特殊用途语言特性

### 默认实参

为了使得函数既能接纳默认值，也能接受用户指定的值，将其定义如下形式：

```C++
typedef string::size_type sz; 
string screen(sz ht = 24, sz wid =80, char backgrnd = ' ');
```

上式为每一个形参都提供了默认实参，默认实参作为形参的初始值出现在形参列表中；

**注意！一旦某个形参被赋予了默认值，它后面的所有形参都必须有默认值**

函数调用时实参必须按位置解析，默认实参负责填补函数调用缺少的尾部实参：

```c++
window = screen(, , '?');  //错误：只能省略尾部的实参
window = screen('?'); //合法，调用 screen('?', 80, ' ')，因为char可以转化为size_type类型 ？== 63
```

**在给定的作用域中，一个形参只能被赋予一次默认实参。**

```C++
string screen(sz, sz, char = ' ');
string screen(sz, sz, char = '*');  //错误：重复声明
string screen(sz = 24, sz = 80, char);  // 正确：添加默认实参
```

**局部变量不能作为默认实参。除此之外，只要表达式的类型能转化为形参所需的类型，表达式就能作为默认实参。**

### 内联函数与constexpr函数

内联函数可以避免函数调用的开销，将函数指定为 inline，通常就是将它在每个调用点上“内联地”展开，假设我们将shorterString函数定义成为内联函数，则如下调用：

```C++
cout << shorterString(s1, s2) << endl;
```

将在编译过程中展开成类似下面的形式：

```C++
cout << (s1.size() < s2.size() ? s1 : s2) << endl;
```

从而消除函数运行时的开销。

声明方式：

```c++
inline const string &shorterString(const string &s1, const string &s2){
    return s1.size() <= s2.size() ? s1 : s2;
}
```

**一般来说，内联机制用于优化规模较小、流程直接、频繁调用的函数。内联仅仅是向编译器发出一个请求，编译器可以选择忽略。**

constexpr函数是指能用于常量表达式的函数：

> 1）函数的返回类型和所有形参都得是字面值类型；
>
> 2）并且函数体内必须有且仅有一条return语句；

```C++
constexpr int new_sz(){return 42;}
constexpr int foo = new_sz();
```

**对于某个给定的内联函数或者constexpr函数，编译器在展开时需要函数的定义，因此允许它们在程序中多次定义，但它们的多个定义必须完全一致；基于此，我们一般将内联函数以及constexpr函数定义在头文件中。**

### assert 和 NDEBUG

assert是一种预处理宏，行为类似内联函数，使用一个表达式作为它的条件：

```C++
assert(expr);
```

首先对expr求值，如果表达式为假，则assert输出信息并终止程序的执行；如果表达式为真，则什么也不做；

宏名字在程序内必须唯一，含有cassert头文件的程序不能再定义名为 assert的变量、函数、或其它实体。

assert常用于检查不能发生的条件：

```C++
assert(word.size() > threshold);  //程序的所有给定单词都必须大于某个阈值
```

assert的行为依赖于一个名为NDEBUG的预处理变量的状态，如果定义了NDEBUG，则assert什么都不做。默认状态下没有定义NDEBUG，此时assert将执行运行时检查。

除了用于assert之外，也可以使用NDEBUG编写自己的条件调试代码：

```C++
void print(const int ia[], size_t size){
    #ifndef NDEBUG
    	// __func__存放函数的名字
    	ceer << __func__ << ":array size is " << size << endl;
    #endif
    //...
}
```

![image-20230306173414122](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230306173414122.png)

如上图，C++编译器还定义了四个对程序调试很有用的名字。

## 函数指针

###  定义函数指针

函数指针指向的是函数而非对象，函数指针指向某种特定类型。**函数的类型由它的返回类型和形参类型共同决定，与函数名无关：**

```C++
bool lengthCompare(const string &, const string &);
```

该函数的类型是bool(const string&, const string&)。要想声明一个可以指向该函数的指针，只需要用指针替换函数名即可：

```C++
bool (*pf)(const string &, const string &);  //未初始化
```

**注：没有括号则pf是一个返回bool*类型的函数**

当我们把函数名作为一个值使用时，该函数自动转换为指针：

```C++
pf = lengthCompare;
pf = &lengthCompare;  //等价于上一行，取地址符可选
//调用，解引用指针同样是可选的
bool b1 = pf("hello", "goodbye");
bool b2 = (*pf)("hello", "goodbye");  //等价调用
bool b3 = lengthCompare("hello", "goodbye");  //等价调用
```

在指向不同函数类型的指针间不存在转换规则，但可以为函数指针赋一个nullptr或者值为0的整型常量表达式；

### 使用函数指针

当我们使用重载函数时，上下文必须清晰界定到底应该选用哪个函数：

```C++
void ff(int*);
void ff(unsigned int);

void (*pf1)(unsigned int) = ff;  //pf1指向ff(unsigned)

void (*pf2)(int) = ff;  //错误
double (*pf3)(int*) == ff; //错误

```

**不能定义函数类型的形参，但是形参可以是指向函数的指针**

```C++
// 第三个形参是函数类型，它会自动转换成指向函数的指针
void useBigger(const string &s1, const string &s2, bool pf(const string &, const string &));
//等价声明
void useBigger(const string &s1, const string &s2, bool (*pf)(const string &, const string &));
//直接将函数作为实参使用,会自动转换成指针：
useBigger(s1, s2, lenthCompare);

//简化函数指针作为形参的声明
//Func 和 Func2 是函数类型
typedef bool Func(const string&, const string&);
typedef decltype(lengthCompare) Func2;  //等价类型
//Funcp 和 Funcp2 是指向函数的指针
typedef bool (*Funcp)(const string&, const string&);
typedef decltype(lengthCompare) *Funcp2;  //等价类型
```

> 关于typedef与函数指针的使用：

```c++
typedef bool (*Funcp)(const string&, const string&);
```

> 如上式，实际上写成下式更好理解(但不正确，必须写作上式)

```C++
typedef bool(*)(const string&, const string&) Funcp;
```

> 显然，typedef将Funcp作为了函数指针的别名，函数别名的使用同理

当使用typedef定义了自己的类型以后，则声明useBigger时可以重新以如下形式：

```C++
void useBigger(const string&, const string&, Func);
void useBigger(const string&, const string&, FuncP2);
```

上两式等价，第一条语句中，编译器自动将Func表示的函数类型转换为指针

### 返回指向函数的指针

声明一个返回函数指针最简单的方式就是使用类型别名：

```C++
using F = int(int*, int);
using PF = int(*)(int*, int);
```

与函数类型的形参不同，返回类型不会自动地转换成指针，我们必须显式地将返回类型指定为指针：

```C++
PF f1(int);  //正确
F f1(int);  //错误，F是函数类型，f1不能返回一个函数
F *f1(int);  //正确
```

同样，我们可以用如下形式直接声明f1:

```C++
int (*f1(int))(int*, int);
```

和返回数组指针类似，我们需要一个（）将函数整体包括起来，接着在尾部加上返回类型的剩余部分；在上式中，f1是一个函数，它的形参是一个int类型的参数，它的返回类型是

```C++
int*(int* ,int)
```

同样，C++11的尾置返回类型也能使用：

```c++
auto f1(int) -> int*(int*, int);
```

使用auto 和 decltype简化函数指针的使用：定义函数，返回函数指针指向相同类型的函数：

```C++
string::size_type sumLength(const string&, const string&);
string::size_type largerLenth(const string&, const string&);
//上两个函数类型完全一致，则我们可以编写一个函数，接受string类型的参数，返回指向上两个函数之一的指针：
decltype(sumLength) *get_Fcn(const string &);
```

**要牢记将decltype作为某个函数时，它的返回类型是函数类型而非指针，因此我们要加上*以表面我们需要返回指针**

# 类

类的基本思想是**数据抽象**与**封装**，数据抽象是一种依赖**接口**和**实现**分离的编程设计技术。

类想要实现数据抽象和封装，首先需要定义一个**抽象数据类型**，在抽象数据类型中，由类的设计者思考并设计类的实现过程，而使用该类的程序员只需要明白类做了什么，而不需要了解类的工作细节。

## 定义抽象数据类型

定义和声明成员函数的方式与普通函数差不多，成员函数的声明必须放在类的内部，定义既可以在内部也可以在外部；

```c++
struct Sales_data{
    //在函数后面加const，将其声明为常成员函数，常成员函数不能修改成员变量的值
    std::string isbn() const {return bookNo;}
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
    
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// Sales_data 的非成员接口函数
Sales_data add(const Sales_data&, const Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
std::istream &read(std::istream&, const Sales_data&);
```

**定义在类内部的函数是隐式的inline函数**

###  定义成员函数

#### 引入this

所有的成员函数必须在类内部声明，但成员函数体可以定义在类内也可以定义在类外。

成员函数通过一个名为 **this** 的额外隐式参数来访问它的对象，当我们调用一个成员你函数时，用请求该函数的对象地址初始化this:

```C++
total.isbn();
```

例如调用上式，则编译器负责把total的地址传递给isbn的隐式形参this，可以等价认为编译器将该调用重写成了如下形式：

```C++
total.isbn(&total);  //伪代码
```

在成员函数中，我们可以直接调用该函数的对象的成员，无须通过成员访问符来实现这一点：当isbn使用bookNo时，它隐式地使用this指向的成员，就像我们书写了this->bookNo一样。

**this是一个常量指针，因此任何同名参数和变量的定义都是非法的**

#### 引入 const 成员函数

```C++
std::string isbn() const {return bookNo;}
```

isbn 函数参数列表之后的const 关键字用于修改隐式this指针的类型。

由于this是隐式的，并且不会出现在参数列表中，当我们想要将this声明成指向常量的指针时，C++允许的做法就是把const关键字放在成员函数的参数列表之后，伪代码如下：

```C++
std:: string isbn(const Sales_data *const this){
    return this->bookNo;
}
```

**常量对象，以及常量对象的引用或者指针都只能调用常量成员函数（不能将非常量赋值给常量指针或者常量引用）**

#### 类作用域与成员函数

即便成员变量定义在成员函数之后，比如bookNo定义在isbn函数之后，isbn也能够使用bookNo。因为编译器分为两步处理类：首先编译成员的声明，然后才轮到成员函数体。**因此，成员函数体能够随意使用类中其它成员而无需在意成员出现的次序。**

#### 类的外部定义成员函数

类外部定义的成员必须包含它所属的类名，如果成员被声明为常量成员函数，则定义也必须在参数列表后明确指定 const 属性：

```C++
double Sales_data::avg_price() const{
    if(units_sold)
        return revenue / units_sold;
    else
        return 0;
}
```

#### 定义返回this对象的函数

```C++
//成员函数返回this
return this; //返回当前对象的地址
return *this; //当函数的返回类型是引用时，则返回当前对象本身，否则返回当前对象的拷贝

//Sales_data中 combine 的定义
Sales_data &Sales_data::combile(const Sales_data &rhs){
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}
```

### 定义类内非成员函数

**一般来说，如果非成员函数是类接口的组成部分，则这些函数的声明应该与类在同一个头文件内。**

```C++
istream &read(istream &is, Sales_data &item){
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

ostream &print(ostream &os, Sales_data &item){
    os << item.isbn() << " " << item.unit_sodl << " " << item.revenue << " " << item.avg_price();
    return os;
}
```

**IO类属于不能被拷贝的类型**

```C++
Sales_data add(const Sales_data &lhs, const Sales_data &rhs){
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}
```

### 构造函数

类通过一个或者几个特殊成员函数控制对象的初始化过程，这些函数叫做构造函数，**无论何时类的对象被创建，就会执行构造函数**。

构造函数的名字和类名相同，和其它函数不同在于**构造函数没有返回类型**。类可以包含多个构造函数，**不同的构造函数之间必须在参数数量和参数类型上有所区别**。

不能将构造函数声明为const，当我们创捷类的一个const对象时，**直到构造函数完成初始化过程，对象才能真正取得其常量属性**。因此，**构造函数在const对象的构造过程中可以向其**。

#### 默认构造函数

当类没有显式地定义地定义构造函数时，编译器会为我们隐式地定义一个默认构造函数。构造函数规则如下：

1）当存在类内初始值时，用它来初始化类成员；

2）否则，默认初始化该成员

编译器合成的默认构造函数只适合非常简单的类，对于一个普通的类来说，我们必须定义它自己的默认构造函数，原因如下：

1）编译器只有发现类不包含任何构造函数的情况下才会生成默认的构造函数，一旦定义了其他的构造函数，那么除非我们定义默认构造函数，否则类将没有默认构造函数；

**只有当类没有声明任何构造函数时，编译器才会自动地生成默认构造函数。**

2）对于某些类来说，合成的默认构造函数可能执行错误的操作：如果定义在块中的内置类型或者复合类型（比如指针和数组）的对象被默认初始化，则他们的值将是未定义的；

**如果类内包含内置类型或复合类型的成员，只有当这些成员都被赋予了初始值，这个类才适合使用合成的默认构造函数**

3）编译器不能为某些类合成默认的构造函数。比如，某个类中包含一个其它类类型的成员且这个成员的类型没有默认构造函数，那么编译器将无法初始化该成员；

构造函数例子：

```C++
struct Sales_data{
    //新增构造函数
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s){}
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(p*n){}
    Sales_data(std::istream &);
    
    //之前已有的成员
    std::string isbn() const {return bookNo;}
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// Sales_data 的非成员接口函数
Sales_data add(const Sales_data&, const Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
std::istream &read(std::istream&, const Sales_data&);
```

#### =default的含义

```C++
Sales_data() = default;
```

**C++11新标准**中，当我们需要默认的行为，那么可以通过在参数列表后面写上 = default 要求编译器生成默认构造函数，**=default可以和声明一起出现在类的内部，也可以作为定义一起出现在类的外部**。出现在**内部**，而默认构造函数是**内联**的，出现在**外部**，则该成员默认情况下**不是内联**的；

#### 构造函数初始值列表

```C++
Sales_data(const std::string &s):bookNo(s){}
Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(p*n){}
```

上俩式中，冒号与花括号之间的代码称为构造函数初始值列表，它负责为新创建的对象的一个或者几个数据成员赋初值；

构造函数不应该轻易地覆盖掉类内初始值，如果不能使用类内初始值，则所有的构造函数都应该显式地初始化每个内置类型的成员。

**使用构造函数初始值列表而不是放在构造函数体内初始化，因为有些成员函数必须初始化，比如引用、const对象。而成员函数的定义是在声明完之后才执行的。因此，当我们在构造函数定义内其实是对成员执行赋值操作。**

成员的初始化顺序与它们在类定义中出现顺序一致。构造函数初始值列表中初始值的前后位置关系不会影响实际的初始值顺序。

#### 在类的外部定义构造函数

```C++
Sales_data::Sales_data(std::istream &is){
    read(is, *this);  //read函数的作用是从is中读取一条信息存入this对象中
}
```

构造函数没有返回类型，函数从函数名字开始，类外定义构造函数，同样需要指明构造函数是哪个类的成员。

#### 默认实参用于构造函数

```C++
class Sales_data{
public:
    Sales_data(string s = ""):book_No(s){}
};
```

上诉Sales_data中，默认构造和给定一个string实参的构造函数一样，并且形参是默认实参，因此可以将其作为默认构造函数，不用另外写一个默认构造函数。

**如果一个构造函数为所有参数都提供了默认实参，则它实际上也定义了默认构造函数。**

#### 委托构造函数

C++11 新标准

一个委托构造函数使用它所属类的其它构造函数执行它自己的初始化过程：

```C++
struct Sales_data{
    //非委托构造函数使用对应实参初始化成员
    Sales_data(string s, unsigned cnt, double price):bookNo(s), units_sold(cnt), revenue(cnt * price){}
    //其余构造函数全部委托给另一个构造函数
    Sales_data():Sales_data("", 0, 0){}
    Sales_data(string s): Sales_data(s, 0, 0){}
    Sales_data(std::istream &is)Sales_data(){read(is , *this);}
};
```

#### 默认构造函数的作用

当对象被默认初始化或者值初始化时自动执行默认构造函数。

默认初始化在以下情况下发生：

1）当我们在块作用域内不使用任何初始值定义一个非静态变量或者数组时；

2）当一个类本身含有类类型的成员并且使用合成的默认构造函数之时（即编译器合成的构造函数）；

3）当类类型的成员没有在构造函数初始值列表中显式地初始化时；

值初始化在以下情况下发生：

1）在数组初始化的过程中如果我们提供的初始值数量少于数组的大小；

2）当我们不使用初始值定义一个局部静态变量时（默认初始化为0）；

3）当我们书写形如T（）的表达式显式地请求值初始化时，其中T是类型名。

**编译器不能为某些类合成默认的构造函数。比如，某个类中包含一个其它类类型的成员且这个成员的类型没有默认构造函数，那么编译器将无法初始化该成员：（就像类中有未初始化的内置或者复合类型对象，当编译器使用合成的默认构造函数，则这些对象会被赋予未定义的值）**

```C++
class NoDefault{
public:
    NoDefault(const std::string &); //不是默认构造函数
};
struct A{
    NoDefault my_mem;  
};
A a;  // 错误：不能为A合成构造函数
struct B{
	B(){}  // 错误： b_member没有初始值
    NoDefault b_member;
};
```

C++新手常犯错误：

```C++
Sales_data obj();  //声明一个函数，并不是对象，也自然不会使用默认构造函数
Sales_data obj;  //正确的声明使用默认构造函数初始化的对象
```

#### 转换构造函数（隐式的类类型转换）

与C++内置类型隐式转换规则类似，我们也能为类定义隐式转化规则。**如果构造函数只接受一个实参，则它实际上定义了转换为此类类型的隐式转换机制，称为转换构造函数。**

```C++
class Sales_data{
public:
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s){}
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(p*n){}
    Sales_data(std::istream &);
    std::string isbn() const {return bookNo;}
    Sales_data& combine(const Sales_data&);
};

Sales_data item("c");
string null_book = "9-999-9999-99";
item.combine(null_book);  
//上式等价于：
Sales_item tmp(null_book);
item.combine(tmp);
//或者：
item.conbine(Sales_data("9-9999-999-9")); 

//只允许一步类类型转换
item.combine("9-9999-999-9"); // 错误
item.conbine(string("9-9999-999-9"));  //正确
item.conbine(Sales_data("9-9999-999-9"));  //正确
```

#### explicit

在要求隐式转换的程序上下文中可以通过将构造函数声明为explicit加以阻止：

```c++
class Sales_data{
public:
    ales_data() = default;
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(p*n){}
    explicit Sales_data(const std::string &s):bookNo(s){}
    explicit Sales_data(std::istream &);
};
```

如此，就没有任何构造函数能用于隐式地创建Sales_data对象：

```C++
item.combine(null_book);  
item.combine(cin);  //错误：构造函数是 explicit 的
```

**explicit 只对一个实参的构造函数有效，因为多个实参的构造函数不能用于隐式转换。只能在类内声明构造函数时使用explicit关键字，在类外定义时不应重复。**

虽然被声明为explicit的构造函数不支持隐式转换，但仍然可以显式转换：

```C++
item.combine(Sales_data(null_book)); // 正确
item.combine(static_cast<Sales_data>(cin));
// static_cast:强制类型转换
```

#### 字面值常量类

当满足下列条件，称为聚合类：

1）类的所有成员都是public；

2）没有定义任何构造函数；

3）没有类内初始值；

4）没有基类，也没有virtual函数；

constexpr函数的参数和返回值必须是字面值类型。当数据成员都是字面值类型的聚合类是字面值常量类。

构造函数不能是const的，但字面值常量类的构造函数可以是constexpr函数，一个字面值常量类必须至少提供一个constexpr构造函数。

constexpr构造函数体一般是空的；

## 访问控制与封装

C++中使用访问说明符加强类的封装性：

1）定义在 public 说明符之后的成员在整                                                                                               个程序内可被访问，public成员定义类的接口；

2）定义在 private 说明符之后的成员可以被类的成员函数访问，当不能被使用该类的代码访问，private部分封装了类的实现细节；

Sales_data类重新定义如下：

```C++
class Sales_data{
public:
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s){}
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(p*n){}
    Sales_data(std::istream &);
    std::string isbn() const {return bookNo;}
    Sales_data& combine(const Sales_data&);
    
private:
    double avg_price() const{return units_sold ? revenue / units_sold : 0;};
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```

一个类可以包含0~多个访问说明符，并且某个说明符出现的次数也没有严格限定。

**struct 和 class 的唯一区别在于默认访问权限不同，struct默认public，而class默认private；**

## 友元

### 友元函数

类可以允许其它类或者函数访问它的非公有成员，方法是让其他类或者函数称为它的友元：

```C++
// 为 Sales_data的非成员函数所作的友元声明
class Sales_data{
    friend Sales_data add(const Sales_data&, const Sales_data&);
	friend std::ostream &print(std::ostream&, const Sales_data&);
	friend std::istream &read(std::istream&, const Sales_data&);

public:
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s){}
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(p*n){}
    Sales_data(std::istream &);
    std::string isbn() const {return bookNo;}
    Sales_data& combine(const Sales_data&);
    
private:
    double avg_price() const{return units_sold ? revenue / units_sold : 0;};
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// Sales_data 的非成员接口函数声明
Sales_data add(const Sales_data&, const Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
std::istream &read(std::istream&, const Sales_data&);
```

友元声明只能出现在类的内部，但具体位置不限制，友元不是类的成员，不受所在区域访问控制级别的约束。

**友元的声明仅仅指定访问权限，并非通常意义上的函数声明，因此如果想要调用友元函数，我们必须再另外对函数进行一次声明。**

### 友元类

除了能声明友元函数，C++允许将其它类定义为友元，也可以把其它类的成员函数定义为友元。**此外，友元函数能定义在类的内部，这样的函数是隐式内联的。**

```C++
class Screen{
    // Window_mgr 的成员可以访问Screen的私有部分
	friend class Window_mgr;   
};
```

如果一个类指定了友元类，则友元类的成员函数可以访问此类包括非公有成员在内的所有成员。

```C++
class Window_mgr{
public:
    using ScreenIndex = std::vector<Screen>::size_type;
    void clear(ScreenIndex);
private:
    std::vector<Screen> screens{Screen(24, 80, ' ')};
};

void Window_mgr::clear(ScreenIndex i){
    Screen &s = screens[i];
    s.contents = string(s.height * s.width, ' ');
}
```

如果clear不是Screen的友元，上面的代码就不能通过编译，因为此时clear将不能访问Screen的height、width和contents成员。而将Window_mgr指定为友元后，Screen的所有成员对于Window_mgr就都变成可见的了。

**友元关系不具有传递性**

除了令整个类作为友元之外，Screen还可以只为clear提供访问权限，当把一个成员函数声明为友元时，必须指出这个成员函数属于哪个类：

```C++
class Screen{
    //Window_mgr::clear必须在Screen类之前被声明；
    friend void Window_mgr::clear(ScreenIndex);
}
```

想要定义友元，对程序的结构必须仔细组织：

1）首先定义Window_mgr类，其中声明clear函数，但是不能定义它。在clear函数使用Screen类的成员之前必须先声明Screen;

2）定义Screen，包括对于clear的友元声明；

3）最后定义clear，此时它才能使用Screen的成员。

**尽管函数的重载名字相同，但仍属于不同的函数。一个类想要把一组重载函数都声明为它的友元，则需要对这组函数中的每一个分别声明。**

类和非成员函数的声明不是必须在他们的友元函数之前。就算在类的内部定义了友元函数，我们也必须在类外部提供相应的声明从而使得函数可见。

```C++
struct X{
	friend void f(){/*友元函数可以定义在类的内部*/}
    x(){f();}  // 错误:f还没有被声明
    void g();
    void h();
};

void X::g(){f();}  // 错误:f还没有被声明
void f();
void X::h(){f{};}  //正确
```

## 类的其它特性

定义一个类型成员，除了定义数据和含函数成员之外，**类还可以自定义某种类型在类中的别名，由类定义的类型别名和其它成员一样存在访问限制，并且可以是 public 或者 private 中的一种：**

```C++
class Screen{
public:
    //类型成员必须先定义后使用，不同于类的其它成员
    typedef std::string::size_type pos;
    //等价声明：using pos = std::string::size_type;
private:
    pos cursor = 0;
    pos height =, width = 0;
    std::string contents;
};
```

给Screen增加细节，让它更加实用：构造函数让用户自定尺寸以及内容，两个成员函数负责移动光标以及读取给定位置的字符：

```C++
class Screen{
public:
    typedef std::string::size_type pos;
    Screen() = default;  // 有了其它构造函数，就要写一个默认构造函数
    Screen(pos ht, pos wd, char c):height(ht), width(wd), contents(wd * ht, c){}
    char get() const{  //读取光标处的字符
        return contents[cursor];
    }  //隐式内联
    inline char get(pos r, pos wd) const;  //显式内联
    Screen &move(pos r, pos c);
private:
    pos cursor = 0;
    pos height = 0, width = 0;
    std::string contents;
};
```

### 令成员作为内联

类内定义的函数自动声明为inline函数，同时，我们也可以显式地将类外定义的函数声明为inline函数：

```C++
//类外指定inline
inline Screen &Screen::move(pos r, pos c){  
    pos row = r * width;
    cursor = row + c;
    return *this;
}
//类内显式声明为inline
char Screen::get(pos r, pos c) const{
    pos row = r * width;
    return contents[row + c];
}
```

**最好只在类外部定义的地方说明inline，显然这样更容易理解。**

### 重载成员函数

成员函数同样可以被重载，只要函数之间在参数的数量或者类型上有区别就行：

```C++
// Screen 中重载定义了两个get()
Screen myscreen;
char ch = myscreen.get();
ch = myscreen.get(0, 0);
```

### 可变数据成员

一个可变数据成员永远不会是const，即便它是const对象的成员。**因此，一个const成员函数可以改变一个可变成员的值：**

```C++
class Screen{
public:
    void some_member() const;
private:
    mutable size_t access_ctr;  //即使在const对象内也可以被修改
};

void Screen::some_member() const{
    ++access_ctr;  //保存一个值，用于记录成员函数被调用的次数
}
```

尽管some_member是一个const成员函数，它仍然能够改变access_ctr的值。

### 类数据成员的初始值

类内初始化必须使用=的初始化形式或者花括号括起来的直接初始化形式：

```C++
class Window_mgr{
private:
    // 默认情况下，一个window_mgr包含一个标准尺寸的空白Screen
    // 使用一个单独的元素值对vector成员进行了初始化
    // vector默认是一个单元素对象
    std::vector<Screen> screens{Screen(24, 80, ' ')};
};
```

### 返回*this的成员函数

```C++
inline Screen &Screen::move(pos r, pos c){  
    pos row = r * width;
    cursor = row + c;
    return *this;
}
```

返回引用的函数是左值的，意味着这些函数返回的是对象本身而非对象的副本：

```C++
// 实现替换光标位置字符功能
inline Screen &Screen::set(char c){
    contents[cursor] = c;
    return *this;
}
// 调用
Screen myscreen;
myscreen.move(4, 0).set('#');
// 上式等价于：
myscreen.move(4,0);
myscreen.set('#');
// 如果返回类型不是引用，则：
Screen temp = myScreen.move(4, 0); //对返回值进行拷贝
temp.set('#'); //不会改变myScreen的contents，因为返回的仅仅是拷贝
```

### 从const 成员函数返回*this

如果简单地对一个const成员函数返回引用对象，则可能会出现无法编译的情况：

```C++
// display 的返回类型是 const Screen&
myScreen.display(cout).set('*');  //无法通过编译，显然set无权改变一个const对象
```

**const成员函数以引用的形式返回*this，那么它的返回类型将是常量引用。**

我们可以对成员函数进行重载，以区分对方是否是const的：

```C++
class Screen{
public:
    Screen &display(std::ostream &os){
        do_display(os);
        return *this;
    }
    const Screen &display(std::ostream &os) const{
        do_display(os);
        return *this;
    }
private:
    void do_display(std::ostream &os) const{
        os << contents;
    }
};
```

成员函数互相调用时，this指针隐式传递。因此，当display的非常量版本调用do_display时，它的this指针将隐式地从指向非常量的指针转为指向常量的指针，而当do_display完成后，display会各自返回解引用this所得的对象。

### 类类型

每个类定义了唯一的类型。即便是两个成员完全一样的类，他们也是不同的类型。

```C++
Sales_data iteml;
class Sales_data iteml;  //等价的初始化Sales)data类型对象的方式
```

**我们能够仅仅声明类而不定义它**

```C++
class Screen;
```

声明且未定义的类称为**不完全类型**，可以定义指向这种类型的指针或者引用，也可以声明以不完全类型作为参数或者返回类型的函数。

**只有当类全部完成后类才算被定义，所以一个类的成员类型不能是该类自己。然而，一旦类名出现，就代表它被声明过了，因此类允许指向它自身类型的指针或者引用：**

```C++
class Link_screen{
	Screen window;
    Link_screen *next;
    Link_screen *prev;
};
```

## 类的作用域

每个类都会定义自己的作用域。在类的作用域外，普通的数据和函数成员只能由对象、引用或者指针使用成员访问运算符访问。

```C++
void Window_mgr::clear(ScreenIndex i){
    Screen &s = screens[i];
    s.contents = string(s.height * s.width, ' ');
}
```

如上，因为编译器在处理参数列表前已经明确clear函数正位于Window_mgr类的作用域中，所以不必再专门说明screen以及ScreenIndex是在Window_mgr类中定义的。

**当成员函数定义在类外，并且函数的返回类型使用的类型位于某个类的作用域外时，则必须指明它是哪个类的成员。**

**编译器处理完类中的全部声明之后才会处理成员函数的定义，但是成员函数声明、返回类型、参数列表中使用的名字则必须在使用前就确保可见。**

```C++
typedef double Money;
string bal;
class Account{
public:
    Money balance(){return bal;}
private:    
    Money bal;
};
```

Money在类外定义，banlance会先在自己这条语句之前的类作用域内查找，之后查找类外；函数体在类处理完之后才定义，因此返回的是类成员 bal 而不是 string bal;

**一般地，内层作用域可以重新定义外层作用域中的名字，即使这个名字已经使用过。然而在类中，如果成员使用了外层作用域中的某个名字，而该名字代表一种类型，则不能在之后重新定义：**

```C++
typedef double Money;
string bal;
class Account{
public:
    Money balance(){return bal;}  //使用外层作用域的Money
private:    
    typedef double Money;  // 错误：不能重新定义
    Money bal;
};
```

## 类的静态成员

有时候类需要一些成员与本身直接相关，而不是各个对象。

### 声明静态成员

在成员声明前加上static使其与类关联，static成员可以是public也可以是private的：

```c++
class Account{
public:
    void calculate(){amount += amount * interestRate;}
    static double rate(){return interestRate;}
    static void rate(double);
private:
    string owner;
    double amount;
    static double interestRate;
    static double initRate();
};
```

类的静态成员存在于任何对象之外，对象中不包含任何与静态数据成员有关的数据。

静态成员函数不包含this指针。静态成员函数不能声明为const的，而且也不能在static函数体内使用this指针。

### 使用类的静态成员

使用作用域运算符直接访问静态成员：

```C++
double r;
r = Account::rate();
```

仍然可以使用类的对象、引用、指针来访问静态成员：

```c++
Account ac1;
Account *ac2 = &ac1;

r = ac1.rate();
r = ac2->rate();
```

成员函数不需要使用作用域运算符就能直接使用静态成员：

```C++
class Account{
public:
    void calculate(){amount += amount * interestRate;}
private:
    static double interestRate;
};
```

### 定义静态成员

既可以在类内，也可以在类外定义静态成员函数。**当在类外定义静态成员时，不能重复static关键字，该关键字只能出现在类内声明处**：

```C++
void Account::rate(double){
    interestRate = newRate;
}
```

**类的静态成员不是由类的构造函数初始化的，且不能在类的内部初始化静态成员。必须在类的外部定义和初始化每个静态成员，一个静态数据成员只能定义一次：**

```C++
double Account::interestRate = initRate();  //从类名开始，剩余部分语句就位于类作用域内，因此可以直接使用initRate(),而不需要使用作用域符，即便它是private的。
```

### 静态成员类内初始化

通常类的静态成员不应该在类的内部初始化。**但我们可以为静态成员提供const整数类型的类内初始化，不过要求静态成员必须是字面值常量类型的constexpr。**

```C++
class Account{
public:
    static double rate(){return interestRate;}
    static void rate(double);
private:
    static constexpr int period = 30;  //period是常量表达式
    double daily_tbl[period];  //可以用一个初始化了的静态数据成员指定数组成员的维度
};
```

**即使一个常量静态数据成员在类的内部被初始化了，通常情况下也应该在类的外部定义一下该成员，因为常量静态数据成员不属于任何一个对象，自然也不会为了它申请额外空间，当有另外的函数需要调用这个类的常量静态成员时，会找不到它：**

```C++
constexpr int Account::period;  // 初始化在类内定义提供，因此类外不能再指定一个初始值
```

### 静态成员对比普通成员的优势所在

静态数据成员可以是不完全类型（声明且未定义的类）。特别的，静态数据成员的类型可以就是他所属的类类型。而非静态数据成员则只能声明为它所属类的指针或者引用：

```C++
class Bar{
public:
    //...
private:
    static Bar mem1;  //静态成员可以是不完全类型
    Bar *mem2;  //正确，指针成员可以是不完全类型
    Bar mem3;  //错误：数据类型必须是完全类型
};
```

我们还可以使用静态成员作为默认实参，而普通成员不行：

```C++
class Screen{
public:
    Screen& clear(char = bkground);
private:
    static const char bkground;
};
```

# IO库

C++不直接处理输入输出，而是通过一族定义在标准库中的类型处理IO。

IO库定义了读写内置类型值的操作。此外一些类，如string，通常也会定义类型的IO操作，来读写自己的对象。

## IO类

iostream 定义了用于读写流的基本类型，fstream定义了读写命令文件的类型，sstream定义了读写内存string对象的类型。

![image-20230316154437685](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230316154437685.png)

为了支持使用宽字符的语言，标准库定义了一组类型和对象来操纵wchar_t类型的数据。宽字符版本的类型和函数的名字以一个w开始。

### IO类型间的关系

标准库通过**继承机制**使我们忽略不同类型之间的流的差异。继承机制使我们可以声明一个特定的类继承自另一个类，我们可以通过一个派生类（继承类）对象当作其基类对象来使用。我们如何使用基类的成员函数（如istream中的>>或者getline），就能同样对派生类对象调用这些成员函数（如fstream中的>>和getline）。

#### IO对象无拷贝或者赋值

**IO对象无拷贝或者赋值**

```C++
ofstream out1, out2;
out1 = out2;  //错误
ofstream print(ofstream);  //错误：不能初始化ofstream参数
out2 = print(out2);  //错误：不能拷贝流对象
```

显然，不能拷贝流对象自然不能将形参或者返回类型设置为流类型。**进行IO操作的函数通常以引用方式传递和返回流。读写一个IO对象会改变其状态，因此传递和返回的引用不能是const的。**

####  条件状态

由于流可能处于错误状态，因此代码通常应该在使用一个流之前检查它是否处于良好状态。确定一个流对象的状态的最简单方法是将它作为一个条件来使用：

```C++
while(cin >> word)
    //...
```

IO库定义了四个 iostate 类型的 constexpr 值表示特定的位模式，这些值用来表示特定类型的IO条件，可以与位运算符一起使用来一次性检测或设置多个标志位。

分别为：badbit，failbit，eofbit，goodbit.

当某个或某些值被置位时，对应流的某些状态(比如badbit置位代表系统级错误)。

对应有一组函数用于检查这些标志位的状态：bad, fail, good, eof.

```C++
auto old_state = cin.rdstate(); //返回流的当前状态
cin.clear();  //将四个标志位复位，让cin重新有效
process_input(cin);  //使用cin
cin.setstate(old_state);  //将cin置位原来状态
```

带参数的clear()接受一个iostate值，表示流的最新状态，用于复位单一的条件状态位。

```C++
cin.clear(cin.rdstate() & ~cin.failbit & ~cin.badbit);  //复位failbit 和 badbit，保持其它标志位不变
```

#### 管理输出缓冲

每个输出流都管理一个缓冲区，用来保存程序读写的数据。有了缓冲机制，操作系统就可以将程序的多个输出操作组合成单一的系统级写操作。

导致缓冲刷新（数据真正写到输出设备或文件）的原因：

1）程序正常结束，作为main函数的return操作的一部分，缓冲刷新被执行；

2）缓冲区满时，需要刷新缓冲，而后新的数据才能继续写入缓冲区；

3）使用操作符（如endl）来显式地刷新缓冲区；

4）在每个输出操作之后，我们可以用操作符 unitbuf 设置流的内部状态，来清空缓冲区。**默认情况下，对cerr是设置unitbuf的，因此写到cerr的内容都是立即刷新的。**

5）一个输出流可能被关联到另一个流。当读写被关联的流时，关联到的流的缓冲区会被刷新（默认情况下，cin和cerr都关联到cout。因此，读cin或写cerr都会导致cout的缓冲区被刷新）。

```C++
cout << "hi!" << endl;  //输出hi！和一个换行符，然后刷新缓冲区
cout << "hi!" << flush;  //输出hi！，然后刷新缓冲区，不附加任何额外字符
cout << "hi!" << ends;  //输出hi！和一个空字符，然后刷新缓冲区

cout << unitbuf;  //之后的所有输出操作后都会立即刷新缓冲区，即每次写操作之后都进行一次flush操作
cout << nounitbuf;  //回到正常的缓冲模式
```

**如果程序异常终止，输出缓冲区是不会被刷新的。**

**当一个输入流被关联到一个输出流时，任何试图从输入流读取数据的操作都会先刷新关联的输出流。标准库将cin和cout关联在一起：**

```C++
cin >> ival;
```

上式会导致cout的缓冲区被刷新。

```C++
//不带参数版本：返回指向输出流的指针，如果当前对象关联到一个输出流，则返回的就是指向这个流的指针，否则返回空指针
x.tie();
//带一个参数版本，令自己关联到此输出流
x.tie(&o);  //返回原先关联的输出流，将x关联到o

cin.tie(&cout);  //将cin与cout关联
cin.tie() << "fq!";  //可以写成这样,因为cin.tie()返回cout

ostream *old_tie = cin.tie(nullptr);  //cin不再与其它流关联,old_tie为原来关联的输出流，这里是cout

cin.tie(&cerr);  //将cin与cerr绑定在一起，读取cin会刷新cerr而不是cout
cin.tie(old_tie);  //重建cin与cout之间的关联
```

**每个流同时最多关联到一个流，但多个流可以同时关联到同一个ostream**

## 文件的输入输出

fstream定义了三个类型支持文件IO: ifstream读文件，ofstream写文件，fstream读写文件。

### 使用文件流对象

创建文件流对象时，我们可以**可选的**提供文件名，如果提供，则open会被自动调用：

```C++
ifstream in(ifile);  //构造一个ifstream并打开给定文件,ifile为string类型参数
ofstream out;  //输出文件流未关联到任何文件
```

C++11新标准中文件名既可以是库类型string，也可以是c风格字符数组。

**在要求使用基类型对象的地方可以使用继承类型的对象代替**：接收一个iostream类型引用（或者指针）参数的函数，可以用一个对应的fstream类型来调用。

```C++
// 假定输入文件和输出文件的名字是通过传递给main函数的参数来指定的
int main(int argc, char *argv[]){
//argv[0]通常存放程序的名字，因此从1开始
    ifstream input(argv[1]);
    ofstream output(argv[2]);  //打开文件
    
    Sales_data total;
    if(read(input, total)){
        Sales_data trans;
      	print(output,total) << endl;
    }
    
}

istream &read(istream &is, Sales_data &item){
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

ostream &print(ostream &os, Sales_data &item){
    os << item.isbn() << " " << item.unit_sodl << " " << item.revenue << " " << item.avg_price();
    return os;
}
```

 当我们定义了一个空文件流对象，可以随后调用open来将它与其它文件关联起来：

```C++
ifstream in(ifile);
ofstream out;
out.open(ifile + ".copy");  //打开指定文件
```

如果调用open失败，则failbit会被置位，因此运行open是否成功的检测是个好习惯：

```C++
if(out){
    /*open成功，可以使用文件*/
}
```

**对一个已经打开的文件流调用open会失败，并且导致failbit被置位。为了将文件流关联到另外一个文件，必须首先关闭已经关联的文件：**

```C++
in.close();
in.open(ifile + "2");
```

open（）成功会置流状态，使得good（）为true。

```C++
for(auto p = argc + 1; p != argv + argc; ++p){
    ifstream input(*p);
    if(input)
        process(input);
    else
        cerr << "****" + string(*P);
}  //每个循环步input都会离开作用域，则会被销毁
```

**当一个fstream对象离开其作用域时，与之关联的文件会自动关闭。当一个fstream对象被销毁时，close()会自动被调用。**

####文件模式

每个流都有一个关联的文件模式，用来指出如何使用文件。

![image-20230316180008775](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230316180008775.png)

无论以何种方式打开文件，我们都可以指定文件模式，指定文件模式有如下限制：

1）只可以对ofstream或者fstream对象设定out模式；

2）只可以对ifstream或者fstream对象设定in模式；

3）只要trunc（覆盖）没被设定，就可以设定app模式。在app模式下，即使没有显式设定out，文件也总是以out模式被打开；

4）默认情况下，即使我们没有设定trunc（覆盖），以out打开的文件也会被截断。因此，为了保留以out模式打开的文件的内容，我们必须同时指定app模式。或者同时指定in模式，即同时进行读写操作；

5）ate 和binary模式可用于任何类型文件流对象，且可以和其它任何文件模式组合使用。

**每个文件流类型都定义了一个默认文件模式，未指定则默认**

**仅以out模式打开的文件会丢弃已有数据，因此同时指定app**

```C++
ofstream out("file1");  //隐式out，并截断
ofstream out2("file1",out);  //显式out，并截断
    ofstream out2("file1",out | app);  //显式指定app
```

每次调用open的可以改变其文件形式：

## string 流

sstream定义了三个类型支持内存IO，istringstream、ostringstream和stringstream;

### 使用stringstream

```C++
//使用istringstream
string line, word;
vector<PersonInfo> people;
while(getline(cin, line)){
    PersonInfo info;
    istringstream record(line);
    record >> info.name;
    while(record >> word)
        info.phones.push_back(word);
    people.push_back(info);
}
```

### 使用ostringstream

逐步构造输出，最后一起打印：

```C++
for(const auto &entry : people){
    ostringstream formatted, badNums;
    for(const auto &nums :entry.phones){  // phones 是个vector
        //假设valid()用于验证有效，format()用于改写格式
        if(!valid(nums)){
            badNums << " " << nums;
        }else{
            formatted << " " << format(nums);
        }
    }
    if(badnNums.str().empty())
        os << entry.name << " " 
        << formatted.str() << endl;
    else
        cerr << "input error:" << entry.name
        << "invalid number(s)" << badNums.str() << endl;
}
```

# 顺序容器

元素在顺序容器中的顺序与其加入容器时的位置相对应，所有容器类都共享公共的接口，不同容器按不同方式对其进行扩展。**一个容器就是一些特定类型对象的集合**。

## 容器库概览

一般来说，每个容器都定义在一个头文件中，文件名与类型名相同。比如:list定义在list头文件中。对大多数，但不是全部容器，我们还需要额外提供元素类型的信息：

```C++
list<Sales_data>;
deque<double>;
```

可以定义元素类型是另一个容器的容器：

```C++
vector<vector<string>> lines;
```

## 迭代器

迭代器有公共的接口，所有提供相同操作的迭代器对某个操作的实现方式都是相同的。

###  迭代器范围

一个迭代器范围由一对迭代器表示，两个迭代器分别指向同一个容器中的元素或者是**尾元素之后的位置**，这两个元素通常被称为begin和end，这种元素范围被称为左闭合区间。

```C++
[begin, end)
```

**begin和end必须指向相同的容器，end不能指向begin之前的位置。**

### begin 和 end 成员

begin 和 end 有多个版本，带r的版本返回反向迭代器，以c开头的版本则返回 const 迭代器：

```C++
list<string> a = {};
auto it1 = a.begin(); //list<string>::iterator
auto it1 = a.rbegin(); //list<string>::reverse_iterator
auto it1 = a.cbegin(); //list<string>::const_iterator
auto it1 = a.crbegin(); //list<string>::const_reverse_iterator
```

不以c开头的函数都是被重载过的。即：实际上有两个begin成员，一个是const 成员，返回的是const_iterator类型。另一个是非常量成员，返回容器的iterator类型。**当我们对一个非常量对象调用这些函数时，得到的是返回iterator的版本。只有在对一个const对象调用这些函数时，才会得到一个const版本。**可以将一个普通的iterator转换为对应的const_iterator，反之不行（const_cast:添加或者去掉const）。

**当不需要写访问时，应使用 cbegin 和 cend**

### 容器定义和初始化

每个容器都定义了一个默认构造函数。除array之外，其它容器的默认构造函数都会创建一个指定类型的空容器。且都可以接受指定容器大小和元素初始值的参数。

将一个容器初始化为另一个容器的拷贝：

```C++
list<string> authors = {"Milton","Austen"};
vector<const char*> articles = {"a", "an", "the"};

// 直接拷贝整个容器
list<string> list(authors);  //正确
deque<string> authList(authors); //错误:容器类型不匹配
// 拷贝由一个迭代器指定的范围
forward_list<string> words(articles.begin(), articles.end());  //正确，可以将const char* 转化为string
```

**当拷贝迭代器指定范围时，容器类型可以不匹配，元素类型也可以不一致（但要能转换）**

显式列表初始化：

```C++
list<string> authors = {"Milton","Austen"};
vector<const char*> articles = {"a", "an", "the"};
```

**对于除了array之外的容器类型，初始化列表还隐含地指定了容器的大小**

顺序容器提供这样一个构造函数:它接受一个容器大小和一个（可选的）元素初始值。如果不提供，**则标准库会创建一个值初始化器：**

```C++
vector<int> ivec(10, -1);  //10个元素，每个都初始化为-1
list<string> svec(10, "hi");  //10个元素，每个都初始化为hi
forward_list<int> ivec(10); //10个元素，每个都初始化为0
```

**如果元素类型是内置类型或者具有默认构造函数的类类型，则可以只为构造函数提供一个容器大小参数。**

当定义array时，除了要指定元素类型，还要指定容器大小

当我们要使用array类型时，我们必须同时指定元素类型和大小：

```C++
array<int, 10>::size_type i;  //正确，数组类型包括元素类型和大小
array<int>::size_type j;  //错误
```

如果我们对array进行列表初始化，初始值的数目必须小于等于array的大小，**当初始值的数目小于array的大小时，它们被用来初始化array靠前的元素。**

```C++
array<int, 10> ia = {42};  //ia[0] 为 42. 剩余元素为 0
```

虽然不能对内置数组进行拷贝或者对象赋值，但对array没有这些限制：

```C++
int digs[10] = {0,1,2,3,4,5,6,7,8,9};
int cpy[10] = digs;  // 错误，内置数组不支持拷贝或者赋值
array<int, 10> digits = {0,1,2,3,4,5,6,7,8,9};
array<int, 10> copy = digits;  //正确，数组类型匹配则合法
```

### 赋值 assign and swap

```C++
c1 = c2;  //将c1的内容替换为c2中元素的拷贝
c1 = {a, b, c};  // 赋值后,c1的大小为3
```

1式左右容器相等，**如果原来两个容器大小不同，则赋值运算后两者的大小都与右边容器的原大小相同**。2式赋值后，c1大小变为花括号列表中值的数目。

```C++
array<int> a = {0};
a2 = {0};  //错误：不能将一个花括号列表赋值给数组
```

由于右边运算对象的大小可能与左边运算对象的大小不同，因此array不支持assign，也不允许用花括号包围的值列表进行赋值。

assign仅适用于顺序容器，赋值运算符要求左右运算对象具有相同的类型。**assign允许我们从一个不同但相容的类型赋值，或者从容器的一个子序列赋值。**

```C++
list<string> names;
vector<const char*> oldstyle;
names = oldstyle;  //错误：类型不匹配
// 正确，可以将const char* 类型转换为string
names.assign(oldstyle.begin(), oldstyle.end());

```

**由于旧元素会被新的拷贝替换，因此传递给assign的迭代器不能指向调用assign的容器。**

```C++
lsit<string> slist1(1);  //1个元素，为空string
slist1.assign(10, "hiya!");   //10个元素，每个都是hiya!
//等价于：
slist1.clear();
slist1.insert(slist1.begin(), 10, "hiya!");
```

swap操作交换两个**相同类型容器**的内容：

```C++
vector<string> svec1(10);
vector<string< svec2(24);
swap(svec1, svec2);
```

调用swap之后，svec1将包含24个string元素，svec2将包含10个元素（swap并未交换元素本身，只是交换了两个容器内部数据结构（即交换元素的内存地址））。

元素不会移动意味着：**迭代器，引用，指针仍然指向swap之前的元素，但这些元素在swap之后已经属于不同的容器了。**

如：iter假如指向svec1[3]，则swap之后，它指向svec2[3]。

**对string调用sawp会导致跌掉其、引用、指针失效**。

**！！！当我们swap两个array时，是会真正交换它们的元素的**

### 容器大小操作

1）成员函数 size() 返回容器中元素的数目；

2）empty() 当 size 为0时返回布尔值 true，否则返回 false；

3）max_size() 返回一个大于或等于该类型容器所能容纳的最大元素数的值。

**注：forward_list支持max_size和empty, 但不支持size**

### 关系运算符

每个容器都支持**相等运算符**（== 和 !=）；**除了无序关联容器外**的所有容器都支持关系运算符（>, >=, <, <=)。**关系运算符左右两边的运算对象必须是相同类型的容器，且必须保存相同类型的元素**。

> 如果两个容器大小相同切所有元素两两对应相等，则这两个容器相等；否则两个容器不等。
>
> 如果两个容器大小不同，但较小的容器中每个元素都等于较大容器中的对应元素，则较小容器小于较大容器。
>
> 如果两个容器都不是另一个容器的前缀子序列，则它们的比较结果取决于第一个不相等元素的比较结果。

**只有当容器的元素类型也定义了相应的比较运算符时，我们才可以使用关系运算符比较两个容器：**

```C++
vector<Sales_data> storeA, storeB;
if(storeA < storeB) //错误，Sales_data没有 < 运算符
```

## 顺序容器特有操作

### 向顺序容器中添加元素

![image-20230318222247463](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230318222247463.png)

push_back将一个元素追加到容器的尾部，除array和forward_list之外，每个顺序容器（包括string类型）都支持push_back。

```C++
string word;
while(cin >> word)
    container.push_back(word);
```

push_back的调用在容器的尾部创建一个实参的拷贝。容器的类型可以是list、vector、deque。

**容器元素是拷贝！无论是初始化或者插入**

list、forward_list、deque容器支持名为push_front的操作：

```C++
list<int> ilist;
//将元素插入到ilist开头
for(size_t ix =0; ix != 4; ++x){
    ilist.push_front(ix);
}
```

insert 成员允许我们在容器任意位置插入0个或多个元素。vector、deque、list和string都支持insert成员。forward_list提供了特殊版本的insert成员。

**insert函数将元素插入到迭代器所指定位置之前！**

```C++
slist.insert(iter, "hello!");  // 将"hello!"添加到iter之前的位置

vector<string> svec;
list<string> slist;

//等价于调用了slist.push_front("hello!");
slist.insert(slist.begin(), "hello!");

//vector不支持push_front，但我们可以这样
svec.insert(svec.begin(),"hello!");
```

除了上诉插入一个元素之外，insert还可以接受更多的参数：

```c++
//将指定数量的元素插入到指定位置之前，这些元素都按给定值初始化
//将10个“Anna”插入到end之前
svec.insert(svec.end(), 10, "Anna");

vector<string> v = {"quasi", "simba", "frollo", "scar"};
//将v的最后两个元素添加到slist的开始位置
slist.insert(slist.begin(), v.end() - 2, v.end());
slist.insert(slist.end(), {"these", "word"});

// 错误：迭代器表示拷贝范围，不能指向与目的地位置相同的容器
slist.insert(slist.begin(), slist.begin(), slist.end());
```

**如果我们传递给insert一对迭代器，则他们不能指向添加元素的目标容器。**

**接受元素个数或者范围的insert版本返回指向第一个新胶乳元素的迭代器。如果范围为空，则insert会返回第一个参数。**

```C++
list<string> lst;
auto iter = lst.begin();
while(cin >> word)
    iter = lst.insert(iter, word); 
```

新标准引入了三个新成员：emplace_front、emplace和emplace_back，**这些操作构造而不是拷贝元素**。三个成员分别对应push_front、insert和push_back（这三个成员拷贝对象， **而三个新成员使用传递给它们的参数在容器内层空间中直接构造元素**）。

```C++
//使用三个参数的Sales_data构造函数
c.emplace_back("978", 25, 15.99);

//错误：没有接受三个参数的push_back版本
c.push_back("978", 25, 15.99);

//正确：创建了一个临时Sales_data对象传递给push_back
c.push_back(Sales_data("978", 25, 15.99);
```

1、3式都创建了新的Sales_data对象，1会在容器管理的内存空间直接创建对象。3会创建一个局部临时对象，将其压入容器。

```C++
c.emplace_back();  //使用Sales_data的默认构造函数
c.emplace(iter, "999");  //使用Sales_data(string)
```

### 访问元素

包括array在内的所有顺序容器都有一个front成员函数，除了forward_list之外的所有顺序容器都有一个back函数。**这两个函数分别返回首元素和尾元素的引用：**

```C++
if(!c.empty()){
    auto val = *c.begin(), val2 = c.front();
    auto last = c.end();
    auto val3 = *(--last);  //不能递减forward_list迭代器
    auto val4 = c.back();  //forward_list不支持
}
```

 **在容器中访问元素的成员函数返回的都是引用，若容器是一个const对象，则返回值是const的引用。如果不是const的，则返回值是普通的引用。我们可以用来改变元素的值**：

```C++
if(!c.empty()){
    c.front() = 42;  //改变了c.front()的元素值
    auto &v = c.back();
    v= 1042;  //改变了c.back()的元素值
    auto v2 = c.back();  //v2不是一个引用，它是c.back()的一个拷贝
    v2 = 0;  //无法改变c.back()的元素值
}
```

如果我们希望下标是合法的，可以使用at成员函数，如果下标越界，at会抛出一个out_of_range异常：

```C++
vector<string> svec;
cout << svec[0];  //运行时错误，svec中没有元素
cout << svec.at(0);  //抛出out_of_range异常
```

### 删除元素

pop_front 和 pop_back 成员函数分别删除首元素和尾元素

。vector 和 string 不支持这两个成员函数，forward_list 不支持 pop_back 。**不能对一个空容器执行pop操作**

```C++
//这些操作返回 void
while(!ilist.empty()){
    process(ilist.front());  //某些处理
    ilist.pop_front();
}
```

erase成员函数从指定位置删除元素。**可以删除一个由迭代器指定的元素，也可以删除由一对迭代器指定范围内的所有元素。**两种形式的erase都返回指定删除的（最后一个）元素之后位置的迭代器：

```C++
list<int> lst = {0,1,2,3,4,5,6,7,8,9};
auto it = lst.begin();
while(it != lst.end()){
    if(* it % 2)
        it = lst.erase{it};
    else
        ++it;
}
```

删除多个元素：

```C++
// elem为迭代器
eleml = slist.erase(elem1, elem2);
```

### forward_list的特殊之处

单向链表无法访问前驱节点，因此没有定义insert, emplace 和 erase。而由insert_after, emplace_after 和 erase_after 的操作代替。**当我们想要删除elem3时，我们应用指向elem2的迭代器调用erase_after**。另外，forward_list 还定义了一个before_begin()用于返回**首前**迭代器，它允许我们在链表首元素之前添加删除元素。

```c++
forward_list<int> lst = {0,1,2,3,4,5,6,7,8,9};
auto pre = lst.before_begin();
auto curr = lst.begin();
while(curr != lst.end()){
    if(* curr % 2)
        curr = lst.erase_after{pre};
    else{
        pre = curr;
        ++curr;
    }
}
```

### 改变容器大小

array不支持resize函数。如果当前函数大于所要求的大小，则会删除容器后部的元素；如果当前大小小于新大小，则会将新元素添加到容器的后部:

```C++
list<int> ilist(10, 42);  //10个int, 每个值都是42
ilist.resize(15);  //将5个值为0的元素添加到ilist的末尾
ilist.resize(25, -1);  //将 10 个值为 0 的元素添加到ilist的末尾
ilist.resize(5);  //大小为5，值为42
```

**如果元素是类类型，当我们使用resize向容器添加元素时，必须提供初始值或者元素类型必须提供默认构造函数。**

## 容器操作可能使迭代器失效

在向容器添加元素后：

1）vector 和 string，如果存储空间重新分配，则指向容器的所有迭代器、指针、引用都将失效。如果未重新分配，则插入为之前的有效；插入位置后的无效。

2）插入deque除首尾位置之外任何位置的操作都会使迭代器、指针和引用失效，首尾位置插入会导致迭代器失效，但指向存在元素的引用和指针不会失效。

3）对于list 和 forward_list 指向容器的迭代器、指针、引用仍然有效。

当我们删除一个元素后，指向被删除元素的迭代器、指针和引用都会失效：

1）对于list 和 forward_list 指向容器其它位置的迭代器、指针、引用仍然有效。

2）deque除首尾位置之外任何位置的删除操作都会使所有的迭代器、指针和引用失效。删除首尾元素则首、尾后迭代器失效，其它位置不会影响。

3）vector 和 string，删除元素之前的仍然有效。

**当我们删除元素时，尾后迭代器总是失效。**

```C++
vector<int> vec = {0,1,2,3,4,5,6,7,8,9};
auto it = vec.begin();
while(it != vec.end()){
    if(* it % 2){
        it = vec.insert(it, * it);   //insert返回插入的第一个元素迭代器
        it += 2;  
    }else
        it = vec.erase{it};  /erase返回删除元素之后元素的迭代器
}
```

## vector对象是如何增长的

由于vector和string是可增长的，并且在内存上连续，因此每当我们增长它们就可能导致重新分配足够空间给它们的问题。如果每增长一次，就重新分配一次空间，那性能会十分差。**因此，stl实现者采取：对不得不重新分配新内存空间的情况，vector 和 string 的实现通常会分配比新空间需求更大的内存空间。**

![image-20230320171941570](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230320171941570.png)

**注1：调用reverse不会减少容器占用的内存空间数**

**注2：调用shrink_to_fit可以别忽略，即不一定保证会退回内存空间**

```C++
c.size();  // 返回c的实际大小
c.capacity();  //返回C所占内存可以保存的元素数目
c.reserve(50);  //将c的capacity至少设定为50，可能会更大
c.shrink_to_fit();  //请求归还剩余内存，令capacity == size,标准库不一定满足这个请求
```

## string 的众多函数

```C++
// 构造string的其它方法
string s(cp, n);  //s是cp指向数组中前n个字符的拷贝
string s(s2, pos2);  //s 是 string s2从下标pos2开始的字符的拷贝。
string s(s2, pos2, len2);  // s 是 string s2 从下标开始len2个字符的拷贝，构造函数至多拷贝s2.size() - pos2 个字符

// 子字符串操作
s.substr(pos, n);  //返回一个string, 包含s中从下标pos开始的n个字符的拷贝。pos默认实参为0，n默认实参为size() - pos。
```

```C++
//修改string中的字符的操作
s.insert(pos, args);  //在pos之前插入args指定的字符，pos可以是迭代器或者下标。接受下标的版本返回一个指向s的引用；接受迭代器的返回指向第一个插入字符的迭代器
s.erase(pos, len);  //删除从pos开始的 len 个字符，省略len则删除pos之后的所有字符，返回指向s的引用。
s.assign(args);  //将s中的所有字符替换为args指定的字符，返回引用
s.append(args);  //将args追加到s，返回一个指向s的引用
s.relaces(range, args);  //删除 s 中 range 范围内的内容，替换为args指定的字符。range可以是下标或者长度，或是一对指向s的迭代器。返回s的引用

/*上诉args可以是如下形式，assign和append可以使用所有形式:
(str);
(str,pos,len);
(cp, len);  // cp 字符串指针
(cp);
(n, c);  // n 个字符 c
(b, e);  // 迭代器b和e指定范围内的字符
{初始化列表};
*/
```

```c++
// string的搜索操作
s.find(args);  //查找args第一次出现的位置
s.rfind(args);  //查找args最后一次出现的位置
s.find_first_of(args);  //在s中查找args中任何一个字符第一次出现的位置
s.find_last_of(args);  //在s中查找args中任何一个字符最后一次出现的位置
s.find_first_not_of(args);  //在s中查找第一个不再args中的字符
s.find_last_not_of(args);  //在s中查找最后一个不再args中的字符

/*上诉args必须是：
(c, pos);  // 从pos开始找c
(s2, pos);  // 从pos开始找字符串s2
(cp, pos);  // 从pos开始找cp指向的c风格字符串
(cp, pos, n);  // 从pos开始找cp指向的数组中的前n个字符，pos和n无默认值
*/
```

```C++
//compare函数中的几种参数形式：
//n:个数  pos:下标  cp：指向c风格字符串指针
s.compare(s2);
s.compare(pos1, n1, s2);
s.compare(pos1, n1, s2, pos2, n2);
s.compare(cp);
s.compare(pos1, n1, cp);
s.compare(pos1, n1, cp, n2);
```

**当将string转化为数值时，第一个非空白字符必须是数值中可能出现的字符。**

```C++
//string和数值之间的转换
to_string(val);  //一组重载，返回数值val的string表示，小整型会被提升

//返回s的起始子串的数值，返回值分别为int\long\unsigned long\long long\ unsigned long long。示转换所用的基数，默认为10，是size_t的指针，用来保存s中第一个非数值字符的下标，p默认为0，即函数不保存下标：
stoi(s, p, b); 
stol(s, p, b);  
stoul(s, p, b);  
stoll(s, p, b);   
stoull(s, p, b);  

//返回s的起始字串的数值，返回值类型分别是float\double\long double。参数p的作用同上
stof(s, p);
stod(s, p);
stold(s, p);
```

## 容器适配器

容器、迭代器和函数都有适配器。顺序容器的适配器包括：stack、queue 和 priority_queue。**一个容器适配器接受一种已有的类型，使其看起来像是一种不同的类型**。如stack接受一个顺序容器(除array和forward_list之外)，并使其操作起来像一个stack一样.

![image-20230320202231479](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230320202231479.png)

每个适配器都定义了两个构造函数：默认构造函数创建一个空对象，接受一个容器的构造函数拷贝该容器用来初始化适配器。

```C++
//假设 deq 是 deque<int>
stack<int> stk(deq);
```

**默认情况下stack和queue都是基于deque实现的，priority_queue是在vector之上实现的。**

我们可以在创建一个适配器时将一个命名的顺序容器作为第二类型的参数来重载默认容器类型：

```C++
stack<string, vector<string>> str_stk;  //vector实现空栈
stack<string, vector<string>> str_stk2(str_stk);
```

stack可以使用除了array以及forward_list之外的任何容器类型来构造：

```C++
stack<int> intStack;  
for(size_t ix = 0; ix != 10; ++x){
    intStack.push(ix);  //intStack保存0-9十个数字
}
while(!intStacl.empty()){
    int value = intStack.top();  //.top()返回栈顶元素的引用
    intStack.pop();  //弹出栈顶元素
}
```

**我们只可以使用适配器操作，而不能使用底层容器类型的操作**：

```C++
inStack.push_back(ix);  //wrong
```

queue不能基于vector构造，priority_queue不能基于list构造。queue使用**先进先出**，priority_queue允许我们为队列中的元素创建优先级。新加入的元素会排在所有优先级比它低的元素之前。

# 泛型算法

标准库并未给每个容器都定义成员函数来实现诸如：查找特定元素、替换删除某个特定元素、重排元素顺序等操作。而是将这些操作被义为**泛型算法**。**泛型**指他们可以用于不同类型的元素和多种容器类型。

## 概述

除了少数例外，标准库算法都对一个范围内的元素进行操作。我们将此范围称为“输入范围”。**接受输入范围的算法总是使用前两个参数来表示此范围，两个参数分别指向要处理的第一个元素和尾元素之后位置的迭代器**。

### 只读算法

一些算法只会读取输入范围内的元素，而从不改变。比如：

```C++
std::find(vec.begin(), vec.end(), val);  //在[begin, end)之间查找val，返回val的迭代器或者end()
std::count(vec.begin(), vec.end(), val); //统计[begin, end)之间val出现的次数
int std::accumulate(vec.begin(), vec.end(), 0);  //统计[begin, end)之间所有元素的和，0为和的初值
int std::accumulate(vec.begin(), vec.end(), string(""));  // 可以用于string，但不能用于const char * 因为没有重载 +
```

操作两个序列的算法：

```C++
equal(roster1.cbegin(), roster1.cend(), roster2.cbegin());  //用于确定两个序列是否保存相同的值，每个元素一一比较，都相等返回true;接受三个迭代器，第三个参数用于表示第二个序列的首元素。
```

equal() 不要求元素类型一样，只要能通过 == 比较两个元素类型即可。

**equal的第二个序列的长度要大于等于第一个序列**

**所有接受单一迭代器用来表示第二个序列的算法，都假定第二个序列至少与第一个序列一样长**

### 写容器元素的算法

一些算法将新值赋予序列中的元素。当使用这些算法时必须确保序列原大小至少不小于我们要求写入的元素数目。**因为算法不会执行容器操作，因此它们自身不可能改变容器的大小**：

```C++
fill(vec.begin(), vec.end(), 0);  //将0赋予输入序列中的每个元素
```

**算法不会检查写操作：**

```C++
vectkr<int> vec;
fill_n(vec.begin(), vec.size(), 0);  //fill_n将给定值赋予迭代器指向元素开始的指定个数的元素
//由于fill_n假定指定个数是安全的：
fill_n(vector.begin(), 1, 0);  //容器是空的，结果未定义，灾难！
```

一种保证算法有足够空间来容纳数据的方法是使用**插入迭代器**。通常，**当我们通过一个迭代器向元素赋值时，值被赋予迭代器所指向的元素，而当我们调用插入迭代器，一个与赋值号右侧值相等的元素被添加到容器中。**

```C++
//back_inserter接受一个指向容器的引用，返回一个与该容器绑定的插入迭代器，当我们通过此迭代器赋值时，赋值运算符会调用push_back将一个具有给定值的元素添加到容器中
vector<int> vec;
auto it = back_inserter(vec);
*it = 42;  //vector有一个元素，值为42
```

常用back_insert创建一个迭代器：

```C++
vector<int> vec;
// 正确：back_inserter 创建一个插入迭代器，可用来向vec添加元素
fill_n(back_inserter(vec), 10, 0);  //giao!组合技！添加10个元素到vec
```

copy将输入范围内的元素拷贝到目的序列中。

```C++
int a1[] = {0,1,2,3,4,5,6,7,8,9};
int a2[sizeof(a1) / sizeof(*a1)];
// ret指向拷贝到a2的尾元素之后的位置
auto ret = copy(begin(a1), end(a1), a2);
//第三个参数表示目的序列的起始位置
```

**传递给copy的目的序列至少要包含与输入序列一样多的元素。**

replace算法将一个序列中所有等于给定值的元素都替换为另一个值：

```C++
replace(ilst.begin(), ilst.end(), 0, 42); //将所有0都换成42

//使用back_inserter 按需要增长目标序列
replace_copy(ilst.cbegin(), ilst.cend(),back_inserter(ivec) 0, 42);  //islt没有改变，ivec包含一份ilst的拷贝，不过原有的ilst中值为0的元素都变成了42
```

### 重排容器元素的算法

```C++
void elimDups(vector<string> &words){
    // sort 将 words 按字典排序
    sort(words.begin(), words.end());
    // unique 重新排序，使得每个单词的重复都出现在序列末尾,返回指向第一个重复元素的迭代器
    auto end_unique = unique(words.begin(), words.end());
    // 擦除重复单词
    words.erase(end_unique, words.end());
}
```

**即使words中没有重复单词，这样调用仍然俺去，这样的话 end_unique == end()**

## 定制操作

### 向算法传递函数

标准库中允许我们提供自己定义的操作代替某些算法的默认运算符。

**谓词**是一个**可调用的表达式**，其返回结果是一个能用作条件的值。**二元谓词意味着它们有两个参数**。接受谓词参数的算法对输入序列中的元素调用谓词。**因此，元素类型必须能转换为谓词的参数类型**。

```C++
bool isShorter(const string &s1, const string &s2){
    return s1.size() < s2.size();
}
// 按长度重排words
sort(words.begin(), words.end(), isShorter);

// 稳定版本sort，相等元素的次序不会变换
stable_sort(words.begin(), words.end(), isShorter);
```

### lambda 表达式

根据算法接受一元谓词还是二元谓词，我们传递给算法的谓词必须严格接受一个参数或者两个参数。我们可以向一个算法传递任何类别的**可调用对象**。一个lambda表达式表示一个可调用的代码单元。相当于一个未命名的**内联函数**，一个lambda具有一个返回类型、一个参数列表和一个函数体：

```C++
[capture list](parameter list) -> return type {function body}
```

capture list 是一个lambda所在函数定义的局部变量的列表（通常为空）。lambda必须尾置返回来指定返回类型。

```C++
auto f = []{return 42;};
cout << f() << endl;
```

**参数列表和返回类型可以忽略，但捕获列表和函数体必须永久保留。**

**如果函数体内包含任意单一return之外的内容，且未指定返回类型，则返回void**

与普通函数不同，lambda不能有默认参数。

```C++
//等价于isShorter
[](const string &s1, const string &s2){return a.size() < b.size();}

stable_sort(words.begin(), words.end(), [](const string &a, const string &b){return a.size() < b.size();});
```

捕获列表为空表明此lambda不使用它所在函数体中的任何局部变量。捕获列表指引lambda在其内部包含访问局部变量所需的信息：**捕获列表只适用于局部非static变量，因为它可以直接使用局部static变量**

```C++
[](const string &a){return a.size() >= sz;};  // 错误：sz未捕获
//调用find_if
auto wc = find_if(words.begin(), words.end(), [sz](const string &a){return a.size() >= sz;});  // 函数返回第一个长度不小于sz的元素的迭代器，如果没有则返回words.end()

//自适应输出复数
make_plural(count, "world", "s");

//for_each函数接收一个可调用对象，对输入序列中的每个元素调用此对象
for_each(wc, words.end(), [](const string &s){cout<< s << " ";});
cout << endl;
```

**lambda可以直接使用定义在当前函数之外的名字。**

```C++
//完整biggies
void biggies(vector<string> & words, vector<string>::size_type sz){
    //按字典排序并删除重复单词
    elimDups(words);
    //按长度排序
    stable_sort(words.begin(), words.end(), [](const string &a, const string &b){return a.size() < b.size();});
    auto wc = find_if(words.begin(), words.end(), [sz](const string &a){return a.size() >= sz;}); 
    auto count = words.end() - wc;
    cout << make_plural(count, "world", "s");
    for_each(wc, words.end(), [](const string &s){cout<< s << " ";});
cout << endl;
}
```

#### lambda捕获和返回

**当定义一个lambda时，编译器生成一个与lambda对应的新的类类型（未命名的）。**

当向一个函数传递lambda时，同时定义了一个新类型和该类型的一个对象：**传递的参数就是此编译器生成的类类型的未命名对象**。

**默认从lambda生成的类都包含一个对应该lambda所捕获的变量的数据成员。**

被捕获的变量的值是在lambda创建时拷贝，而不是调用时拷贝：

```C++
void fcnl(){
    size_v1 = 42;
    auto f = [v1]{return v1;}
    v1 = 0;
    auto j = f();  // j 为 42； f保存了我们创建它时的v1的拷贝
}
```

同时，我们定义lambda时可以采用引用方式捕获变量：

```C++
void fcnl(){
    size_v1 = 42;
    auto f = [&v1]{return v1;}
    v1 = 0;
    auto j = f();  // j 为 0
}
```

比如，当我们遇上不能拷贝的对象时，就可以使用引用：

```C++
void biggies(vector<string> &words, vector<string>::size_type sz, ostream &os = cout, char c = ' '){
    for_each(words.begin(), words.end(), [&os, c](const string &s){os << s << c;});  //ostream对象不能拷贝
}
```

我们可以从一个函数返回lambda，此lambda不能包含引用捕获，因为作用域消失。

**除了显式列出我们希望使用的变量外，还可以让编译器根据lambda中的代码推断我们需要使用的变量。当我们在捕获列表中写一个&（引用捕获）或者=（值捕获）时，编译器就会帮助我我们隐式捕获所需变量：**

```C++
wc = find_if(words.begin(), words.end(), [=](const string &s){ return s.size() >= sz;});
```

当我们想要对捕获变量采用不同的捕获方式时，可以混合使用显式、隐式捕获：

```C++
void biggies(vector<string> &words, vector<string>::size_type sz, ostream &os = cout, char c = ' '){
    // os 隐式引用捕获， c显示值捕获
    for_each(words.begin(), words.end(), [&, c](const string &s){os << s << c;});  
    // c 隐式值捕获，os显式引用捕获
    for_each(words.begin(), words.end(), [=， &os](const string &s){os << s << c;});  
}
```

**混合使用捕获方式时，第一个元素必须是=或者&。且显式捕获和隐式捕获的方式必须不同**

可变lambda支持改变一个被捕获的值，能省略参数列表：

```C++
void fcn3(){
    size_t v1 =42;
    auto f = [v1] () mutable { return ++v1;};
    v1 = 0;
    auto j = f();  // j == 43;
}
```

一个引用捕获的变量是否可以修改依赖于此引用指向的是否是const类型：

```C++
void fcn4(){
    size_t v1 =42;
    auto f = [&v1] () mutable { return ++v1;};
    v1 = 0;
    auto j = f();  // j == 1;
}
```

**如果函数体内包含任意单一return之外的内容，且未指定返回类型，则返回void**

```C++
transform(v1.begin(), v1.end(), v1.begin(),[](int i) -> int {if(i < 0) return -i; else return i;});  // 当我们需要为一个lambda定义返回类型时，必须使用尾置返回类型

transform(v1.begin(), v1.end(), v1.begin(),[](int i){if(i < 0) return -i; else return i;});  // 错误：无法推断返回类型
```

#### 参数绑定bind

如果lambda捕获列表为空，则可以使用函数代替它，多次使用时，应该用函数代替lambda列表。

名为bind的标准库函数，定义于functional中，可以看作一个通用函数适配器，接受一个可调用对象，返回一个新的可调用对象用来适应原对象的参数列表：

```C++
auto newCallable = bind(callable, arg_list);
```

其中，arg_list是一个逗号分隔的参数列表，对应给定callable的参数。**当我们调用newCallable时，newCallable会调用callable并将arg_list传递给它。**

```C++
bool check_size(const string &s, string:;size_type sz){
    return s.size() >= sz;
}
// check6是一个可调用对象，并用string 和 值6 来调用check_size
auto check6 = bind(check_size, _1, 6);
```

_1 为占位符，表示check6的此参数对应check_size的第一个参数，调用check6必须传递给它一个参数（占位符个数）。

```C++
string s = "hello";
bool b1 = check6(s);  //check6 会调用 check_size(s, 6)
```

因此，我们可以将：

```C++
wc = find_if(words.begin(), words.end(), [sz](const string &s){ return s.size() >= sz;});
```

改为：

```C++
wc = find_if(words.begin(), words.end(), bind(check_size, _1, sz));
```

占位符定义在命名空间placeholders中，而placeholders命名空间定义在functional头文件中

我们可以使用bind绑定可调用对象的参数或者重排其顺序：

```c++
// 假定f是一个可调用对象，有五个参数
auto g = bind(f, a, b, _2, c, _1);
```

生成一个新的可调用对象，它有两个参数，分别用_2 和 _1表示，这个新可调用对象将它自己的参数作为第三个和第五个参数传递给f，f的第一二四个参数分别被绑定在abc上。

**传递给g的参数按位置绑定到占位符，第一个参数绑定到_1，第二个参数绑定到\_2。**

```C++
g(X, Y);  ->  f(a, b, Y, c, X);
```

重排参数顺序，颠倒含义：

```C++
//isShorter(A, B) -> isShorter(B, A)
sort(words.begin(), words.end(), isShorter);
sort(words.begin(), words.end(), bind(isShorter, _2, _1);
```

绑定引用参数：

```C++
for_each(words.begin(), words.end(), bind(print, ref(os), _1, ' '));
```

标准库ref函数返回一个对象，包含给定的引用，此对象是可以拷贝的。cref函数生成一个保存const引用的类。

## 额外的迭代器

头文件iterator中定义了每个容器定义的迭代器以外的其它迭代器。

### 插入迭代器

接受一个容器，生成一个迭代器，实现向给定的容器添加元素。插入器有三种类型：

1）back_inserter 创建一个使用push_back 的迭代器；

2）front_inserter 创建一个使用push_front的迭代器；

3）inserter 创建一个使用 insert 的迭代器，此函数接受第二个参数，必须是指向给定容器的迭代器，元素将被插入到迭代器所指元素之前。

假设 it 是由 inserter 生成的迭代器，则：

```C++
*it = val;
```

等价于：

```C++
it = c.insert(it ,val);  //insert返回新插入的元素的迭代器
++it;  //使得it指向原来的元素
```

front_inserter生成的迭代器元素总是插入到容器第一个元素之前:

```C++
list<int> lst = {1, 2, 3, 4};
list<int> lst2, lst3;
// lst2 == {4, 3, 2, 1}
copy(lst.cbegin(), lst.cend(), front_inserter(lst2));
// lst3 =={1, 2, 3, 4}
copy(lst.cbegin(), lst.cend(), inserter(lst3, lst3.begin()));
```

**front_inserter 生成的迭代器会将插入的元素顺序颠倒。**

### iostream 迭代器

**流迭代器不支持递减！**

虽然不是容器，但是标准库仍然定义了这些IO类型对象的迭代器。 istream_iterator 读取输入流， ostream_iterator 向一个输出流写数据。**通过流迭代器，我们可以用泛型算法从流对象读取数据以及写入数据。**

当创建一个流迭代器时，必须指定迭代器将要读写的对象类型。一个istream_iterator使用>>来读取流。

```C++
istream_iterator<int> in_iter(cin);  //从 cin 读取 int
istream_iterator<int> int_eof;  //空的istream_iterator对象可以当作尾后迭代器使用
while(in_iter != eof){
    vec.push_back(*in_iter++);  // 后置++返回旧值并使用>>运算符读取下一个值
}
```

上式可以重写为：

```C++
istream_iterator<int> in_iter(cin), eof;
vector<int> vec(in_iter, eof);  //直接用一对表示范围的迭代器构造vec
```

![image-20230322175458516](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230322175458516.png)

流迭代器可以调用一些算法：

```
istream_iterator<int> in(cin), eof;
cout << accumulate(in, eof, 0) << endl;
```

同样，我们可以对任何具有输出运算符（<<）的类型定义ostream_iterator。**必须将ostream_iterator绑定到一个指定的流，不允许存在空的或者表示尾后位置的ostream_iterator**。

![image-20230322180313271](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230322180313271.png)

我们可以使用ostream_iterator来创建输出值的序列：

```C++
ostream_iterator<int> out_iter(cout, " ");
for(auto e : vec)
    *out_iter++ = e;  //使用<<将元素写到cout
cout << endl;
```

循环可以重写为：

```c++
for(auto e : vec)
    out_iter = e;  //使用<<将元素写到cout
cout << endl;
```

**也就是说，当我们向out_iter赋值时，我们可以忽略解引用和递增运算。**

通过调用copy 来打印vec中的元素：

```C++
copy(vec.begin(), vec.end(), out_iter);
cout << endl;
```

我们可以为任何定义了输入运算符的类型创建istream_iterator对象。因此我们可以使用流迭代器处理类类型。

### 反向迭代器

在容器中从尾元素向首元素反向移动的迭代器。**对于反向迭代器，递增（减）的含义会颠倒过来。递增一个反向迭代器会移动到前一个元素。**

除了forwrad_list外，其它容器都支持反向迭代器。通过rbegin、rend、crbegin和crend获得反向迭代器。分别指向尾元素和首元素之前的一个位置。

```C++
sort(vec.begin(), vec.end()); // 正常序排列
sort(vec.rbegin(), vec.rend());  // 逆序排列
```

反向迭代器需要递减运算，因此不可能从forward_list 或者一个流迭代器创建反向迭代器。

通过调用reverse_iterator的base成员函数给反向迭代器转向：

```C++
auto rcomma = find(line.crbegin(), line.crend(), ',');
cout << string(line.crbegin, rcomma) << endl;  //错误，会逆序打印最后一个单词
cout << string(rcomma.base(), line.cend()) << endl;
```

由于左闭右开的特性，转向之后的输入序列会向右平移一个元素。

## 泛型算法结构

大部分算法离不开下列四种形式：

```C++
alg(beg, end, other args);
alg(beg, end, dest, other args);
alg(beg, end, beg2, other args);
alg(beg, end, beg2, end2, other args);
```

beg, end 表示算法所操作的输入范围。dest、beg2、end2承担指定目的位置和第二个范围的角色。

 如果dest是一个直接指向容器的迭代器，那么算法将输出数据写入已存在的元素内。**更常见的做法是，dest绑定一个插入迭代器，或者ostream_iterator。将新元素写入容器里或者写入到一个输出流。**

## 特定容器算法

list 和 forward_list 定义了几个特定的成员函数形式的算法。

![image-20230323121847970](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230323121847970.png)

![image-20230323121908553](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230323121908553.png)

# 关联容器

关联容器的元素是按关键字来保存和访问的。两个主要的关联容器类型是map和set。

## 使用关联容器

map:

```C++
// 统计每个单词在输入中出现的次数
map<string, size_t> word_count;
string word;
while(cin >> word)
    ++word_count[word];
for(const auto & w : word_count)
    cout << w.first << w.second << endl;
```

set:

```C++
// 统计输入中每个单词出现的次数
map<string, size_t> word_count;
set<string> exclude = {"the", "a", "an"};  //列表初始化
//跳过the\a\an
while(cin >> word){
    if(exclude.find() == exclude.end())
    	++word_count[word];
}
for(const auto & w : word_count)
    cout << w.first << w.second << endl;
```

## 概述

### 定义关联容器

当定义一个map时，必须既指明关键字类型又指明值类型。而定义一个set时，仅需要指明关键字类型。**每个关联容器都定义了一个默认构造函数，它创建了一个指定类型的空容器。**我们也可以将关联容器初始化为另一个同类型容器的拷贝。或从一个值范围来初始化关联容器：

```C++
map<string, size_t> word_count;  //空容器

// 列表初始化
set<string> exclude = {"the", "a", "an"};  
map<string, string> authors = { {"joyce", "james"},{"austen", "jane"}, {"dickens", "charles"}};  
```

**一个map和set的关键字必须是唯一的，而multimap和multiset则没有此限制，它们允许多个元素具有相同的关键字：**

```C++
// ivec中保存0-9各两次共20个
vector<int> ivec;
for(int i = 0; i < 10; i++){
    ivec.push_back(i);
    ivec.push_back(i);
}

// iset中包含10个元素，miset中包含20个元素
set<int> iset(ivec.cbegin(), ivec.cend());
multiset<int> miset(ivec.cbegin(), ivec.cend());
```

### 关键字类型要求

**对于有序容器，必须定义元素比较的方法**。与算法类似，同样提供自己定义的操作来代替关键字上的<运算符。所提供的操作必须在关键字类型上定义一个**严格弱序**（严格小于等于、具有传递性）。**如果两个关键字是等价的，那么容器将它们视作相等来处理，当用作map关键字时，只能有一个元素和这两个关键字关联，我们可以用任何一个来访问对应的值：**

```c++
// 定义一个比较函数：
bool compareIsbn(const Sales_data &lhs, const Sales_data &rhs){
    return lhs.isbn() < rhs.isbn();
}

// 定义multiset(使用我们自己定义的操作)
// bookstore中的元素以ISBN的顺序进行排序
multiset<Sales_data, decltype(compareIsbn)*> bookstore(compareIsbn);
mulitset<Sales_data, string, decltype(compareIsbn)*> bookstore(compareIsbn);
//等价于:
multiset<Sales_data, bool(*)(const Sales_data &, const Sales_data &)> bookstore(compareIsbn);
```

用compareIsbn初始化bookstore对象，这表示我们向bookstore添加元素时，通过调用compareIsbn来为这些元素排序。用compareIsbn代替&compareIsbn，**当我们使用一个函数的名字时，它在需要的时候会自动转化为一个指针**

### pair 类型

pair 定义在头文件 utility 中，一个pair保存两个数据成员。

```C++
pair<string, string> anon;
pair<string, size_t> word_count;
pair<string, vector<in>> line;
```

**pair的默认构造函数会对数据成员进行值初始化。**我们也可以提供初始值：

```C++
pair<string, string> anon{"James", "joyce"};
```

**pair的数据成员是public的，分别命名为first，second**

map的元素是pair。

```C++
make_pair(v1, v2);  //返回一个用v1, v2初始化的pair
```

返回pair类型的函数：

```C++
pair<string, int> process(vector<string> &v){
    if(!v.empty())
        return {v.back(), v.back().size()};  //列表初始化
    	//等价替换：return make_pair(v.back(), v.back().size());
    else
        return pair<string, int>():  //隐式构造返回值
}
```

## 关联容器操作

```C++
// key_type: 容器类型的关键字类型
// mapped_type: 只适用于map，值类型
// value_type: 对于set, 与key_type相同，对于map，为pair<const key_type, mapped_type>

map<string, int>::value_type v;  // v是一个pair<const string, int>
map<string, int>::key_type v2;  // v2是一个string
```

### 关联容器迭代器

解引用关联容器迭代器会得到一个类型为容器的value_type的值的引用。

```C++
auto map_it = word_count.begin();
map_it->first = "new_key";  //错误，key是const的
++map_it->second;  //正确
```

**set的迭代器是const的**

### 添加元素

```C++
vector<int> ivec = {2,4,6,8};
set<int> set2;
set.insert(ivec.cbegin(), ivec.cend());
set.insert({1,3,5,7});  //不重复，现在有8个元素

// 向map添加元素
word_count.insert({word, 1});
word_count.insert(make_pair(word, 1));
word_count.insert(pair<string, size_t>(word, 1));
word_count.insert(map<string, size_t>::value_type(word, 1));
```

对于不包含重复关键字的容器，insert和emplace会返回一个pair，first是个迭代器，指向具有给定关键字的元素，second是个bool，说明是否插入成功。

在multimap和multiset容器上调用insert总会插入一个元素：

```C++
multimap<string, string> authors;
authors.insert({"barth, john", "sot-weed factor"});
authors.insert({"barth, john", "lost in the funhouse"});
```

对允许重复关键字的容器，接受单个元素的insert操作返回一个指向新元素的迭代器，无需bool，因为必插入。

### 删除元素

我们可以通过传递给erase一个迭代器或者一个迭代器对来删除一个元素或者一个元素范围。

**关联容器定义了一个额外的erase，接受一个key_type参数，删除所有匹配给定关键字的元素，返回实际删除的数量。**

对于保存不重复关键字的容器，erase的返回值总是0或1，返回0则表示想要删除的元素不再容器中。

```C++
auto cnt = authors.erase("barth, john");  //cnt == 2
```

### map下标操作

map和unordered_map提供了下标运算符以及at函数，**set不支持下标**。

```C++
map<string, size_t> word_count;
word_count["anna"] = 1;  //插入一个关键字anna
```

**使用一个不存在容器中的关键字作为下标时，会添加一个具有此关键字的元素到map中。**

```C++
c[k];  //返回k的值，如果k不在c中，则创建并值初始化
c.at(k);  //若K不在c中，抛出一个out_of_range异常
```

### 访问元素

```C++
c.find(k);  //返回一个迭代器，指向关键字为k的元素，如果k不在c中，则返回c.end()
c.count(k);  //返回关键字等于k的元素数量。
c.lower_bound(k);  //返回第一个关键字不小于k的元素的迭代器
c.upper_bound(k);  //返回第一个关键字大于k的元素的迭代器
c.equal_range(k);  //返回一个迭代器pair，表示关键字等于k的元素的范围，了两个迭代器分别由lower_bound和upper_bound提供
```

**如果一个multimap或者multiset中多个元素具有给定关键字，则这些元素会相邻存储。**

**如果 lower_bound 和 upper_bound返回相同的迭代器，则给定关键字不在容器中。**

## 无序容器

新标准定义了四个无序容器，不是使用比较运算符来组织元素，而是使用**哈希函数**和关键字类型的==运算符。

无序容器提供了一组管理桶的操作：

![image-20230325162141004](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230325162141004.png)

**标准库为内置类型、string、智能指针类型定义了hash模板，但我们不能直接定义关键字类型为自定义类类型的无序容器。不能直接使用哈希模板，而必须提供我们自己的hash模板版本。**

为了能将Sales_data用作关键字，我们需要提供函数来代替==运算符和哈希值计算函数：

```C++
size_t hasher(const Sales_data &sd){
    return hash<string>()(sd.isbn());  //使用标准库的string类型hash函数计算ISBN的哈希值
}
bool eqop(const Sales_data *lhs, const Sales_data *rhs){
    return lhs.isbn() == rhs.isbn();
}
```

使用上两个函数定义一个元素为自定义类类型的unordered_multiset：

```C++
using SD_multiset = unordered_multiset<Sales_data, decltype(hasher)*, decltype(eqop)*>;

Sd_multiset bookstore(42, hasher, eqop);
```

如果我们的类定义了 == 运算符，则可以只重载哈希函数：

```C++
unoredered_set<Foo, decltype(FooHash)*> fooset(10, FooHash); //Foo定义了==运算符
```

# 动态内存

C++支持动态分配对象，只有当显式被释放时，这些对象才会销毁。C++标准库定义了两个智能指针类型来管理动态分配的对象。当一个对象应该被释放时，指向它的智能指针可以确保自动地释放它。   

**静态内存用于保存局部static对象以及定义在任何函数之外的变量。栈内存用来保存定义在函数内的非static对象。**  

每个程序有一个内存池，被称为**自由空间**或者**堆**。用堆来存储动态分配的对象。 

程序使用动态内存基于以下三种原因之一：

1. 程序不知道自己需要使用多少对象；
2. 程序不知道自己所需对象的准确类型；
3. 程序需要在多个对象间共享数据；

## 动态内存与智能指针

new：在动态内存中为对象分配空间并返回一个指向该对象的指针。

delete：接受一个动态对象的指针，销毁该对象并释放与之关联的内存。

新标准库提供了两种智能指针来管理动态对象，负责自动释放所指向的对象。**shared_ptr允许多个指针指向同一个对象，unique_ptr则独占所指的对象，weak_ptr是一种弱引用，指向shared_ptr所管理的对象。**

这三种类型都定义在**memory头文件**中。

shared_ptr和unique_ptr都支持的操作：

```C++
shared_ptr <T> sp;
unique_ptr<T> up;
p;
*p;
p->mem;
p.get();  //返回p中保存的指针。当智能指针释放了其对象，返回的指针所指向的对象也消失了。
swap(p, q);
p.swap(q);
```

### shared_ptr 类

智能指针也是模板，当我们创建智能指针时，必须提供额外信息：指针可以指向的类型：

```C++
shared_ptr<string> p1;
shared_ptr<list<int>> p2;

if(p1 && p1->empty())
    *p1 = "hi";  //如果P1指向空string，赋“hi”
```

shared_ptr独有的操作：

```C++
make_shared<T>(args);  
shared_ptr<T>p(q);  //p是q的拷贝，会传递q中的计数器，q中的指针必须能转换为T*
p = q;
p.use_count();  //返回与p共享对象的智能指针数量
p.unique();  //若p.use_count()为1，返回true；
```

当要使用make_shared时，同样需要指明类型

```C++
shared_ptr<int> p3 = make_shared<int>(42);
shared_ptr<string> p4 = make_shared<string>(10, '9');  //指向“9999999999”
shared_ptr<int> p3 = make_shared<int>();  //指向0
auto p = make_shared<vector<string>>();
```

make_shared用其参数来**构造**给定类型的对象。如果不传递任何参数，就会进行值初始化。

当进行拷贝和赋值操作时，每个shared_ptr都会记录有多少个其它的shared_ptr指向相同的对象：

```C++
auto p = make_shared<int>(42);
auto q(p);  //p\q指向相同对象，该对象有两个引用者
```

每个shared_ptr都有一个**引用计数**，拷贝一个shared_ptr就会递增。当我们给shared_ptr赋予一个新值或者当shared_ptr被销毁，计数器就会递减：

**一旦计数器变为0，就会自动释放自己所管理的对象**：

```C++
auto r = make_shared<int>(42);
r = q;  //递增q的计数器，递减r的计数器，r原来所指对象没有引用，自动释放
```

shared_ptr的析构函数会递减引用计数，当变为0时，销毁对象并释放内存。

定义名为strBlob的类，这个类中实现多个类共享底层vector:

```C++
class StrBlob{
public:
    typedef vector<string>::size_type size_type;
    StrBlob();
    StrBlob(initializer_list<string> il);
    size type size() const { return data->size();};
    bool empty() const { return data->empty();};
    // 添加删除
    void push_back(const string &t){data->push_back(t)};
    viod pop_back();
    // 元素访问
    string& front();
    string& back();
private:
    shared_ptr<vector<string>> data;
    // 如果data[i]不合法，抛出一个异常
    void check(size_type i, const string &msg) const;
}
```

两个构造函数：

```C++
StrBlob::StrBlob():data(make_shared<vector<string>>()){ }
StrBlob::StrBlob(initializer_list<string> il):make_sared<vector<string>>(il)){ };
```

检查函数：

```C++
void StrBlob::check(size_type i, const string &msg) const{
    if(i >= data->size())
        throw out_of_range(msg);
}
```

strblob使用默认的拷贝、析构、赋值类成员函数来对数据成员进行操作。

### 直接管理内存

使用new动态分配和初始化对象：

```C++
int *pi = new int; 
```

自由空间（堆）中分配的内存是无名的，因此new返回一个指向该对象的指针。

**默认情况下，动态分配的对象是默认初始化的，这也意味着内置类型或者组合类型的对象的值是未定义的，类类型对象将用默认构造函数初始化：**

```C++
int *pi = new int(1024);  
string *ps = new string(10, '9');
vector<int> *pv = new vector<int>{0,1,2,3,4,5,6,7,8};
```

也可以进行值初始化，跟个（）：

```C++
string *ps1 = new string();  //值初始化， 空string
string *ps2 = new string;  //默认初始化， 空string
int *pi1 = new int;  //默认初始化，*pi1值未定义
int *pi2 = new int();  //值初始化      
```

当我们提供括号包围的初始化器，就可以使用auto推断类型，**只有当括号中仅有单一初始化器时才能使用auto：**

```C++
auto p1 = new auto(obj);
auto p2 = new auto{a, b, c};  //错误，括号只能有单个初始化器
```

用new分配const对象是合法的：

```C++
const int *pci = new const int(1024);
```

**动态分配的const对象同样必须进行初始化**

默认情况下，如果new不能分配所要求的内存空间（内存耗尽），会返回一个bad_alloc类型的异常，可以改变使用new的方式阻止：

```C++
int * p1 = new (nathrow) int;  //如果分配失败，new返回一个nullptr
```

bad_alloc 和 nothrow 都定义在头文件new中。

delete表达式用来释放内存，接受一个指针指向我们想要释放的对象：

```C++
delete p;  //p必须指向一个动态分配的指针或者空指针
```

释放非new指针或者将相同指针释放多次都是未定义的。

释放const指针同样delete指针即可：

```C++
const int *pci = new int(1024);
delete pci;  
```

**通过new申请动态必须通过delete显式释放，在显式释放之间该内存空间都会一直被占用，即使作用域结束。不记得释放就会导致内存泄漏。**

坚持使用智能指针！

delete之后。原来指向这个内存地址的所有指针就变成了空悬指针。

```C++
int *p(new int(1024));
auto q = p;
delete p;
p = nullptr;
```

虽然令p = nullptr 了，但是q仍然空悬。

### shared_ptr 和 new 结合使用

我们可以用new返回的指针来初始化智能指针：

```C++
shared_ptr<int> p2(new int(42));
```

由于接受指针参数的智能指针构造函数是explicit的，不能隐式转换，因此我们不能用一个内置指针隐式转化为一个智能指针，必须使用直接初始化：

```C++
shared_ptr<int> p1 = new int(1024);  //错误：必须使用直接初始化形式
shared_ptr<int> p2(new int(1042)); //正确
```

同样：

```C++
shared_ptr<int> clone(int p){
    return new int(p);  //错误，隐式转化为shared_ptr<int>
}
```

改为：

```C++
shared_ptr<int> clone(int p){
    return shared_ptr<int>(new int(p));  
}
```

不要混合使用普通指针和指针指针：

```C++
void process(shared_ptr<int> ptr){/*...*/}
int main(){
    int *x(new int(1024));  //危险！x是普通指针
    process(x);  //错误，不能将普通指针隐式转化
    process(shared_ptr<int>(x));  //正确，但是计数器仅为1，函数调用完成后内存就被释放
    int j = *x;  //错误，x是空悬指针
    return 0;
}
```

智能指针定义了一个get函数，它返回一个内置指针指向只能指针管理的对象。使用get指针不能delete该指针，**该指针用于向无法使用智能指针的代码传递一个内置指针。**

**不要使用get初始化另一个智能指针或者为智能指针赋值**：

```C++
shared_ptr<int> p(new int(42));  //等价于：
shared_ptr<int> p = make_shared<int>(42);
int *q = p.get(); //q是普通指针，不要让它管理的指针被释放
{
    shared_ptr<int>(q);
} //q被销毁，指向的内存被释放！
int foo = *P;  //未定义:p指向的内存已经被释放了
```

我们可以使用reset来将一个新的指针赋值给一个shared_ptr:

```C++
p = new int(1024);  //错误，不允许隐式转换
p.reset(new int(1024));  //正确，p指向一个新对象
```

若p是唯一指向原先对象的shared_ptr，还会释放原先对象。

当p不是唯一指向的智能指针时，我们最好赋值一份拷贝再修改底层值：

```C++
if(!p.unique())
    p.reset(new int(*p));
*p += newVal;
```

### 智能指针和异常

 当发生异常时，我们直接管理的内存是不会被自动释放的，如果使用内置指针管理内存，且在new之后在对应的delete之前发生了异常，则内存不会被释放：

```C++
void f(){
    int *P = new int(42);
    /*这段代码抛出一个异常，且在f中未被捕获*/
    delete ip;  
}
```

如果在f中没有捕获异常，那么内存就永远不会释放了。

 默认情况下，shared_ptr 假定它们指向的是动态内存。因此，当一个shared_ptr被销毁时，它默认地对它管理的指针进行delete操作。

使用自己的释放操作：

```C++
struct connection;
struct destination;
void disconnect(connection);
connection connect(destination*);  

void end_connection(connection *p){ disconnect(*p);}

void f(destination &d){
    connection c = connect(&d);
    shared_ptr<connection> p(&c, end_connection);
}
```

当p被销毁时，它不会对自己保存的指针执行delete，而是会调用end_connection。

### unique_ptr

一个unique_ptr“拥有”它所指向的对象。某个时刻只能有一个unique_ptr指向一个给定对象。当unique_ptr被销毁时，它所指向的对象也会被销毁。

当我们定义一个unique_ptr时，需要将其绑定到一个new返回的指针上,且必须采用直接初始化：

```C++
unique_ptr<double> p1;  //可以指向一个double的unique_ptr
unique_ptr<int> p2(new int(42));  //p2指向一个值为42的int
```

unique_ptr不支持普通的拷贝或者赋值操作：

```C++
unique_ptr<string> p1(new string("stegosaurus"));
unique_ptr<string> p2(p1); //错误，不支持拷贝
unique_ptr<string> p3;
p3 = p2;  //错误，不支持赋值
```

我们可以通过调用release 或者 reset 将指针的所有权从一个（非const）unique_ptr 转移给另一个 unique:

```C++
unique_ptr<string> p2(p1.release());  //release将p1置位空，并返回p1所指对象的地址
unique_prt<string> p3(new string("Trex"));
// 将所有权从p3转移给p2
p2.reset(p3.release());  //reset释放了p2原来指向的内存
```

调用release会切断unique_ptr和它原来管理的对象间的联系。**release返回的指针通常被用来初始化另一个智能指针或给另一个智能指针赋值**，如果我们不用另一个智能指针来保存，则我们的程序就要负责资源的释放：

``` C++
p2.release();  //错误：p2不会释放内存，而且我们丢失指针
auto p = p2.release();  //正确，但我们必须牢记delete(p)
```

不能拷贝unique_ptr有一个例外：我们可以拷贝或赋值一个将要被销毁的unique_ptr：

```C++
// 从函数返回一个unique_ptr
unique_prt<int> clone(int p){
    return unique_ptr<int>(new int(p));
}
// 还可以返回一个局部对象的拷贝
unique_prt<int> clone(int p){
    unique_ptr<int> ret(new int(p))
    return ret;
}
```

重载一个unique_ptr中的删除器必须在尖括号中提供删除器的类型，在创建和reset这种类型的对象时，必须提供一个可调用的删除器对象：

```C++
unique_ptr<objT, delT> p(new objT, fcn);
```

具体例子：

```C++
void f(destination &d){
    connection c = connect(&d);
    unique_ptr<connection, decltype(end_connection)*> p(&c, end_connection);
}
```

### weak_ptr

weak_ptr是一种不控制所指对象生存期的智能指针，它指向一个由shared_ptr管理的对象。将一个weak_ptr绑定到shared_ptr不会改变shared_ptr的引用计数。一旦最后一个指向对象的shared_ptr被销毁，对象就会被释放。

初始化：

```C++
auto p = make_sharedM<int>(42);
weak_ptr<int> wp(p);  //p的引用计数未改变
```

我们不是使用weak_ptr直接访问对象，而必须调用lock：

```C++
if(shared_ptr<int> np = wp.lock()){  // np不为空则条件成立
    /*...*/
}
```

如果weak_ptr所指对象存在，则lock函数返回一个指向共享对象的shared_ptr。

## 动态数组

C++定义了另一种new表达式语法，可以分配并初始化一个数组。标准库中包含了一个名为allocator的类，允许我们将分配和初始化分离。

**大多数应用应该使用标准库容器而不是动态分配的数组，使用容器更为简单、更不容易出现内存管理错误并且可能有更好的性能。**

### new和数组

new分配要求数量的对象并返回第一个对象的指针：

```C++
int *pia = new int[get_size()];  //pia指向第一个int
```

方括号中必须是整型，但不必是常量。同样，也可以用一个表示数组类型的类型别名分配一个数组：

```C++
typedef int arrT[42];
int *p = new arrT;
```

编译器执行如下：

```C++
int *p = new int[42];
```

**分配一个数组会得到一个元素类型的指针，由于不是数组类型，因此不能对动态数组调用begin或者end，同样不能使用范围for处理动态数组的元素**

也就是说动态数组其实并不是数组，仅仅是一个连续内存

默认情况下，new分配的对象都是默认初始化的。也可以对数组中的元素进行值初始化：

```C++
int *pia = new int[10];  //十个未初始化的int
int *pia = new int[10]();  //十个值初始化为0的int
string *psa = new string[10];  //10个空string
string *psa = new string[10]();  //10个空string
```

新标准允许使用花括号列表：

```C++
int *pia3 = new int[10]{0，1，2，3，4，5，6，7，8，9};  
string *psa = new string[10]{"a", "an"};  //剩下的进行值初始化
```

同内置数组一样，初始化器提供的数目小于元素数目，剩余元素会值初始化，**大于元素数目，new表达式失败，不会分配内存**。

**虽然不能创建大小为0的静态数组对象，但动态分配一个空数组是合法的**，可以用任意表达式确定要分配的对象的数目：

```C++
size_t n = get_size();
int *p = new int[n];
for(int* q = p; q != p + n; ++q){/*...*/}

char arr[0];  //错误
char *cp = new char[0];  //正确，但cp不能解引用
```

释放动态数组：

```C++
delete p;  //p必须指向一个动态分配的对象或为空
delete [] pa;  //pa必须指向一个动态分配的数组或为空
```

delete动态数组，数组中的元素按逆序销毁。

标准库提供了一个可以管理new分配的数组的unique_ptr版本：

```C++
unique<int[]> up(new int[10]);
up.release();  //自动使用delete[]销毁其指针
```

可以使用下标运算符访问指向动态数组的unique_ptr:

```C++
for(size_t i = 0; i != 10; ++i)
    up[i] = i;
```

**指向动态数组的unique_ptr不支持成员访问运算符(. and ->)**

shared_ptr不支持直接管理动态数组。如果希望使用shared_ptr管理一个动态数组，必须提供自己定义的删除器：

```C++
shared_ptr<int> sp(new int[], [](int *p){ delete[] p;});
sp.reset();  //使用我们提供的lambda释放数组
```

shared_ptr未定义下标运算符，并且不支持指针的算术运算：

```C++
for(size_t i = 0; i != 10; ++i)
    *(sp.get() + i) = i;  //使用get获取一个内置指针
```

### allocator类

  allocator定义在头文件memory中，它帮助我们分配对象的分配和构造。

类似于vector，allocator是一个模板。为了定义一个allocator对象，我们必须指定分配的对象类型：

```C++
allocator<string> alloc;
auto const p = alloc.allocate(n);  //分配n个未初始化的string
```

allocator分类的内存是未构造的。construct成员函数接受一个指针和**零个**或**多个**额外参数，在给定位置构造**一个元素**：

```C++
auto q = p;  //q指向最后构造的元素之后的位置
alloc.construct(q++);  //*q为空字符串
alloc.construct(q++, 10, 'c');  //*q为cccccccccc
alloc.construct(q++, "hi");  //*q为hi
```

**使用未构造的内存，行为是未定义的。**

当我们用完对象后，必须对每个构造的元素调用destroy来销毁：

```C++
while(q != p)
    alloc.destroy(--q); //释放我们真正构造的string
```

循环开始时，q指向最后构造的元素之后的位置。

**我们只能对真正构造了的元素进行destroy操作**

释放内存通过调用deallocate完成：

```C++
alloc.deallocate(p, n);
```

**传递给deallocate的大小参数必须与调用allocated分配内存时提供的大小操作具有一样的值**

标准库为allocator定义了两个伴随算法：

```C++
uninitialized_copy(b, e, b2);  //从迭代器b到e拷贝元素到迭代器b2所指的未构造的原始内存中，b2指向的内存必须足够大
uninitialized_copy_n(b, n, b2);  //从迭代器b到拷贝n个元素到迭代器b2所指的未构造的原始内存中，b2指向的内存必须足够大
uninitialized_fill(b, e, t);  //用t的拷贝填满b和e指向的原始内存
uninitialized_fill(b, n, t);  //用t的拷贝从迭代器b指向的内存开始创建n个对象
```

例子：

```C++
vector<string> vi;  
auto p = alloc.allocate(vi.size() * 2);
auto q = uninitialized_copy(vi.begin(), vi.end(), p);  //q返回拷贝完成后的第一个空原始内存
uninitialized_fill_n(q, vi.size(), 42);  //将剩余元素初始化为42
```

## 使用标准库：文本查询程序

实现一个简单的文本查询程序，允许用户在给定文件中查询单词，查询结果是：单词在文件中出现的次数以及其所在行的列表，如果单词在一行中出现多次，此行只列出一次。行会升序输出。

### 文本查询程序设计

需要完成的任务：

1）当程序读取输入文件时，必须记住单词出现的每一行。即逐行读取，并分词

2）当程序输出时：

​	~ 提取每个单词所关联的行号；

​	~ 行号必须升序且不重复；

​	~ 必须打印给定行号的文本；

从定义一个保存输入文件的类开始，这个类包含一个vector和一个map，vector的每个元素保存每行输入文本，map用来关联每个单词和它行号的set。显然，这个类还需要一个用来读取给定输入文件的构造函数和一个执行查询的操作。

定义另一个类用来保存查询结果，完成打印工作。

**在定义类之前，先定义一个使用该类的方法**：

```C++
void runQueries(ifstream &infile){
    TextQuery tq(infile);
    while(true){
        cout << "enter word to look for, or q to quit: ";
        string s;
        if(!(cin >> s)  || s == "q") break;
        print(cout, tq.query(s))  << endl;
    }
}
```

 定义TextQuery 和 QueryResult 类：

```C++
class QueryResult{
friend ostream& print(ostream&, const QueryResult&);
public:
    QueryResult() = default;
    QueryResult(string s, shared_ptr<set<int>> p , shared_ptr<vector<string>> f):sought(s), lines(p), file(f){}
private:
    string sought;
    shared_ptr<vector<string>> file;
    shared_ptr<set<int>> lines;
};
class TextQuery{
public:
    friend ostream& print(ostream& os, set<int> map_set);
    TextQuery() = default;
    TextQuery(ifstream &infile):file(new vector<string>){
        string text;
        while(getline(infile, text)){
            file->push_back(text);
            int n = file->size() - 1;
            istringstream line(text);
            string word;
            while(line >> word){
                auto &lines = wm[word];  //map[args]如果不存在args会创建一个并默认初始化
                if(!lines)
                    lines.reset(new set<int>);
                lines->insert(n);
            }
        }
    }
    QueryResult query(const string &s) const{
        static shared_ptr<set<int>> nodata(new set<int>>);
        auto loc = wm.find(s);
        if(loc == wm.end())
            return QueryResult(s, nodata, file);
        else
            return QueryResult(s, loc->second, file);
    }
   
private:
    shared_ptr<vector<string>> file;
    map<string, shared_ptr<set<int>>> word_map;
};
```

定义函数：

```
ostream& print(ostream& os, const QueryResult& qr){
	os << qr.sought << "occurs" << qr.lines->size() << " " << make_plural(qr.lines->size()), "time", "s") << endl;
	for(auto num : *qr.lines)
		os << "\t(line " << num + 1 << ") " 
		<< *(qr.file->begin() + num) << endl;
	return os;
}
```

# 拷贝控制

## 拷贝、赋值、销毁

### 拷贝构造函数

如果一个构造函数的**第一个参数是自身类类型的引用**，**且任何额外参数都有默认值**，则此构造函数是拷贝构造函数：

```C++
class Foo{
public:
    Foo();  //默认构造函数
    Foo(const Foo&);  //拷贝构造函数
};
```

拷贝构造函数经常会被隐式调用，因此通常不应该是explicit的。

如果我们没有为一个类定义拷贝构造，编译器会为我们定义一个。**即使我们定义了其它的构造函数，编译器也会为我们合成一个拷贝构造函数。**合成的拷贝构造函数会从给定对象中依次将每个非static成员拷贝到正在创建的对象中。

拷贝初始化和直接初始化：

```C++
string dots(10, '.');  //直接初始化
string s(dots);  //直接初始化(调用拷贝构造函数)
string s2 = dots;  //拷贝初始化
string null_book = "9-999-99999-9";  //拷贝初始化
string nines = string(100, '9');  //拷贝初始化
```

当我们使用拷贝初始化时，我们要求编译器将右侧运算对象拷贝到正在创建的对象中。

**除了使用=定义变量，下列情况也会发生拷贝初始化：**

1）将一个对象作为实参传递给一个非引用类型的形参

2）从一个返回类型为非引用的函数返回一个对象

3）用花括号列表初始化一个数组中的元素或一个聚合类中的成员

编译器被允许绕过拷贝构造函数：

```C++
string null_book = " 9-999-99999-9";
```

编译器可以将该代码改写为：

```C++
string null_book(" 9-999-99999-9");
```

### 拷贝赋值运算符

类可以控制其对象如何赋值：

```C++
Sales_data trans, accum;
trans = accum;  // 使用拷贝赋值运算符
```

**同样，如果类未定义自己的拷贝赋值运算符，编译器会为它合成一个。**

重载运算符的参数表示运算符的运算对象。某些运算符，包括赋值运算符，必须定义为成员函数。

**如果一个运算符是一个成员函数，其左侧运算对象就绑定到this参数。对于一个二元运算符，其右侧运算对象作为显式参数传递。**

```C++
class Sales_data{
public:
    Sales_data& operator=(const Sales_data&);
};

Sales_data& Sales_data::operator=(const Sales_data& rhs){
    bookNo = rhs.bookNo;  //调用string::operator=
    units_sold = rhs.units_sold;
    return *this; 
}
```

为了与内置的赋值保持一致，赋值运算符通常返回一个指向其左侧对象的引用。

### 析构函数

析构函数释放对象所使用的资源，并销毁对象的非static数据成员。

析构函数没有返回值，也不接受参数：

```C++
class Foo{
public:
    ~Foo();
};
```

显然，析构函数无法被重载。**对于一个给定类，只会有唯一一个析构函数。**

在一个构造函数中，成员的初始化是在函数体执行之前完成的，且按照类中出现的顺序进行初始化。**而在一个析构函数中，首先执行函数体，然后销毁成员，成员按初始化的逆序销毁。**

**与普通指针不同，智能指针是类类型，因此具有析构函数，智能指针的成员在析构阶段会被自动销毁。**

调用析构的场景：

1）变量离开作用域时被销毁。

2）当一个对象被销毁时，其成员被销毁。

3）容器被销毁时，其元素被销毁。

4）对于动态分配的对象，当对指向它的指针应用delete运算符时被销毁。

5）对于临时对象，当创建它的完整表达式结束时被销毁。

当一个类未定义自己的析构函数时，编译器会为它定义一个合成析构函数。**析构函数本身并不直接销毁成员，成员在析构函数体之后隐含的析构阶段中被销毁。**

### 三/五法则

如果一个类需要一个析构函数，则几乎可以肯定它也需要一个拷贝构造函数和一个拷贝赋值函数。合成的析构函数不会delete一个指针数据成员，因此我们需要定义一个析构函数来释放构造函数分配的内存：

```C++
class HasPtr{
public:
    HasPtr(const string &s = string()):ps(new string(s)),i(0){}
    ~HasPtr(){delete ps;}
    // 错误：HasPtr需要一个拷贝构造函数和一个拷贝赋值函数
};
```

因为我们的类成员有内置指针，**当我们使用合成的拷贝构造函数和合成的拷贝赋值函数时，这些函数会简单的拷贝指针成员，也就意味着多个HasPtr对象可能指向相同的内存：**

```C++
HasPtr f(HasPtr hp){
    HasPtr ret = hp;
    return ret;  // ret和hp被销毁
}
```

此代码会导致同一个指针被delete两次。此外，f的调用者还会使用传递给f的对象：

```C++
HasPtr p("some values");
f(p);  // 当f结束，p.ps指向的内存被释放
HasPtr q(p); // 现在p和q都指向无效内存
```

### 使用default

将拷贝控制成员定义为default来显式地要求编译器生成合成的版本：

```C++
class Sales_data{
pubilc:
    Sales_data() = default;
    Sales_data(const Sales_data&) = default;
    Sales_data operator=(const Sales_data&);
    ~Sales_data() = default;
};
```

### 阻止拷贝

定义类时采取某些机制阻止拷贝或者赋值。如：iostream阻止拷贝以避免多个对象写入或读取相同的IO缓冲。

新标准允许我们将拷贝构造函数和拷贝赋值运算符定义为**删除的函数**来阻止拷贝。

我们虽然声明了删除的函数，**但不能以任何方式使用它们， 在函数的参数列表后面加上=delete来指出我们希望将它定义为删除的：**

```C++
struct NoCopy{
    NoCopy() = default;
    NoCopy(const NoCopy&) = delete;
    NoCopy &operator=(const NoCopy&) = delete;
    ~NoCopy() = default;  
};
```

=delete通知编译器我们不希望定义这些成员。**与=default不同，我们可以对任何函数指定=delete。**

**析构函数不能是删除的成员，如果一个类的析构是删除的，则该类不能被销毁**

**如果一个类的数据成员不能默认构造、拷贝、赋值或者销毁，则对应的合成成员函数将被编译器定义为删除的，比如：**

类的某个成员的析构函数是删除或者不可访问的，则对应的析构函数是被定义为删除的；类的某个成员是const的或者引用类型，则类的合成拷贝赋值运算符被定义为删除的；

**声明但不定义一个成员函数是合法的。试图访问一个未定义的成员会导致一个链接时错误。**

## 拷贝控制和资源管理

### 行为像值的类

类值版本的HasPtr:

```C++
class HasPtr{
public:
    HasPtr() = default;
    HasPtr(const string &s = string()):ps(new string(s)), i(0){}
    HasPtr(const HasPtr &p):ps(new string(*p.ps), i(p.i)){}
    HasPtr& operator=(const HasPtr&);
    ~HasPtr(){delete ps;}
private:
    string *ps;
    int i;
};

// 赋值运算符通常会销毁左侧运算对象的资源并从右侧运算对象拷贝数据
HasPtr& HasPtr::operator=(const HasPtr& rhs){
    auto newp = new string(*rhs.ps);
    delete ps;  //释放旧内存
    ps = newp;
    i = rhs.i;
    return *this;
}
```

**当编写赋值运算符时要注意：1）一个对象赋予它自身时必须能正确工作；2）大多数赋值运算符组合了析构函数和拷构造贝函数的工作。**

### 行为像指针的类

最好的方法是使用shared_ptr管理类内资源。但如果我们想直接管理资源，可以使用**引用计数**。

```C++
class HasPtr{
public:
    HasPtr() = default;
    HasPtr(const string &s = string()):ps(new string(s)), i(0), use(new size_t(1){}
    HasPtr(const HasPtr &p):ps(p.ps), i(p.i), use(p.use){++*use;}
    HasPtr& operator=(const HasPtr&);
    ~HasPtr(){}
private:
    string *ps;
    int i;
    size_t *use;
}; 
HasPtr::~HasPtr(){
    if(--*use == 0){
        delete ps;
        delete use;
    }
}
HasPtr& operator=(const HasPtr& rhs){
    ++*rhs.use;
    if(--*use == 0){
        delete ps;
        delete use;
    }
    ps = rhs.ps;
    i = rhs.i;
    use = rhs.use;
    return *this;
}
```

## 交换操作

如果一个类定义了自己的swap，那么算法将使用类自定义的版本，否则，算法将使用标准库定义的swap。

```C++
class HasPtr{
friend void swap(HasPtr&, HasPtr&);
public:
}；
inline void swap(HasPtr &lhs, HasPtr *rhs){
    using swap;
    swap(lhs.ps, rhs.ps);
    swap(lhs.i, rhs.i);
}
```

**如果存在类型特定的swap版本，则匹配成都会由于std中定义的版本。**

定义了swap后可以用于优化赋值运算符：

```C++
HasPtr& operator=(HasPtr rhs){  //形参是非引用！
    swap(*this, rhs);
    ++*use;
    return *this;
}
```

## 拷贝控制示例

![image-20230404113129495](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230404113129495.png)

```C++
class Massage{
friend class Folder;
public:
    // folders 被隐式初始化为空集合
    explicit Message(const string &str = ""):contents(str){}
    // 拷贝控制成员，用来管理指向本Message的指针
    Message(const Message&);
    Message& operator=(const Message&);
    ~Message();
    // 从给定Folder集合中添加、删除本Message
    void save(Folder&);
    void remove(Folder&);
private:
    string contents;
    set<Folder*> folders;
    // 拷贝构造函数、拷贝赋值运算符和析构函数所使用的工具函数
    // 将本Message添加到指向参数的Folder中
    void add_to_Folders(const Message&);
    // 从folders 中的每个Folder中删除本Message
    void remove_from_Folders();
};
```

由于接受一个参数的构造函数有默认实参，因此也被当作默认构造函数。

```C++
void Message::save(Folder &f){
    folders.insert(&f);
    f.addMsg(this);
}
void Message::remove(Folder &f){
    folders.erase(&f);
    f.remMsg(this);
}
```

当我们拷贝一个Message时，得到的副本应该与原Message出现在相同的Folder中：

```C++
void Message::add_to_Folders(const Message &m){
    for(auto f : m.folders)
        f->addMsg(this);
}

Message::Message(const Message &m):contents(m.contens), folders(m.folders){
    add_to_Folders(m);
}
```

当一个Message被销毁时，我们必须从指向此Message的Folder中删除它：

```C++
void Message::remove_from_Folders(){
    for(auto f : m.folders)
        f->remMsg(this);
}

Message::~Message(){
    remove_from_Folders();
}
```

拷贝运算符要处理好自赋值的情况：

```C++
Message& Message::operator=(const Message &rhs){
    remove_from_Folders();
    contents = rhs.contents;
    folders = rhs.folders;
    add_to_Folders(rhs);
    return *this;
}
```

定义Message的swap:

```C++
void swap(Message &lhs, Message &rhs){
    using std::swap;
    for(auto f : lhs.folders)
        f->remMsg(&lhs);
    for(auto f : rhs.folders)
        f->remMsg(&rhs);
    
    swap(lhs.folders, rhs.folders);
    swap(lhs.contents, rhs.contents);
    
    for(auto f : lhs.folders)
        f->addMsg(&lhs);
    for(auto f : rhs.folders)
        f->addMsg(&rhs);
}
```

## 动态内存管理类

某些类需要在运行时分配可变大小的内存空间。实现一个vector<string>类：

使用一个allocator来获得原始内存，每个StrVec有三个指针成员指向元素所使用的内存：

1）elements，指向分配的内存中的首元素；

2）first_free，指向最后一个实际元素之后的位置；

3）cap，指向分配内存末尾之后的位置。

我们的类还需要四个工具函数：

1）alloc_n_copy会分配内存，并拷贝一个给定范围中的元素；

2）free会销毁构造的元素并释放内存；

3）chk_n_alloc保证StrVec至少有容纳一个新元素的空间。如果没有空间则调用reallocate分配更多空间；

4）reallocate在内存用完时为StrVec分配新内存；

```C++
class StrVec{
public:
    StrVec():elements(nullptr), first_free(nullptr), cap(nullptr){}  // 默认构造
    StrVec(const StrVec&);  // 拷贝构造
    StrVec& operator=(const StrVec&);  // 拷贝赋值
    ~StrVec();
    void push_back(const string&);
    size_t size() const { return first_free - elements; }
    size_t capcity() const { return cap - elements; }
    string *begin() const { return elements; }
    string *end() const { return first_free;}
    //...
private:
    Static alloctor<string> alloc;
    void chk_n_alloc(){if (size == capacity()) reallocate();}
    // 工具函数
    pair<string*,string*> alloc_n_copy(const string*, const string*);  // 复制迭代器范围内的元素并返回新的迭代器pair
    void free();  // 销毁元素、释放内存
    void reallocate();  // 获取更多内存并拷贝已有元素
    string *elements;  // 内存首元素指针
    string *first_free;  // 内存第一个空闲元素指针
    string *cap;  // 内存尾后元素指针
};
```

push_back函数：

```C++
void StrVec::push_back(const string& s){
    chk_n_alloc();
    // 在first_free指向的元素中构造s的副本
    alloc.construct(first_free++, s);
}
```

**当我们使用allocator分配内存时，这些内存是未构造的，为了使用，我们必须调用construsct。**

alloc_n_copy成员会分配足够的内存保存给定范围内的元素，并且返回一个指针的pair指向新空间的开始和尾后：

```C++
pair<string*, string*> StrVec::alloc_n_copy(const string *b, const string *e){
    auto data = alloc.allocate(e - b);
    return {data, uninitialized_copy(b, e, data)};
}
```

free负责destroy元素并且释放StrVec自己分配的内存空间：

```C++
void StrVec::free(){
    if(elements){  // 不能传空指针给deallocate
        for(auto p = first_free; p != elements; )
            alloc.destroy(p--);
        alloc.deallocate(elements, cap - elements);
    }
}
```

destroy运行string的析构函数释放自己分配的内存空间。当元素被销毁之后，就可以调用deallocate释放给StrVec分配的内存空间。

拷贝控制成员：

```C++
StrVec::StrVec(const StrVec &s){
    auto newdata = alloc_n_copy(s.begin(), s.end());
    elements = newdata.first;
    first_free = cap = newdata.second;
}

StrVec::~StrVec(){ free();}

StrVec &StrVec::operator=(const StrVec &rhs){
    auto data = alloc_n_copy(rhs.begin(), rhs.end());
    free();
    element = data.first();
    first_free = cap = data.second;
    return *this;
}
```

**reallocate使用移动构造函数将对象移动而不是拷贝到正在创建的对象。当reallocate在新内存中构造string时，必须调用move表示希望使用string的移动构造函数：**

```c++
void StrVec::reallocate(){
    auto newcapacity = size() ? 2 * size() : 1;
    auto newdata = alloc.allocate(newcapacity);
    //将数据从旧内存移到新内存
    auto dest = newdata;
    auto elem = elements;
    for(size_t i = 0; i != size(); i++){
        alloc.construct(dest++, std::move(*elem++));
    }
    free();
    elements = newdata;
    first_free = dest;
    cap = element + newcapacity;
}
```

**调用move返回的结果会令construct使用string的移动构造函数。**

## 对象移动

新标准库给与了可以移动而非拷贝对象的能力。1）可以增强性能；2）有些类不支持拷贝，比如IO，比如unique_ptr。

### 右值引用

必须绑定在右值的引用称为右值引用，**通过&&而不是&获得右值引用，右值引用只能绑定到一个将要销毁的对象上。**

我们不能将常规的引用绑定到要求转换的表达式、字面常量或者返回右值的表达式上。但我们可以将一个右值引用绑定到这类表达式上，却不能直接绑定到左值上：

```C++
int i = 42;
int &r = i;   // r引用i
int &&rr = i;  // 错误，不能将右值引用绑定到左值
int &r2 = i * 42;  // 错误，i * 42 是右值
const int &r3 = i * 42;  // 正确，可以将const 的引用绑定到右值
int &&rr2 = i * 42;  // 正确
```

****

左值持久：右值短暂。**变量表达式都是左值，因此我们不能将右值引用绑定到一个右值引用类型上：**

```C++
int &&rr1 = 42;  // 正确
int &&rr2 = rr1;  // 错误
```

****

虽然不能将右值引用绑定至左值，但我们可以显式地将左值转换为对应右值的引用类型：

```C++
int &&rr3 = std::move(rr1);  // ok
```

move函数返回给定对象的右值引用。调用move之后：我们要保证不再使用调用它的参数（这里是rr1）。**我们可以销毁移后源对象（rr1），也可以赋予它新值，但不能使用一个移后源对象的值。**

### 移动构造函数和移动赋值运算符

类似拷贝函数，移动构造函数的第一个参数是该类的一个引用。但这个引用参数在移动构造函数中是一个右值引用。与拷贝构造函数一样，任何额外的参数都必须有默认实参。

**移动构造函数必须保证移动后源对象被销毁是无害的，且源对象必须不再指向被移动的资源。**

```C++
StrVec::StrVec(StrVec &&s) noexcept:elements(s.elements), first_free(s.first_free), cap(s.cap){
    // 让s可以安全运行析构函数
    s.elements = s.first_free = s.cap = nullptr;
}
```

移动构造函数不分配任何新内存：它接管给定的StrVec中的内存。（就是给这块内存换个主人 = =）

noexcept 通知标准库：该函数不会抛出异常

**不抛出异常的移动构造函数和移动赋值运算符必须标记为noexcept。（防止移动一半抛出异常不上不下改变原有对象）**

****

移动赋值运算符执行与析构函数和移动构造函数相同的工作。同样，如果移动赋值运算符不抛出异常，我们要将其标记为noexcept的：

```c++
StrVec &StrVec::opertor=(StrVec &&rhs) noexcept{
    // 确保自赋值
    if(*this != rhs){
        free();
        elements = rhs.elements;  //接管而不是拷贝
        first_free = rhs.first_free;
        cap = rhs.cap;
        rhs.elements = rhs.first_free = rhs.cap = nullptr;
    }
    return *this;
}
```

**必须确保移动后源对象可析构且有效（赋予新值或安全使用）**

****

编译器同样会合成移动构造函数和移动赋值运算符。**只有当一个类没有定义任何自己版本的拷贝控制成员。且类的每个非static数据成员都可以移动时，编译器才会为它合成移动构造函数或移动赋值运算符。如果一个成员是类类型，且该类有对应的移动操作，编译器也能移动该成员：**

```C++
struct X{  // 编译器会为X合成移动操作
  int i;  // 内置类型可移动  
  string s;  // string定义了自己的移动操作
};

struct hasX{
  X mem;  // X有合成的移动操作
};

X x, x2 = move(x);  // 使用合成的移动构造函数
hasX hx, hx2 = move(hx);   // 使用合成的移动构造函数
```

与拷贝操作不同，移动操作永远不会隐式定义为删除的函数。但如果我们显式地将移动操作生成=default的，且编译器不能移动所有的成员，则编译器会将移动操作定义为删除的函数。

> 1）当类成员定义了自己的拷贝构造函数且未定义移动构造函数；或者类成员未定义拷贝构造函数且编译器不能合成移动构造函数时，则类的移动构造函数会被定义为删除的。
>
> 2）当类成员的移动构造或者移动赋值函数被定义为删除的或者private的，则类的移动构造或移动赋值被定义为删除的。
>
> 3）如果类的析构函数被定义为删除的或者不可访问的，则类的移动构造函数被定义为删除的。
>
> 4）类成员是const或者是引用，则类的移动赋值运算符被定义为删除的。

如：

```C++
//假设y定义了拷贝构造，但没有定义自己的移动构造函数
struct hasY{
public:
    hasY() = default;
    hasY(hasY &&) = default;
    Y mem;  // hasY将有一个删除的移动构造函数
};
hasY hy, hy2 = std::move(hy);  // 错误：移动构造函数是删除的
```

**如果类定义了一个移动构造函数和/或定义了一个移动赋值运算符，则该类的合成拷贝构造函数和拷贝赋值运算符会被定义为删除的。**

****

类遵循移动右值，拷贝左值的规则，即当一个类既有拷贝构造又有移动构造函数时，会根据参数是左值还是右值决定调用哪个构造函数。

**当类没有移动构造函数时，右值也会被拷贝。**

****

一个赋值运算符实现拷贝和移动两种功能：

```C++
class HasPtr{
public:
    HasPtr(HasPtr & p): ps(p.ps), i(p.i){}
    HasPtr(HasPtr & p) noexcept: ps(p.ps), i(p.i){ p.ps = 0; }
    HasPtr& operator=(HasPtr rhs){  // 非引用实参
        swap(*this, rhs);
        return *this;
    }
};

hp = hp2;  // hp2是一个左值，通过拷贝构造函数来拷贝
hp = std::move(hp2);  // 移动构造函数移动hp2
```

**如果类定义了任意一个拷贝操作，那就应该定义所有的五个操作。**

****

新标准库定义了一种**移动迭代器**适配器。一个移动迭代器通过改变给定迭代器的解引用运算符的行为来适配此迭代器，**即，移动迭代器解引用运算符生成一个右值引用。**

通过调用标准库的 make_move_iterator 函数将普通迭代器转换为一个移动迭代器。移动迭代器支持所有正常的迭代器操作(比如将一对移动迭代器传递给算法)，可以将移动迭代器传递给uninitialuzed_copy:

```C++
void StrVec::reallocate(){
    auto newcapacity = size() ? 2 * size() : 1;
    auto newdata = alloc.allocate(newcapacity);
    //将数据从旧内存移到新内存
    auto dest = newdata;
    auto elem = elements;
    for(size_t i = 0; i != size(); i++){
        alloc.construct(dest++, std::move(*elem++));
    }
    free();
    elements = newdata;
    first_free = dest;
    cap = element + newcapacity;
}

// StrVec::reallocate()的替代
void StrVec::reallocate(){
    auto newcapacity = size() ? 2 * size() : 1;
    auto first = alloc.allocate(newcapacity);
    auto last = uninitialized_copy(make_move_iterator(begin()), make_move_iterator(end()), first);
    free();
    elements = first;
    first_free = last;
    cap = elements + newcapacity;
}
```

uninitialized_copy 会对输入序列中的每个元素调用construct将元素“拷贝”，当我们传递移动迭代器后，解引用生成的是一个右值引用，因此construct将会使用移动构造函数来构造元素。

### 右值引用和成员函数

可以让类的成员函数同时提供拷贝和移动两个版本，这样就能从中受益。比如标准库对push_back就有如下重载：

```C++
void push_back(const X&);
void push_back(X&&);
```

****

向右值赋值，或者在右值对象上调用函数，是被允许的：

```C++
auto n = (s1 + s2).find('a');  // 正确
s1 + s2 = "wow!";  // 正确
```

而有时候我们希望自己的类能阻止这种做法，强制左值运算对象是一个左值，我们可以通过在参数列表后放置一个**引用限定符**：

```C++
class Foo{
public:
    Foo &operator=(const Foo&) &;  //只能向可修改的左值赋值
};

Foo &Foo::operator=(const Foo &rhs) &{
    return *this;
}
```

**引用限定符**可以是 & 或者 &&，分别指出this可以指向一个左值或者右值。类似于const限定符，引用限定符只能用于非static的成员函数，且必须同时出现在函数的声明和定义中。

一个函数可以同时用const和引用限定。在这种情况下，引用限定符必须跟随在const限定符之后：

```C++
class Foo{
public:
    Foo someMem() & const;  // 错误
    Foo anotherMem() const &;  // 正确
};
```

****

可以通过引用限定符和const限定符区分函数的重载：

```C++
class Foo{
public:
    Foo sorted() &&;
    Foo sorted() const &;
private:
    vector<int> data;
};
// 本对象为右值，可以原址排序
Foo Foo::sorted() &&{
    sort(data.begin(), data.end());
    return *this;
}

// 本对象是一个const 或者是一个左值，哪种情况都不能对其进行原址排序
Foo Foo::sorted() const &{
    Foo ret(*this);
    sort(ret.data.begin(), ret.data.end());
    return ret;
}
```

**当对一个const右值或者一个左值执行sorted时，我们不能改变对象，因为不知道是否有其他用户仍需使用原数值。因此在排序前就需要拷贝。**

如果我们定义了两个或两个以上具有相同名字和参数列表的成员函数，就必须对所有函数的都加上引用限定符，或者所有都不加。

# 重载运算和类型转换

当运算符作用于类类型的运算对象时，可以通过运算符重载重新 定义该运算符的含义。

## 基本概念

重载运算符的参数数量和该运算符作用的运算对象一样多。除了重载的函数调用符operator()之外，其它的重载运算符不能含有默认实参。

**如果一个运算符函数是成员函数，则它的第一个运算对象绑定到隐式的this指针上。**

**对于一个运算符来说，它或者是类的成员，或者至少含有一个类类型的对象：**

```C++
int operator+(int, int);  // 错误，不能为int重定义内置的运算符
```

只能重载原有运算符，不能发明新的运算符号。有四个符号（+，-，*，&）既是一元又是二元运算符，所有这些运算符都能被重载。

不能重载的运算符：

```C++
::     .*     .     ?=
```

我们能将运算符作用域类型正确的实参间接调用，也能直接调用：

```C++
data1 + data2;
operator+(data1, data2);
```

调用成员函数的方式调用：

```C++
data1 += data2;
data1.operator+=(data2);
```

**通常情况下，不应该重载逗号、取地址、逻辑与或运算符**

****

当我们把运算符定义成成员函数时，它的左侧运算对象必须是运算符所属类的一个对象：

```C++
string s = "world";
string t = s + "!";  // 正确
string u = "hi" + s;  // 如果operator+是string的成员函数，则错误
```

## 输入和输出运算符

### 重载输出运算符<<

通常情况下，<<运算符的第一个形参是非常量的ostream对象的引用，第二个形参一般是一个常量的引用，该常量是我们想要打印的类类型。**因此，为了与其它输出运算符保持一致，opetator<<一半返回它的ostream形参：**

```C++
ostream &operator<<(ostream &os, const Sales_data &item){
    os << item.isbn()  << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();
    return os;
} 
```

**输出运算符尽量减少格式化操作，比如换行符**

**输入输出运算符必须是非成员函数，IO运算符通常需要读写类的非公有成员，因此一般声明为友元**

### 重载输入运算符>>

通常情况下，>>运算符的第一个形参是运算符将要读取的流的引用，第二个形参是将要读入到的非常量对象的引用。该运算符通常返回某个给定流的引用：

```C++
istream &operator>>(istream &is, Sales_data &item){
    double price;
    is >> item.bookNo >> item.units_sold >> price;
    if(is)  // 检查输入是否成功
        item.revenue = item.units_sold * price;
    else
        item = Sales_data();  // 输入失败，对象被赋予默认状态
    return is;
}
```

**输入运算符必须处理出入失败的情况，而输出运算符不需要。**

****

当执行驶入运算符时可能发生如下错误：

1）当流含有错误类型的数据时读取操作可能失败。

2）当读取操作到达文件末尾或者遇到输入流的其他错误也会失败。

**当读取操作发生错误时，输入运算符应该负责从错误中恢复**

## 算术和关系运算符

通常情况下，我们把算术和关系运算符定义为非成员函数以允许对左侧或右侧的运算对象进行转换。**大部分算术和关系运算符的形参都是常量引用**。

如果类定义了算术运算符，则它一般也会定义一个对应的复合赋值运算符，最有效的办法是使用复合赋值运算符定义算术运算符：

```C++
Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs){
    Sales_data sum = lhs;
    sum += rhs;  //已经定义的复合运算符重载
    return sum;
}
```

### 相等运算符

通常情况下，相等运算符具有传递性，即 a==b，b==c 则 a==c。如果类定义了operator==，则也应该定义operator!=。

**定义了一个，另一个使用定义的就可以了，比如定义了相等运算符，不等直接if(相等）else 就可以了**

### 关系运算符

定义了相等关系的类也常常包含关系运算符。定义关系运算符应该：

1）定义顺序关系，令其与关联容器中对关键字的要求一致；

2）如果类同时也含有==运算符，应该定义一种关系令其与==保持一致。**如果两个对象是!=的，则肯定有一个小于另一个。**

## 赋值运算符

除了拷贝赋值与移动赋值运算符之外，类还可以定义其它赋值运算符以使用别的类型作为右侧运算对象：

```C++
vector<string> v;
v = {"a", "an", "the"};  //右侧对象为花括号列表元素
```

定义StrVec的额外赋值运算符：

```C++
StrVec &StrVec::operator=(initialize_list<string> li){
    auto data = alloc_n_copy(il.begin(), il.end());
    free();
    elements = data.first;
    first_free = cap = data.second;
    return *this.
}
```

**不论形参是什么，赋值运算符都必须定义为成员函数**

## 下标运算符

表示容器的类通常可以通过元素在容器中的位置访问元素，这些类通常会定义下标运算符operator[ ] 。**下标运算符必须是成员函数。**

下标运算符通常以所访问的元素的引用作为返回值，我们最好同时定义下标运算符的常量版本和非常量版本：

```C++
class StrVec{
public:
    string& operator[](size_t n ){ return elements[n]};
    const string& operator[](size_t n ) const { return elements[n]};
    
private:
    string *elements;
};

StrVec svec;
const StrVec cevc = svec;
svec[0] == "aa";  // 正确
cvec[0] == "aa";  // 错误，返回的是常量引用
```

## 递增递减运算符

不要求一定得是类成员函数，但改变的正好是操作对象的状态，因此最好将其设定为成员函数。

**定义递增和递减运算符的类应该同时定义前置和后置两个版本。**

```C++
class StrBlobPtr{
public:  
    StrBlobPtr& operator++();  // 前置
    StrBlobPtr& operator--();  // 返回本身的引用
};

StrBlobPtr& StrBlobPtr::operator++(){
    check(curr, "increment past end of StrBlobPtr");
    ++curr;  //check检查curr的状态，当curr指向vector末尾时抛出异常
    return *this;
}

StrBlobPtr& StrBlobPtr::operator--(){
     --curr;  //check检查curr的状态，当curr等于0时，递减会产生无效下标
    check(curr, "decrement past begin of StrBlobPtr");
    return *this;
}
```

后置版本接受一个额外的（但不被使用）的int类型的形参。当我们使用后置版本，编译器为这个形参提供一个值为0的实参。

```C++
class StrBlobPtr{
public:  
    StrBlobPtr operator++(int);  // 后置
    StrBlobPtr operator--(int);  // 返回拷贝
};

StrBlobPtr StrBlobPtr::operator++(int){  
    StrBlobPtr ret = *this;  //后置无需检查，
    ++*this;  //已经定义了，直接用前置版本
    return ret;
}

StrBlobPtr StrBlobPtr::operator--(int){
    StrBlobPtr ret = *this;  //后置无需检查，
    --*this;  //已经定义了，直接用前置版本
    return ret;
}
```

**因为我们不会用到int形参，所以无需命名。**

显式调用：

```C++
StrBlobPtr p(a);
p.operator++();  // 前置
p.operator++(0);  // 后置
```

## 成员访问运算符

迭代器和智能指针常用到解引用（*）和箭头（->）运算符：

```C++
class StrBlobPtr{
public:
    string& operator*() const {
        auto p = check(curr, "dereference past end");
        return (*p)[curr];  //(*p)是对象所指的vector
    }
    
    string& operator->() const {
        // 直接将操作委托给解引用运算符
        return &this->operator*();
    }
};
```

**箭头运算符必须是类的成员，解引用运算符通常也是类的成员，尽管并非必须如此。**

当我们重载箭头时，可以改变的是箭头从哪个对象当中获取成员，而箭头获取成员这一事实本身则永远不变。

对于形如point->mem的表达式来说，point必须是指向类对象的指针或者是一个重载了operator->的类的对象，分别等价于:

```C++
(*point).mem;  //point是一个内置的指针类型
point.operator->() -> mem;  // point是类的一个对象
```

如果point是定义了operator->的一个对象，则我们使用point.operator->()的结果来获取mem。

## 函数调用运算符

当类重载了函数调用运算符，则我们可以像使用函数一样使用该类的对象：

```C++
struct absInt{
    int operator()(int val) const {
        return val < 0 ? -val : val;
    }  
};
```

使用：

```C++
int i = -42;
absInt absObj;
int ui = absObj(i);
```

**函数调用符必须是成员函数，一个类可以定义多个不同版本的调用运算符，相互之间应该在参数数量或类型上有所区别。**

如果类定义了调用运算符，则称该类的对象为**函数对象**。

****

函数对象常常用作泛型算法的实参：

```C++
class PrintString{
public:
    PrintString(ostream &o = cout, char c = " "):os(o), sep(c){ }
    void operator()(const string &s){ os << s << sep; }
private:
    ostream os;
    char sep;
};

// 使用函数对象作为标准库里的for_each算法的实参
for_each(vs.begin(), vs.end(), PrintString(cerr, '\n'));
```

### lambda是函数对象

当我们编写一个lambda后，编译器将该表达式翻译成一个未命名类的未命名对象。例如：

```C++
stable_sort(words.begin(), words.end(), [](const string &a, const string &b) {return a.size() < b.size();});
```

其行为类似于：

```C++
class ShorterString{
public:
    bool operator()(const string &a, const string &b) const {
        return a.size() > b.size();
    }
};
```

默认情况下lambda不能改变捕获的变量，因此默认情况下由lambda产生的类当中的函数调用运算符是一个const成员函数。

用这个类代替：

```C++
stable_sort(words.begin(), words.end(), ShorterString());
```

通过引用捕获的变量不需要在对应类中生成对应的数据成员，而通过值捕获的变量被拷贝到lambda中，因此必须在对应类中建立对应的数据成员：

```C++
auto wc = find_if(words.begin(), words.end(), [sz](const string &a){return a.size() >= sz;});
```

该lambda表达式产生的类形如：

```C++
class SizeComp{
public:
    SizeComp(size_t n): sz(n) { }
    bool operator()(const string &s) const{
        return s.size() >= sz;
    }
private:
    size_t sz;
};
```

这个类不含有默认构造函数，因此使用必须提供一个实参：

```C++
auto wc = find_if(words.begin(), words.end(), SizeComp(sz));
```

**lambda表达式产生的类不含默认构造函数、赋值运算符以及默认析构函数。是否含有默认的拷贝/移动构造函数视捕获类型而定。**

### 标准库定义的函数对象

标准库定义了一组表示算术、关系、逻辑运算符的类，每个类分别定义了一个执行命名操作的调用运算符。

![image-20230407163422359](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230407163422359.png)

这些表示运算符的函数对象类可以用来替换算法中的默认运算符。比如传入一个greater函数对象用于替换sort中默认的operator<排序：

```C++
sort(svec.begin(), svec.end(), greater<string>());
```

**特别注意的是标准库规定其函数对象使用于指针，但比较两个无关指针会产生未定义的行为。**

当我们想要通过比较指针的地址来sort指针的vector:

```C++
vector<string*> nameTable;
// 错误：指针彼此没有联系
sort(nameTable.begin(), nameTable.end(), [](string *a, string *b){ return a < b; });
// 正确：标准库规定的指针的less是良好的
sort(nameTable.begin(), nameTable.end(), less<string*>());
```

### 可调用对象与function

两个不同类型的可调用对象可能共享同一种**调用形式**。调用形式指明了调用返回的类型以及传递给调用的实参类型，一种调用形式对应一个函数类型：

```C++
int(int, int);
```

对应函数类型：接受两个int，返回一个int。当我们想要用map实现一个函数表，表中存放如下可调用对象：

```C++
int add(int i, int j){ return i + j; }
auto mod = [](int i, int j){ return i % j; };
struct divide{
    int operator()(int denominator, int divisor){ return denominator / divisor; }
}
```

以上这些可调用对象都共享同一种调用形式：

```C++
int(int, int);
```

当我们希望使用希望将这些对象加入map:

```C++
map<string, int(*)(int, int)> binops;
binops.insert({"+", add});  // 正确，insert一对pair
binops.insert({"%", mod});  // 错误，mod不是一个函数指针
```

标准库类型function可以解决这个问题，其定义在functional头文件中：

![image-20230407171110177](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230407171110177.png)

function是一个模板，当创建一个function类型时我们必须提供额外信息：

```C++
function<int(int, int)>;  // 表示接受两个int，返回一个int的可调用对象

function<int(int, int)> f1 = add;
function<int(int, int)> f2 = divide();
function<int(int, int)> f3 = [](int i, int j){ return i * j; };
//则我们可以宠幸定义map
map<string, function<int(int, int)>> binops;
```

我们能把所有可调用对象、包括函数指针、lambda或者函数对象在内，都添加到这个map中：

```C++
map<string, function<int(int, int)>> binops = {
    {"+", add},
    {"-", std::minus<int>()},
    {"%", mod}
};
```

一如既往，我们索引map将得到关联值的一个引用：

```C++
binops["+"](10, 5);
binops["%"](10, 5);
```

****

我们不能将重载的函数直接存入function类型的对象中：

```C++
int add(int, int);
Sales_data add(const Sales_data, const Sales_data);
binops.insert({"+", add});  // 错误，哪个add?
```

一种办法是存储函数指针：

```C++
int (*fp)(int, int) = add;
binops.insert({"+", fp});
```

或者使用lambda消除二义性：

```C++
binops.insert({"+", [](int a, int b){return add(a, b);}});
```

## 重载、类型转换和运算符

### 类型转换运算符

我们能够通过定义**类型转换运算符**完成类类型的类型转换，这样的转换有时也被称作**用户定义的类型转换**。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

```C++
operator type() const;
```

type只能是能作为函数返回的类型（比如数组和函数类型就不允许），允许转换成指针（包括数组指针和函数指针）、引用。

**类型转换运算符没有显式的返回类型，也没有形参，且必须定义为类的成员函数。且不允许改变转换对象的内容，因此一般定义为const成员。**

```C++
class SmallInt{
public:
    SmallInt(int i = 0) : val(i){
        if(i < 0 || i > 255)
            throw std::out_of_range("bad smallint value");
    }
    
    operator int() const { return val;}
private:
    std::size_t val;
};

SmallInt si;
si = 4;  // 将4隐式地转换为SmallInt，然后调用SamllInt::operator=
si + 3;  // 将si隐式地转换为int
```

**避免过度使用类型转换函数。**

**定义向bool的类型转换是较为普遍的现象。**

C++11引入了**显式的类型转换运算符：**

```C++
class SmallInt{
public:
    // 编译器不会自动执行
    explicit operator int() const { return val; }
};
```

和显式的构造函数一样，编译器通常不会将一个显式的类型转换运算符用于隐式类型转换：

```C++
SmallInt si = 3;  // 正确：构造函数是隐式的
si + 3;  // 错误：此处需要隐式的类型转换，但explicit
static_cast<int>(si) + 3;  //正确：显式请求类型转换
```

当类型转换运算符是显式的时，我们也能执行类型转换，不过需要通过显式的强制类型转换才行。**但存在例外，当表达式出现在下列位置时，即使是显式的类型转换也会被隐式执行：**

1）if、while以及do语句的条件部分

2）for语句头的条件表达式

3）逻辑与、或、非的运算对象 (&&，|| ，！ )

4）条件运算符的条件表达式（？：）

```C++
while(cin >> value);
```

cin 被 istream operator bool 类型转换函数隐式的执行的转换。

向bool的类型转换通常用在条件部分，因此 operator bool 一般定义为 explicit 的。

### 避免有二义性的类型转换

当类中包含一个或多个类型转换时，必须确保在类类型和目标类型之间只存在唯一一种转换方式。

**通常情况下，不要为类定义相同的类型转换，也不要在类中定义两个及以上转换源或转换目标是算术类型的转换。**

****

实参匹配和相同的类型转换：

```C++
struct B;
struct A{
    A() = default;
    A(const B&);  // 转换构造函数，将B转换为A
};
struct B{
   operator A() const { };  // 类型转换函数，将B转换为A 
};
A f(const A&);
B b;
A a = f(b);  // 二义性错误，到底调用哪个？

//只能显式执行
A a1 = f(b.operator A());
A a1 = f(A(b));
```

二义性与转换目标为内置类型的多重类型转换：

```C++
struct A{
	A(int = 0);  // 最好不要创建两个转换源都是算术类型的类型转换
    A(double);
    operator int() const;
    operator double() const;  // 最好不要创建两个转换源都是算术类型的类型转换
};

void f2(long double);
A a;
f2(a);  // 二义性

long lg;
A a2(lg);  // 二义性
```

**除了显式地向bool的类型转换之外，我们应该尽量避免定义类型转换函数并尽可能地限制那些“显然正确”的非显式构造函数。**

### 函数匹配与重载运算符

重载的运算符也是重载的函数。因此，**通用的函数匹配规则同样适用于判断在给定的表达式中到底应该使用内置运算符还是重载的运算符。**

**同名的成员函数与非成员函数显然不会彼此重载。但当我们在表达式中使用重载的运算符时，无法判断正在使用的是成员函数还是非成员函数：**

```C++
// 对于 a sym b 有两种解释
a.operatorsym(b);
operatorsym(a, b);
```

如果我们对同一个类既提供了转换目标是算术类型的类型转换，也提供了重载的运算符，则同样会遇到二义性问题：

```C++
class SmallInt{
	friend SmallInt& operator+(const SmallInt&, const SmallInt&);
public:  
    SmallInt(int = 0);
    operator int() const { return val; }
private:
    size_t val;
};

SmallInt s1, s2;
SmallInt s3 = s1 + s2;
int i = s3 + 0;  // 二义性，s3转int还是0转smallint
```

# 面向对象程序设计

## OPP概述

面向对象基于三个核心思想：数据抽象、继承和动态绑定。

通过**继承**联系的类：根部有一个**基类**定义所有类共同拥有的成员，从基类继承而来的称为**派生类**，派生类定义各自特有的成员。

对于某些函数，基类希望派生类各自定义适合自身的版本，因此基类将这些函数声明为**虚函数**：

```C++
class Quote{
public:
    string isbn() const;  //共用
    virtual double net_price(size_t n) const;  //虚函数
};
```

派生类通过使用**类派生列表**明确指出继承基类，每个基类都可以带有访问说明符：

```C++
class Bulk_quote : public Quote{
public: 
    double net_price(size_t) const override;
};
```

Bulk_quote在类派生列表中使用了public，因此我们完全可以把Bulk_quote的对象当作Quote的对象来使用。

**override 关键字允许派生类显式地注明它使用哪个成员函数改写基类的虚函数。**

通过使用**动态绑定**我们能用同一段代码分别处理基类与派生类：

```C++
double print_total(ostream &os, const Quote &item, size_t n){
    double ret = item.netprice(n);
    return ret;
}
```

**形参是Quote&，但我们既能使用Quote，也能使用Bulk_quote的对象作为实参调用该函数。**

## 定义基类与派生类

### 定义基类

**基类通常需要定义一个虚析构函数，即使该函数不执行任何实际操作也是如此。**

```C++
class Quote{
public:
    Quote() = default;
    Quote(const string &book, double sale_price):bookNo(book), price(sale_price){}
    string isbn() const { return bookNo; }
    // 虚函数
    virtual double net_price(size_t n) const { return n * price; }
    virtual ~Quote() = default;
private:
    string bookNo;
protected:
    double price = 0.0;
};
```

**当我们使用指针或者引用调用虚函数时，该调用将被动态绑定。**任何构造函数之外的非静态函数都可以是虚函数。关键字virtual只能出现在类内部和声明语句之前而不能用于类外部的函数定义。

****

派生类可以访问基类中的public、protected成员，但不能访问基类的private成员。

### 定义派生类

派生类必须通过**类派生列表**明确指出它从哪个（哪些）基类继承而来。且必须将其继承而来的成员函数中需要覆盖的那些重新声明：

```C++
class Bulk_quote : public Quote{
public: 
    Bulk_quote() = default;
    Bulk_quote(const string&, double, size_t, double);
    // 覆盖基类的函数版本以实现基于大量购买的折扣
    double net_price(size_t) const override;
private:
    size_t min_qty = 0;
    double discount = 0.0;
};
```

Bulk_quote从Quote继承了isbn函数和bookNo、price等数据成员，此外还定义了net_price的全新版本，同时新增两个数据成员min_qty和discount。

****

如果派生类没有选择覆盖虚函数，则派生类会直接继承其在基类中的版本。**C++11允许派生类注明覆盖虚函数的成员函数，在形参列表后面、const、&、&&后面加上override即可**

****

一个派生类对象包含多个组成部分：含有派生类自己定义的非静态成员的子对象；与该派生类继承的基类对应的子对象。因此，我们可以将派生类对象当作基类对象使用，也能将基类的指针或者引用绑定到派生类对象中的**基类部分上**：

```C++
Quote item;
Bulk_quote bulk;
Quote *p = item;  
p = &bulk;  // p指向bulk中的Quote部分
Quote &r = bulk;  // r绑定到bulk的Quote部分
```

这种转换通常被称为**派生类到基类的类型转换**，这意味着我们可以把派生类对象或者派生类对象的引用（指针）用在需要基类引用（指针）的地方。

**在一个对象中，继承自基类的部分和派生类自定义的部分不一定连续存储。**

****

派生类不能直接初始化从基类继承的数据成员，**而应该使用基类的构造函数来初始化它的基类部分:**

```C++
Bulk_quote(const string& book, double p, size_t qty, double disc):Quote(book, p), min_qty(qty), discount(disc) { }
```

首先初始化基类的部分，然后按照声明的顺序一次初始化派生类的成员。

**即，每个类控制自己的成员初始化过程。**

****

如果基类定义了一个静态成员，则整个继承体系中仅存在该成员的唯一定义：

```C++
class Base{
public: 
    static void statmen();
};
clas Derived : public Base{
    void f(const Derived&);
};
```

我们既能通过基类也能通过派生类使用静态成员：

```C++
void Derived::f(const Derived &derived_obj){
    Base::statmen();  //正确
    Derived::statmen();  //正确
    derived_obj.statmen();  //正确
    statmen();  //正确
}
```

****

如果我们想将某个类用作基类，则该类必须已经定义而非仅仅声明。**一个类是基类，同时它也可以是派生类：**

```C++
class Base {};
class D1 : public Base {};
class D2 : public D1 {}；
```

Base是D1的**直接基类**，同时也是D2的**间接基类**。每个类都会继承直接基类的所有成员，**最终的派生类将包含它的直接基类的子对象以及每个间接基类的子对象。**

****

在类名后面跟一个关键字final来阻止继承的发生:

```C++
class NoDerived final {/**/};  //NoDerived不能作为基类
class Last final : Base {/**/};  //Last不能作为基类
```

### 类型转换和继承

智能指针类也支持派生类向基类的类型转换，这意味着我们可以将一个派生类对象的指针存储在一个基类的只能指针内。

如果表达式既不是引用也不是指针，则它的动态类型永远与静态类型一致。**基类的指针或引用的静态类型可能与动态类型不一致。**

**不存在从基类向派生类的隐式类型转换。即使一个基类指针或引用已经绑定在一个派生类对象上：**

```C++
Bulk_quote bulk;
Quote *itemP = &bulk;
Bulk_quote *bulkP = itemP;  //错误
```

**派生类向基类的自动类型转换只对指针或者引用类型有效：**

```C++
Bulk_quote bulk;
Quote item(bulk);  // 调用Quote::Quote(const Quote&)
item = bulk;    // 调用Quote::operator=(cont Quote&)
```

上诉item只能处理item拥有的数据成员，而item没有派生类有的会被忽略，即被**切掉**了。

## 虚函数

由于我们使用基类指针或引用调用一个虚成员函数时会执行动态绑定，即直到运行时才知道调用了哪个虚函数。因此，**所有的虚函数都必须定义。**

**必须要注意，动态绑定只有在使用基类指针或者引用调用时才会发生！**

****

一旦某个函数被声明为虚函数，则它在所有派生类中都是虚函数。一个派生类的函数如果覆盖了某个而继承而来的虚函数，则它的形参类型必须与被覆盖的基类函数完全一致。

同时，它的返回类型也必须与被覆盖的基类函数完全一致，**但有个例外：当类的虚函数返回类型是类本身的指针或者引用时，该规则无效（D由B派生，B的虚函数返回*B，D的对应函数可以返回\*D）。**

****

派生类对应的虚函数形参可以不同，但会被认为是相互独立的，显然与我们的初衷不符。但这种错误很难发现，因此使用override标记虚函数是很有必要的，当没有覆盖虚函数时编译器就会报错。同时我们也可以将函数声明为final的，这时任何尝试覆盖该函数的操作都将发生错误：

```C++
struct B{
    virtual void f1(int) const;
    virtual void f2();
    void f3();
    virtual void f4();
};

struct D2 : B{
    void f1(int) const override;  //正确
    void f2(int) override;  //错误
    void f3() override;  //错误
 
    void f4() const final;  //不允许后续的其它类覆盖f4()
};

struct D2 : D2{
    void f2();  //正确，覆盖间接基类B的f2
    void f4() const;  //错误，已经final了
};
```

****

**虚函数同样拥有默认实参，如果某次函数调用使用了默认实参，则该实参值由本次调用的静态类型决定。即：如果我们通过基类的指针或引用调用函数，则使用基类中的默认实参，即使实际运行的是派生类函数也是如此！**

因此，当虚函数使用默认实参时，则基类和派生类中定义的默认实参最好一致。

****

当我们希望对虚函数的调用不进行动态绑定，而是强迫执行虚函数的某个特定版本，可以使用作用符L

```C++
double undiscounted = baseP->Quote::net_price(42);
```

不管baseP实际指向的是否是基类，都调用基类的net_price函数。

## 抽象基类

我们可以将函数定义为**纯虚函数**从而告诉用户当前的函数是没有实际意义的。**纯虚函数无需定义**：

```C++
class Dsic_qoute : public Quote{
public:
    Disc_quote() = default;
    Disc_quote(const string &book, double price, size_t qty, double disc): Quote(book, price), quantity(qty), discount(disc) { }
    double net_price(size_t) const = 0;  //纯虚函数
protected:
    size_t quantity = 0;
    double discount = 0.0;
};
```

通过在函数体的位置（声明语句的分号之前）书写=0将一个虚函数声明为纯虚函数，=0只能出现在类内部的虚函数声明语句处。

**可以为纯虚函数提供定义，但函数体必须定义在类的外部。**

****

含有纯虚函数的类是**抽象基类**，我们**不能直接创建一个抽象基类的对象**。我们可以定义派生类的对象，前提是这些派生类覆盖了纯虚函数：

```C++
Disc_quote discounted;  //错误
Bulk_quote bulk;  //正确，Bulk_quote没有纯虚函数
```

****

实现Bulk_quote:

```C++
class Bulk_quote : public Disc_quote{
public:
    Bulk_quote() = default;
    Bulk_quote(const string &book, double price, size_t qty, double disc) : Disc_quote(book, price, qty, disc) { }
    double net_price(size_t) const override;
};
```

每个类各自控制其对象的初始化过程。Bulk_quote的构造函数将实参传递给直接基类。Disc_quote的构造函数又将实参传递给直接基类Quote。

## 访问控制与继承

每个类分别控制着其成员对派生类来说是否可访问。

Protected成员对于类的用户来说无法访问，对于**派生类的成员**和**友元**来说可以访问。**但派生类的成员和友元只能通过派生类对象来访问基类的protected成员，派生类对于一个基类对象中的受保护成员没有任何访问权限：**

```C++
class Base {
protected:
    int prot_mem;
};
class Sneaky : public Base {
    friend void clobber(Sneaky&);  // 可以访问 sneaky::prot_mem 和 j
    friend void clobber(Base&);  // 不能访问Base::prot_mem
    int j;  // 默认是private
};
```

**派生类的成员和友元只能访问派生类对象中的基类部分的受保护成员：对于普通的基类对象中的成员不具有特殊的访问权限。**

****

考虑派生列表中的访问说明符：

```C++
class Base {
public:
    void pub_mem();
protected:
    int  prot_mem;
private:
    char priv_mem;
};

struct Pub_Derv : public Base {
    int f() { return prot_mem; }  //正确
    char g() { return priv_mem; }  //错误，private成员对于派生类来说不能访问
};

struct Priv_Derv : private Base {
    int f() const { return prot_mem; }  //正确
    char g() { return priv_mem; }  //错误，private成员对于派生类来说不能访问
};
```

派生访问说明符对派生类的成员及友元能否访问其直接基类的成员没有任何影响！**影响的是派生类用户对于基类成员的访问权限：**

```C++
Pub_Derv d1;  // 继承自Base的成员是public的
Priv_Derv d2;  // 继承自Base的成员是private的
d1.pub_mem();  //正确
d2.pub_mem();  // 错误,pub_mem在派生类中是private的
```

如果继承是public的，则成员将遵循原有的访问说明符。如果继承是protected的，所有继承自基类的public成员全部转为protected的。如果继承是private的，所有继承自基类的public和protected成员全部按转为private成员。**且派生类无法访问继承自基类private的成员。**

假设D继承自B：

1）只有当D公有继承B时，用户代码才能使用派生类向基类的转换；

2）无论D以什么方式继承B，D的成员和友元函数都能使用派生类向基类的转换；

3）当且仅当D以公有或受保护的方式继承B时，D的派生类的成员和友元才能使用D向B的转换。

基类的友元不具有继承性，即：**基类的友元在访问派生类时不具有特殊性，派生类的友元也不能随意访问基类的成员。但是基类的友元可以访问派生类的基类部分：**

```C++
class Base {
    friend class Pal;
};

class Class {
public:
    int f(Base b) { return b.prot_mem; }
    int f2(Sneaky s) { return s.j; }  // 错误，pal不是Sneaky的友元
    int f3(Sneaky s) { return s.prot_mem; }  //正确，pal可以访问snecky的基类部分
};
```

通过在类的内部使用using声明语句，我们可以将该类的直接或间接基类中的任何可访问成员标记出来，改变其可访问性：

```C++
class Base {
public:
    size_t size() const { return n; }
protected:
    size_t n;
};

class Derived : private Base {
public:
    using Base::size;
protected:
    using Base::n;
};
```

默认情况下，size 和 n 在 Drived 中是 private 的。using不能标记直接或间接基类的private成员。

****

```C++
class Base{};
struct D1 : Base{};  //默认public继承
class D2 : Base{};  //默认private继承
```

默认情况下 struct 是 public 继承的， class 是 private 继承的

## 继承中的类作用域

派生类的作用域是嵌套在基类作用域内的，如果一个名字在派生类作用域中无法解析，编译器会继续在外层的基类作用域中寻找。

一个对象、引用或者指针的静态类型决定了该对象的哪些成员是可见的：

```C++
class Disc_quote : public Quote {
public:
    pair<size_t, double> discount_policy() const { return {quantity, discount}; }
};

Bulk_quote bulk;
Bulk_quote *bulkP = &bulk;
Quote *itemP = &bulk;
bulkP->discount_policy();  // 正确
itemP->discount_policy();  // 错误，itemP的类型是Quote*，只能访问bulk的基类部分
```

当派生类重用定义在其直接基类或者间接基类的名字时，此时定义在派生类（内层作用域）的名字将隐藏定义在基类（外层作用域）的名字：

```C++
struct Base{
    Base() : mem(0){ }
protected:
    int mem;
};

struct Drived : Base{
    Derived(int i) : mem(i) { }  // 用i初始化Derived::mem，Base::mem进行默认初始化
    int get_mem() { return mem; }  // 返回Derived::mem
    int get_base_mem() { return Base::mem; }  // 返回Base::mem
 protected:
    int mem;  // 隐藏基类中的mem
};
```

可以通过作用域运算符使用被隐藏的成员。**除了覆盖继承而来的虚函数以外，派生类最好不要重用其它定义在基类中的名字。**

因此，**派生类的函数并不会重载基类的同名函数**，即使形参列表不同，**同名函数遵循内层作用域隐藏外层作用域的设定**。

```C++
struct Base{
    int memfcn();  
};

struct Derived : Bses{
    int memfcn(int);
};

Derived d; Base b;
b.memfcn();
d.memfcn(10);
d.memfcn();  //错误，被隐藏
d.Base::memfcn();  //正确
```

****

和其它函数一样，成员函数无论是否是虚函数都能被重载，派生类可以覆盖重载函数的0个或多个实例。如果派生类希望所有的重载版本对它都可见，则需要覆盖所有的版本，或者一个也不覆盖。

## 构造函数与拷贝函数

### 虚析构函数

当我们delete一个动态分配的对象的指针时会执行析构函数，而在继承体系中，可能会出现指针的静态类型和被删除的对象的动态类型不符的情况。比如：delete一个指向Bulk_quote的Quote*。编译器必须清楚它应该执行的析构函数：

```C++
class Quote{
public:
    virtual ~Quote() = default;
};
```

将析构函数声明为虚函数。**无论派生类使用合成的还是自己定义的析构函数都将是虚函数。**

**基类的析构函数并不遵循：如果一个类需要析构函数，则它同样需要拷贝和赋值操作这个准则。**

如果一个类定义了虚析构函数，则编译器将不会为这个类合成移动操作。

### 合成拷贝控制与继承

对于派生类的析构函数来说，它除了销毁自己的成员外，还负责销毁派生类的直接基类。某些定义基类的方式可能导致派生类的成员称为被删除的函数：

1）如果基类中的默认构造、拷贝构造、拷贝赋值或者析构函数是被删除的或者不可访问的，则派生类对应成员将是被删除的。**因为编译器不能使用基类成员来执行派生类对象基类部分的构造、赋值、销毁。**

2）如果基类中有一个不可访问或者删除的析构函数，则派生类中合成的默认构造和拷贝函数将是删除的，因为编译器无法销毁派生类对象的基类部分。

3）如果基类中的移动操作是删除的或者不可访问的，则派生类中该函数也是删除的，因为派生类中的基类部分不可移动。同样，如果基类的析构函数是删除的或者不可访问的，则派生类的移动构造函数也将是被删除的。

```C++
class B{
public:
    B();
    B(const B&) = delete; 
};

class D : public B {
    // 不声明任何构造函数
};

D d;  // D的合成默认构造函数使用B的默认构造函数
D d2(d);  // 错误，D的合成拷贝构造函数是被删除的
D d3(std::move(d));  //  错误，隐式地使用D的被删除的合成拷贝    
```

B定义了删除的拷贝构造函数，因此编译器不会为它合成移动构造函数。即：**B既不能拷贝也不能移动。**B的派生类D需要考虑它的基类部分如何移动和拷贝。一般情况下，如果基类没有默认、拷贝、移动构造，则派生类也不会定义。

****

一般情况下，基类都会定义**虚析构函数**，所以基类通常不会有移动操作。因此，在它的派生类中也就没有合成的移动操作。

当我们需要移动操作时，就需要显式定义：

```C++
class Quote{
public:
    Quote() = default;
    Quote(const Quote&) = default;
    Quote(Quote&&) = default;
    Quote& operator=(const Quote&) = default;
    Quote& operator=(Quote&&) = default;
    virtual ~Quote() = default; 
};
```

一旦Quote定义了自己的移动操作，则必须显式地定义其它的拷贝操作。

### 派生类的拷贝控制成员

派生类的拷贝控制成员在拷贝和移动自有成员的同时，也要拷贝和移动基类部分的成员。

**不同的地方在于，派生类的析构函数只负责销毁派生类自己分配的资源。类似的，派生类对象的基类部分也是自动销毁的。**

```C++
class Base {/* */};
class D : public Base {
public:
    D(const D& d): Base(d) {}  // 拷贝基类成员
    D(D&& d) Base(std::move(d))  // 移动基类成员
};
```

D类型的形参会被绑定到基类构造函数的Base&形参上。Base的拷贝构造函数负责将d的基类部分拷贝给要创建的对象，但如果我们没有提供基类的初始值：

```C++
D(const D& d){}
```

基类部分将会被默认初始化，而不是拷贝初始化。

**默认情况下，基类默认构造函数会被用来初始化派生类的基类部分。如果我们想要拷贝（移动）基类部分，则显然必须要在派生类的构造函数初始值列表中显式地使用基类的拷贝（移动）构造函数。**

****

同样，派生类的赋值运算符也必须显式地为基类部分赋值：

```C++
D &D::operator=(const D &rhs){
    Base::operator=(rhs);
    /*为D的成员赋值*/
    return *this;
}
```

****

派生类析构函数只负责销毁自己分配的资源：

```C++
class D : public Base {
public:
    // Base::~Base 被自动的调用执行
    ~D(){/*用户定义清除操作*/}
};
```

****

**基类先创建、派生类先析构，因此在拷贝、析构函数中、仅执行与该构造函数或析构函数所属类型相对应的虚函数版本是安全的。**

### 继承的构造函数

派生类可以继承其直接基类定义（不能继承默认、拷贝、移动构造函数）的构造函数：

```C++
class Bulk_quote : public Disc_quote {
public:
    using Disc_quote::Disc_quote;  //继承Disc_quote的构造函数
    double net_price(size_t) const;
};
```

当using作用于构造函数时，using声明语句会令编译器产生代码，而不是平常的令某个名字在当前作用域可见。对于基类的每个构造函数，编译器都会在派生类中生成一个形参列表完全相同的构造函数，形如：

```C++
derived(params) : base(args) {}
```

举个例子，在Bulk_quote类中，继承的构造函数等价于：

```c++
Bulk_quote(const string& book, double price, size_t qty, double disc) : Disc_quote(book, price, qty, disc) { }
```

而派生类自己的数据成员将被默认初始化。

**构造函数的using不会改变访问级别，一个using声明语句不能指定 explicit 或者 constexpr ，如果基类的构造函数是 explicit 或者 constexpr ，则继承的构造函数也拥有相同的属性。**

当一个基类的构造函数含有默认实参，这些默认实参不会被继承。派生类将获得多个继承的构造函数，每个构造函数分别省略一个含有默认实参的形参。

举个例子：假设基类有一个接受两个形参的构造函数，其中第二个形参含有默认实参，则派生类会获得两个构造函数：一个构造函数接受两个形参（没有默认实参），另一个构造函数只接受一个形参。

当基类有几个构造函数，派生类大部分情况下会继承所有的构造函数，但有两个例外：

1）如果派生类定义的构造函数与基类的构造函数具有相同的形参，则该构造函数不会被继承，而是被替换。

2）默认、拷贝、移动构造函数不会被继承。

## 容器与继承

由于不允许在容器中保存不同类型的元素，我们不能把具有继承关系的多种类型的对象直接存放在容器中。因此，通常情况下，当我们使用容器存放继承体系的对象时，采用的是间接存储的方式：

```C++
vector<Quote> basket;
basket.push_back(Quote("0-201-82470-1", 50));
//正确，但只能把对象的Quote部分拷贝给basket
basket.push_back(Bulk_quote("0-201-82470-1", 50, 10, .25));
//调用的Quote定义的版本，打印750
cout << basket.back().net_price(15)  << endl;
```

因此，我们通常在容器中放置（智能）指针而非对象：

```C++
vector<shared_ptr<Quote>> basket;
basket.push_back(make_shared<Quote>("0-201-82470-1", 50));
basket.push_back(make_shared<Bulk_quote>("0-201-82470-1", 50, 10, .25));
//调用的Bulk_quote定义的版本，打印562.5，去掉了折扣
cout << basket.back()->net_price(15)  << endl;       
```

### 编写Basket类

首先我们需要给Quote和Bulk_quote增加一个clone虚函数以动态绑定：

```C++
class Quote{
public:
    // 定义左右值重载
    virtual Quote* clone() const & { return new Quote(*this); }
    virtual Quote* clone() && { return new Quote(std::move(*this)); }
};

class Bulk_quote{
public:
    // 定义左右值重载
    Bulk_quote* clone() const & override { return new Bulk_quote(*this); }
    Buol_quote* clone() && override { return new Bulk_quote(std::move(*this)); }
};
```

定义一个辅助类使用指针和引用实现面向对象编程：

```C++
class Basket{
public:
    void add_item(const Quote& sale);
    void add_item(const Quote&& sale);
    double total_receipt(ostream&) const;
private:
    // 必须定义为static
    static bool compare(const shared_ptr<Quote> &lhs, const shared_ptr<Quote> &rhs) { return lhs->isbn() > rhs->isbn();}
    multiset<shared_ptr<Quote>>, decltype(compare)*> items{compare};  // 不知道为什么这里用大括号，小括号会报错
};
```

定义 total_receipt 函数，用到upper_bound( )函数，该函数返回

指向与相同元素中最后一个元素的下一个位置：

```C++
double total_receipt(ostream&) const {
    double sum = 0.0;
    for(auto iter = items.cbegin(); iter != items.end(); iter = items.upper_bound(*iter)){
        sum += print_total(os, **iter, items.count(*iter));
    }
   	return sum;
}
```

使用clone函数写出add_item：

```C++
class Basket {
public:
    void add_item(const Quote& sale){
        items.insert(shared_ptr<Quote>(sale.clone()));
    }
    void add_item(const Quote&& sale){  // sale虽然是个右值引用，但他本身是个左值
        items.insert(shared_ptr<Quote>(std::move(sale).clone()));  //注意右值成员函数的调用方式
    }
};
```

## 再探文本查询程序

这节将重构文本查询程序，以实现与或非运算，为此采用继承体系。

![image-20230417103549507](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230417103549507.png)

所有的这些类都只包含两个操作：

1）eval，接受一个TextQuery对象并返回一个QueryResult，使用给定的TextQuery对象查找与之匹配的行；

2）rep，返回基础查询的string表示形式，eval函数使用rep创建一个表示匹配结果的QueryResult，输出运算符使用rep打印查询表达式。

我们将定义一个接口类Query用于隐藏整个继承体系，并实现这样的复合查询：

```C++
Query q = Query("fiery") & Query("bird") | Query("wind");
```

该表达式满足：

![image-20230417104802750](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230417104802750.png)

所有的类应有功能：

![image-20230417105150069](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230417105150069.png)



**当我们令一个类共有地继承另一个类时，派生类应当反映与基类的“是一种(Is a)”关系，类和成员函数应当是“有一个（has a）”的关系。**

### Query_base 和 Query 类

```C++
// 抽象基类
class Query_base{
    friend class Query;
protected:
    using line_no = vector<string>::size_type;
    virtual ~Quote_base() = default;
private:
    virtual QueryResult eval(const TextQuery&) const = 0;
    virtual string rep() const = 0;  //纯虚函数，抽象基类
};
```

**当我们不希望用户使用某个类时，可以不将任何成员定义为public，当其位于继承体系中时，可以定义为抽象基类。**

```C++
class Query{
    // 这些运算符类需要访问私有成员
    friend Query operator~(const Query &);
    friend Query operator|(const Query &, const Query &);
    friend Query operator&(const Query &, const Query &);
public:
    Query(const string&);  //返回一个WordQuery对象，还没定义，因此要在之后定义
    //接口函数，q是指针，动态绑定，调用哪个虚函数运行时才确定
    QueryResult eval(const TextQuery &t) const { return q->eval(t); }
    string rep() const { return q->rep();}
private:
    Query(shared_ptr<Query_base> query) : q(query) { }  //定义指向Query_base的指针，但不想用户使用，所以定义为private的
    shared_ptr<Query_base> q;
};
```

重载输出运算符：

```C++
ostream &operator<<(ostream &os, const Query &query){
    return os << query.rep();
}
```

### 定义派生类

一个WordQuery查找一个给定的string，它是在给定的TextQuery对象上实际执行的唯一一个操作：

```C++
class WordQuery : public Query_base{
    friend class Query;
    WordQuery(const string &s) : query_word(s) { }
    QueryResult eval(const TextQuery &t) const {
        return t.query(query_word);
    }
    string rep() const { return query_word; }
    string query_word;  //要查找的单词
};
```

之后，我们的Query构造函数就可以定义了：

```C++
inline Query::Query(const string &s) : q(new WordQuery(s)) { }
```

~运算符生成一个NotQuery：

```C++
class NotQuery : public Query_base{
    friend Query operator~(const Query &);
    NotQuery(const Query &q) : query(q) { }
    string rep() const { return "~(" + query.rep() + ")"}
    QeryuResult eval(const TextQuery&) const;
    Query query;
};

inline Query operator~(const Query &operand){
    return shared_ptr<Query_base>(new NotQuery(operand));
}
```

BinaryQuery类是一个抽象基类，它保存操作两个运算对象的查询类型所需的数据：

```C++
class BinaryQuery : public Query_base{
protected:
    BinaryQuery(const Query &l, const Query &r, string s) : lhs(l), rhs(r), opSym(s) { }
    string rep() const { return "(" + lhs.rep() + " " + opSym + " " + rhs.rep() + ")"; }
    Query lhs, rhs;
    string opSym;
};
```

BinaryQuery不定义eval，而是继承了该纯虚函数，因此也是抽象基类。

有了BinaryQuery，我们就可以定义AndQuery类以及OrQuery类了：

```C++
class AndQuery : public BinaryQuery{
    friend Query operator&(const Query&, const Query&);
    AndQuery(const Query &left, const Query &right) : BinaryQuery(left, right, "&") { }
    QueryResult eval(const TextQuery&) const;
};

inline Query operator&(const Query &lhs, const Query &rhs){
    return shared_ptr<Query_ptr>(new AndQuery(lhs, rhs));
}

class OrQuery : public BinaryQuery{
    friend Query operator|(const Query&, const Query&);
    OrQuery(const Query &left, const Query &right) : BinaryQuery(left, right, "|") { }
    QueryResult eval(const TextQuery&) const;
};

inline Query operator|(const Query &lhs, const Query &rhs){
    return shared_ptr<Query_ptr>(new OrQuery(lhs, rhs));
}
```

### eval 函数

假设QueryResult包含有begin和end成员，允许我们在QueryResult保存的行号set中进行迭代；另外有一个成员函数get_file，返回一个指向待查文件的shared_ptr。

****

```C++
QueryResult OrQuery::eval(const TextQuery &text) const {
    auto right = rhs.eval(text), left = lhs.eval(text);
    auto ret_lines = make_shared<set<line_no>>(left.begin(), left.end());
    ret_lines->insert(right.begin(), right.end());
    return QueryResult(rep(), ret_lines, left.get_file());
}
```

****

```C++
QueryResult AndQuery::eval(const TextQuery &text) const {
    auto right = rhs.eval(text), left = lhs.eval(text);
    auto ret_lines = make_shared<set<line_no>>();
    //调用标准库函数求得共有的行
    set_intersection(left.begin(), left.end(), right.begin(), right.end(), inserter(*ret_lines, ret->begin()));
    ret_lines->insert(right.begin(), right.end());
    return QueryResult(rep(), ret_lines, left.get_file());
}
```

set_intersection使用前四个迭代器表示两个输入序列，最后一个实参表示目的位置（用插入迭代器inserter）。

****

```C++
QueryResult NotQuery::eval(const TextQuery &text) const {
	auto result = query.eval(txet);
    auto ret_lines = make_shared<set<line_no>>();
    auto begin = result.begin(), end = result.end();
    auto sz = result.get_file()->size();
    // 对于输入文件的每一行，如果该行不在result中，则将其添加到ret_lines
    for(size_t n = 0; n != sz; n++) {
        if(begin == end || *begin != n)
            ret_lines->insert(n);
        else if (begin != end)
            ++begin;
    }
    return QueryResult(rep(), ret_lines, left.get_file());
};
```

# 模板与泛型编程

模板是泛型编程的基础，一个模板就是一个创建类或函数的蓝图或者说公式。

## 定义模板

### 函数模板

我们可以定义一个通用的模板函数，而不是为每个类型都定义一个新函数来实现同样的功能：

```C++
template <typename T> int compare(const T &v1, const T &v2){
    if(v1 < v2) return -1;
    if(v2 < v1) reuturn 1;
    return 0;
}
```

模板定义由template开始，后跟一个**模板参数列表**：这是一个逗号分隔的一个或多个**模板参数**的列表，用<>包围起来。

**模板定义中，模板参数列表不能为空**

模板参数表示在类或者函数定义中用到的类型或值，当使用模板时，我们（显式或者隐式地）指定**模板实参**，将其绑定到模板参数上。

****

当我们调用一个函数模板时，编译器（通常）用函数实参来为我们推断模板的实参。如，当我们调用上面的compare时，编译器使用函数的实参类型来确定绑定到模板的参数T的类型：

```C++
cout << compare(1, 0) << endl;  
```

在上面的调用中，实参类型为int，编译器会推断出模板实参为int，并绑定到模板参数T上。编译器用推断出的模板参数来为我们**实例化**一个特定版本的函数。编译器生成的函数版本统称为**实例**。

****

我们可以将类型参数看作类型说明符，就像内置类型或类类型说明符一样使用。**特别是，类型参数可以用来指定返回类型或函数的参数类型，以及在函数体内用于变量声明或类型转换：**

```C++
template <typename T> T foo(T* p){
    T tmp = *p;
    //...
    return tmp;
}
```

类型参数前面必须使用关键字class或者typename:

```C++
template <typename T, U> T calc(const T&, const U&);  // 错误：U之前必须加上 class 或者 typename
```

 在模板参数列表中，这两个关键字含义相同，可以互换使用。一个模板列表中可以同时使用这两个关键字：

```C++
template <typename T, class U> T calc(const T&, const U&);  // 正确
```

****

除了定义类型参数，还可以在模板中定义**非类型参数**。一个非类型参数表示一个值而非一个类型。我们通过一个特定的类型名而非关键字class或者typename指定非类型参数：

```C++
tempale<unsigned N, unsigned M> int compare(const char (&p1)[N], const char (&p2)[M]){
    return strcmp(p1, p2);
} 
```

参数为数组的引用，只能使用常量表达式调用：

```c++
compare("hi", "mom");
```

编译器会使用字面常量值的大小代替M和N，从而实例化模板。**由于编译器会在字符串字面量的末尾插入一个空字符作为终结符**，因此实例化版本为：

```C++
int compare(const char (&p1)[3], const char (&p2)[4]){
    return strcmp(p1, p2);
} 
```

一个非类型参数可以是一个整型、一个指向对象或函数类型的指针或者（左值）引用。**绑定到非类型整型参数的实参必须是一个常量表达式，绑定到指针或者引用非类型参数的实参必须具有静态的生存期。**不能同一个普通（非static）的局部变量或者动态对象作为指针或引用非类型模板参数的实参。指针参数可以用nullptr或者一个值为0的常量表达式来实例化。

**非类型模板参数的模板实参必须是常量表达式**

****

函数模板可以声明为 inline 或 constexpr 的：

```C++
template <typename T> inline T min(const T&, const T&);
```

模板参数应该尽量减少对实参类型的要求：

```C++
template <typename T> int compare(const T &v1, const T &v2){
    if(less<T>()(v1, v2)) return -1;
    if(less<T>()(v2, v1)) reuturn 1;
    return 0;
}
```

上诉函数可以用于指针。

### 类模板

在类模板（及其成员）的定义中，我们使将模板参数当作替身，代替使用模板时用户需要提供的类型或值：

```C++
template <typename T> class Blob{
public:
    typedef T value_type;
    typedef typename vector<T>::size_type size_type;
    Blob();
    Blob(initializer_list<T> il);
    size_type size() const { return data->size(); }
    bool empty() const { return data->empty();}
    void push_back(const T &t) const { data->push_back(t);
    void push_back(T &&t) { data->push_back(std::move(t)); }
    void pop_back();
    T& back();
    T& operator[](size_type i);                     private:
    shared_ptr<vector<T>> data;
    void check(size_type i, const string &msg) const;                                  
};
```

当使用一个类模板时，我们必须提供额外信息，这些信息就是**显式模板实参**列表，它们被绑定到模板参数。例如，为了使用我们的类模板定义类型，就必须提供元素类型：

```C++
Blob<int> ia;
Blob<int> ia2 = {0, 1, 2, 3, 4};
```

**一个类模板的每个实例都是一个独立的类。**

****

可以在模板作用域中引用模板类型：

```C++
shared_ptr<vector<T>> data;  //T是模板参数，用作另一个模板的实参
```

****

定义在类模板之外的成员函数必须以关键字template开始，后接类模板参数列表，且一个从模板生成的类的名字中必须包含模板实参：

```C++
template <typename T> ret_type Blob<T>::member_name(parm_list){} 
```

例如，我们来定义check成员：

```C++
template <typename T> void Blob<T>::check(size_type i, const string &msg) const{
    if(i >= data->size())
        throw out_of_range(msg);
}
```

构造函数同样需要以template开始：

```C++
template <typename T> Blob<T>::Blob() : data(make_shared<vector<T>>()) { } 

template <typename T> Blob<T>::Blob(initializer_list<T> il) : data(make_shared<vector<T>>(il)) { } 
//为了使用接受初始化列表的构造函数创建类，我们必须传递给他一个initializer_list
Blob<string> articles = {"a", "an", "the"};
```

****

默认情况下，一个类模板的成员函数只有当程序用到它时才进行实例化，**如果一个成员函数没有被使用，则它不会被实例化。**

当我们使用一个类模板类型时必须提供模板实参(<T>)，但存在一个例外：在类模板自己的作用域中，我们可以直接使用模板名而不提供实参：

```C++
template<typename T> class BlobPtr{
public:
    BlobPtr() : curr(0) { }
    BlobPtr(Blob<T> &a, size_t sz = 0) : wptr(a.data), curr(sz) { }
    // 不需要显式表面返回类型为BlobPtr<T>&
    BlobPtr& operator++();
    BlobPtr& operator++();
};
```

当处于一个类模板的作用域中时，编译器处理模板自身引用时就好像我们已经提供了与模板参数匹配的实参一样。即：就好像我们这样编写代码：

```C++
BlobPtr<T>& operator++();
BlobPtr<T>& operator++();
```

****

当我们在类模板外定义成员时，就不在类的作用域中：

```C++
template <typename T>
BlobPtr<T> BlobPtr<T>::operator++(int){  //形参给个int，表示后置版本++
    BlobPtr ret = *this;
    ++*this;
    return ret;
}
```

在函数体内，我们已经进入类的作用域，因此在定义ret时无需重复模板实参。如果不提供模板实参，则编译器将假定我们使用的类型与成员实例化所用类型一致。因此，ret的定义与如下代码等价：

```C++
BlobPtr<T> ret = *this;
```

**在一个类模板的作用域内，我们可以直接使用模板名而不必指定模板实参。**

****

当类含有友元声明时，类与友元各自是否是模板是相互无关的。**如果一个类模板包含一个非模板友元，则友元被授权可以访问所有模板实例。如果友元自身是模板，类可以虚授权给所有友元模板实例，也可以只授权给特定实例。**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            

为了引用（类或者函数）模板的一个特定实例，我们必须首先声明模板自身。一个模板声明包括模板参数列表：

```C++
template <typename> class BlobPtr;
template <typename> class Blob;
template <typename T>
	bool operator==(const Blob<T>&, const Blob<T>&);
template <typename T> class Blob{
    // 每个Blob实例将访问权限授予用相同类型实例化的BlobPtr和相等运算符
    friend class BlobPtr<T>;
    friend bool operator==<T> (const Blob<T>&, const Blob<T>&);
};
```

同样，一个类也可以将另一个模板的每个实例都声明为自己的友元，或者限定特定的实例为友元：

```C++
template <typename T> class Pal;
class C{
    friend class Pal<C>;  //用类C实例化的Pal是C的一个友元
    // Pal2的所有实例都是C的友元，这种情况无需前置声明
    template <typename T> friend class Pal2;
};
template <typename T> class C2{
    friend class Pal<T>;  //相同实例化的Pal声明为友元
    // Pal2的所有实例都是C2的友元，这种情况无需前置声明
    template<typename X> friend class Pal2;  
    // Pal3是非模板类，它是C2所有实例的友元
    friend class Pal3;  
}；
```

为了让所有实例称为友元，友元声明中必须使用与类模板本身不同的模板参数。                                                                                                                

```C++
template <typename Type> class Bar{
    friend Type;  // 将访问权限授予用来实例化Bar的类型
};
```

新标准允许我们将模板类型参数声明为友元，因此，对于某个类型名Sales_data将称为Bar<Sales_data>的友元。**这里的Type也可以是内置类型**。

****

与任何其它类类型一样，我们可以定义一个typedef来引用实例化的类：

```C++
typedef Blob<string> StrBlob;
```

**虽然可以定义一个typedef引用实例化的模板类，但不能引用一个模板。即，无法定义一个typedef引用Blob\<T>。**

但新标准允许我们为类模板定义一个类型别名：

```C++
template<typename T> using twin = pair<T, T>;
twin<string> authors;  // authors是一个pair<string, string>
```

****

类模板同样可以声明static成员：

```C++
template<typename T> class Foo{
public:
    static size_t count() { return ctr; }
private:
    static size_t ctr;
};
```

每个Foo的实例都有自己的static成员实例。即，任意给定类型X，都有一个Foo\<X>::ctr和Foo\<X>::count。所有的Foo\<X>类型的对象共享相同的ctr对象和count函数。

与任何其它的static数据成员相同，模板类的static数据成员必须有且仅有一个定义，因此我们将static数据成员也定义为模板：

```C++
template <typename T>
size_t Foo<T>::ctr = 0;
```

访问：

```C++
Foo<int> fi;
auto ct = Foo<int>::count();  
ct = fi.count();  
ct = Foo::count();  // 错误：使用哪个实例的count?
```

### 模板参数

模板参数名的可用范围是在其声明之后，至模板声明或定义结束之前。模板参数会隐藏外层作用域中声明的相同名字。**但是，在模板内不能重用模板参数名：**

```C++
typedef double A;
template <typename A,typename B> void f(A a, B b){
    A tmp = a;  //tmp的类型为模板参数A的类型，而不是double
    double B;  //错误，重新声明了模板参数B
}
```

模板声明必须包含模板参数：

```C++
// 声明
template <typename T> int compare(const T&, const T&);
template <typename T> class Blob;
```

声明的模板参数的名字不必与定义中的相同：

```C++
template <typename T> T clac(const T&, const T&);
template <typename U> U clac(const U&, const U&);
// 定义
template <typename Type> Type calc(const Type &a, const Type &b) { }
```

上诉三个clac都指向相同的函数模板。

****

仅通过作用域运算符，编译器无法分辨究竟是访问一个类型成员还是一个static成员。比如，当编译器遇到:

```C++
T::size_type * p; 
```

它需要知道表示的是定义一个指向类型的指针p，还是用T的静态成员size_type 与 p 相乘 。默认情况下，C++语言假定通过作用域运算符访问的名字不是类型。**因此，当我们希望使用一个模板类型参数的类型成员， 就必须显式地告诉编译器该名字是一个类型：**

```C++
templaye<typename T>
typename T::value_type top(const T &c){
    if(!c.empty())
        return c.back();
    else
        return typename T::value_type();
}
```

**当我们希望通知编译器一个名字表示类型时，必须使用关键字typename，而不能使用class。**

****

我们同样可以提供**默认模板实参**。在新标准中，我们可以为函数和类模板提供默认实参：

```C++
// 默认模板实参less<T> 和默认函数实参F()
template <typedef T, typedef F = less<T>> //less是个类
int compare(const T &v1, const T &v2, F f = F()){
    if(f(v1, v2)) return -1;
    if(f(v2, v1)) return 1;
    return 0;
}
```

当用户调用这个compare时，可以自己提供比较操作：

```C++
bool i = compare(0, 42); //使用less
Sales_data item1(cin), item2(cin);
bool j = compare(item1, item2, compareIsbn)；
```

在第二个调用中，第三个实参必须是可调用对象，且该可调用对象的返回类型必须能转换为bool值，且接受的实参类型必须与compare的前两个实参的类型兼容。模板参数的类型从他们对应的函数实参推断：T被推断为Sales_data，F被推断为compareIsbn的类型。

**对于一个模板参数，只有当它的右侧所有参数都有默认实参时，它才可以有默认实参。**

****

无论何时使用一个类模板，我们都必须在模板名之后接上尖括号。**如果一个类模板为其所有模板参数都提供了默认实参，且我们希望使用这些默认实参，就必须在模板名之后跟一个空尖括号对：**

```C++
template<class T = int> class Numbers {
public:
    Nmubers(T v = 0): val(v){ }
private:
    T val;
}

Numbers<long double> lots_of_precision;
Numbers<> average_precision;  //空<>表示我们希望使用默认类型
```

### 成员模板

一个类（无论是普通还是类模板）可以包含本身是模板的成员函数。这种成员被称为**成员模板**，成员模板不能是虚函数。

普通类的成员模板：

```C++
class DebugDelete{
public:
    DebugDelete(ostream &s = cerr) : os(s) { }
    // T的类型由编译器推断
    template <typename T> void operator()(T *p) const { delete p; }
private:
    ostream &os;
};
```

使用：

```C++
double *p = new double;
DebugDelete d; //可以像delete表达式一样使用的对象
d(p);  // 调用DebugDelete::operator()(double*)，释放p
int *ip = new int;
// 在一个临时DebugDelte对象上调用operator()(int*)
DebugDelete()(ip);
```

我们可以将这个DebugDelete用于unique_ptr的删除器：

```c++
// 实例化DebugDelete::opertator()<int>(int*)
unique_ptr<int, DebugDelete> p(new int, DebugDelete());  //DebugDelete() 提供一个未命名的对象
```

这样，unique_ptr的析构函数就会调用DebugDelete的调用运算符。无论何时unique_ptr的析构函数实例化，DebugDelete的调用运算符就都会实例化：

```C++
void DebugDelete::opertator()(int *p) const { delete p;}
```

****

类模板的成员模板：

```C++
template <typename T> class Blob{
    template <typename It> Bolb(It b, It e);
};
```

将构造函数定义为成员模板。当我们在类模板外定义一个成员模板时，必须同时为类模板和成员模板提供模板参数列表。类模板的参数列表在前：

```C++
template <typename T>
template <typename It>
Blob<T>::Blob(It b, It e): data(make_shared<vector<T>>(b, e)) { }
```

****

为了实例化一个类模板的成员模板，我们必须同时提供类和函数模板的实参：

```C++
int ia[] = {0,1,2,3,4,5,6,7,8,9};
vector<long> vi = {0,1,2,3,4,5,6,7,8,9};
list<const char*> w = {"now", "is", "the", "time"};
Blob<int> al(begin(ia), end(ia));
Blob<int> a2(vi.begin(), vi.end());
Blob<string> a3(w.begin(), w.end());  //实例化Blob<sting>及接受两个list<const char*>::itertor参数的构造函数
```

### 控制实例化

在模板被使用时才会进行实例化。这意味着相同的实例可能出现在多个对象文件中。在新标准中，我们可以通过**显式实例化**来避免这种开销：

```C++
extern template declaration;  //实例化声明
template declaration;  //实例化定义
```

declaration是一个类或者函数声明，其中所有模板参数以被替换为模板实参：

```C++
extern template class Blob<string>;  //声明
template int compare(const int&, const int&);  //定义
```

当编译器遇到extern模板声明时，它不会在本文件中生成实例化代码。**对于一个给定的实例化版本，可能有多个extern声明，但必须只有一个定义。**

```C++
template class Blob<string>;  // 实例化类模板的所有成员
```

当编译器遇到一个实例化定义时，它会为其生成代码。**实例化会实例化类的所有成员，包括内联的成员函数。**因此，在一个类模板的实例化定义中，**所用类型必须能用于模板的所有成员函数。**

## 模板实参推断

从函数实参来确定模板实参的过程被称为**模板实参推断**。

### 类型转换与模板类型参数

如果一个函数形参的类型使用了模板类型参数，那么它采用特殊的初始化规则。只有很有限的几种类型转换会自动地应用于这些实参。编译器通常不是对实参进行类型转换，而是生成一个模板实例。

顶层的const会被忽略，在其它的类型转换中，能在调用中应用于函数模板的包括：

1）const转换，可以将一个非const对象的引用（指针）传递给一个const引用（或指针）形参；

2）一个数组（函数）实参可以转换为一个指向该数组首元素（函数类型）的指针。

**其它类型的转换，比如算术转换、用户定义的转换都不能应用于函数模板：**

```C++
template <typename T> T fobj(T, T);  
template <typename T> T fref(const T&, const T&);

string s1("a value");
const string s2("another value");
fobj(s1, s2);  //调用 fobj(string, string); const被忽略
fref(s1, s2);  //调用 fref(const string&, const string&);s1被转换为const

int a[10], b[42];
fobj(a, b);  // 调用f(int*, int*)
fref(a, b);  // 错误，数组类型不匹配
```

因为fref的形参是引用，因此数组实参调用时是不会转换为指针的，两个数组的大小不一样，显然类型不匹配。

**将实参传递给带模板类型的函数形参时，能够自动应用的类型转换只有const转换以及数组或函数到指针的转换。**

****

函数模板可以有用普通类型定义的参数：

```C++
template <typename T> ostream &print(ostream &os, const T &obj){
    return os << obj;
}
```

**如果函数参数类型不是模板参数，则对实参进行正常的类型转换：**

```C++
ofstream f("output");
print(f, 10);  
```

上式中，f的类型由ofstream隐式转换为ostream&;

### 函数模板显式实参

在一些情况下，我们希望允许用户控制模板实例化：

```c++
template <typename T1, typename T2, typename T3> T1 sum(T2, T3);  // 编译器无法推断出T1，它未出现在函数参数列表中
```

在此例中，没有任何函数实参的类型可以用来推断T1的类型，因此每次调用sum时调用者都必须为T1提供一个**显式模板实参**：

```C++
auto val3 = sum<long long>(i, lng); // long long sum (int long)
```

T1是显式指定的，显式模板实参在尖括号中给出，位于函数名之后，实参列表之前。**显式模板实参由左至右的顺序与对应的模板参数匹配。**

****

用普通类型定义的函数参数允许进行正常的类型转换，出于同样的原因，对于模板类型参数已经显式指定了的函数实参也进行正常的类型转换：

```C++
template <typename T, typename F = less<T>> //less是个类
int compare(const T &v1, const T &v2, F f = F()){
    if(f(v1, v2)) return -1;
    if(f(v2, v1)) return 1;
    return 0;
}

long lng;
compare(lng, 1024);  //错误，模板参数不匹配
compare<long>(lng, 1024);  //正确：实例化compare(long, long)
compare<int>(lng, 1024);  //正确：实例化compare(int, int)
```

### 尾置返回类型与类型转换

由于尾置返回出现在参数列表之后，它可以使用函数的参数：

```C++
template <typename It> auto fcn(It beg, It end) -> decltype(*beg){
    return *beg;  //返回输入序列中的一个元素的引用
}
```

解引用运算符返回的是一个左值，因此decltype推断的类型为beg表示的元素的类型的引用。

****

标准库的**类型转换**模板定义在头文件type_traits中。

![image-20230420161551484](C:\Users\95769\AppData\Roaming\Typora\typora-user-images\image-20230420161551484.png)

上诉每个模板中都有一个名为type的public成员，表示一个类型。如果不可能（或者不必要）转换模板参数，则type成员就是模板参数类型本身。

举个例子：由于迭代器获取的元素类型只会是引用，当我们想要编写一个fcn函数，但返回一个元素值时，就可以使用remove_reference来获取：

```C++
remove_reference<decltype(*beg)>::type  //type是remove_reference的一个类型成员，beg是一个迭代器
```

这样我们将得到beg引用的元素的类型。

实现在fcn中返回元素中的拷贝：

```C++
template <typename It> auto fcn(It beg, It end) -> typename remove_reference<decltype(*beg)>::type{
    return *beg;  //返回输入序列中的一个元素的引用
}
```

**注意：当我们需要用::指明一个类型成员时，需要在前面声明typename。**

### 函数指针与实参推断

当我们使用一个函数模板初始化一个函数指针或者为一个函数指针赋值时，编译器使用指针的类型来推断模板实参：

```C++
template <typename T> int compare(const T&, const T&);
int (*pf1)(const int&, const int&) = compare;  // pf1指向 int compare(cinst int&, const int&)
```

当参数是一个函数模板实例的地址时，程序上下文必须满足：对每个模板参数，能唯一确定其类型或者值：

```c++
void func(int(*)(const string&, const string&));
void func(int(*)(const int&, const int&));  //重载
func(compare);  //错误，调用哪个？
func(compare<int>);  //显式指定实例化版本，正确
```



### 模板实参推断和引用

关于这个示例：

```C+=
template <typename T> void f(T &p);
```

编译器会引用正常的引用绑定规则；const是底层的，而不是顶层的。

****

当一个函数参数是模板类型参数的一个普通（左值）引用时（形如 T&）,我们只能传递给它一个左值，实参可以是const类型的，也可以不是。如果实参是const的，则T将被推断为const类型：

```C++
template <typename T> void f1(T&);  //实参必须是左值

f1(i);  // i为int，T为int
f1(ci);  // ci为const int，T为const int
f1(5);  // 错误，实参必须是左值
```

如果一个函数的参数类型是const T&，则我们显然可以传递给它任何类型的实参：

```C++
template <typename T> void f2(const T&);  
//所有的T都是int，f2的函数参数全部都为const int&
f2(i);  // i为int，T为int
f2(ci);  // ci为const int，T为 int
f2(5);  // T为int
```

****

当一个函数参数是一个右值引用时，我们可以传递个给它一个右值，类型推断过程类型普通左值引用函数参数的推断过程：

```C++
template <typename T> void f3(T&&);
f3(42);  // 实参是一个int类型的右值，模板参数T是int
```

**当我们将一个左值传递给函数的右值引用参数，且此右值引用指向模板类型参数时，编译器推断模板类型参数为实参的左值引用类型。因此，当我们调用f3(i)时，T为int&而不是int。**

如果我们**间接**创建了一个**引用的引用**，**则这些引用形成了“折叠”。在所有情况下（除了一个例外），引用会折叠成一个普通的左值引用类型**。新标准中，折叠规则扩展到了**右值引用**。只有一种特殊情况引用会折叠为右值引用：右值引用的右值引用。即：

```C++
X& &、X& &&、X&& &都折叠为类型X&
X&& &&折叠为X&&
```

**引用折叠只能引用于间接创建的引用的引用，如类型别名或者模板参数。**

这意味着我们可以对一个左值调用f3：

```C++
f3(i);  // T 为 int&
f3(ci);  // T 为 const int&
```

f3的实例化结果伪代码：

```C++
void f3<int&> (int& &&)；  //T为int& 函数参数是T&&
```

折叠！

```C++
void f3<int&> (int&)；
```

总结：

1）如果一个函数参数是一个指向模板类型的右值引用，则它可以被绑定到一个左值；且

2）如果实参是一个左值，则推断出的模板实参类型将是一个左值引用，且函数参数将被实例化一为一个（普通）左值引用参数。

**即，我们可以将任意类型的实参传递给T&&类型的函数参数，当传递给他一个左值时，函数参数会被实例化为一个普通的左值引用。**

### 理解 std::move

使用move可以获得一个绑定在左值上的右值引用，由于move本质上可以接受任何类型的实参，显然它是一个函数模板。

标准库定义：

```C++
template <typename T>
typename remove_reference<T>::type&& move(T&& t){
    return static_cast<typename remove_reference<T>::type&&>(t);
}
```

move的函数参数T&&是一个指向模板类型参数的右值引用，因此我们可以传递给move任何类型的参数：

```C++
string s1("hi!"), s2;
s2 = std::move(string("bye!"));  //正确，从一个右值移动数据
s2 = std::move(s1);  //正确，但赋值之后，s1的值是不确定的
```

在第二个调用中，传递给move的实参是一个左值s1:

推断出T的类型是string&;

因此，remove_reference的type为string；

move的返回类型为string&&;

move的函数参数t实例化为string& &&，会折叠为string&；

因此，这个调用实例化move<string&\>，即：

```C++
string&& move(string&);
```

我们可以使用static_cast显式地将一个左值转换为一个右值引用。

### 转发

某些函数需要将其一个或多个实参连同类型不变地转发给其它函数：

```C++
// 接受一个可调用对象和另外两个参数的模板
// 对“翻转”的参数调用给定的可调用对象
// flipl是一个不完整的实现；顶层const和引用丢失了
template<typename F, typename T1, typename T2>
void flip1(F f, T1 t1, T2 t2){
    f(t2, t1);
}
```

一般情况下，这个函数都工作的很好，但当我们希望用它调用一个接受引用参数的函数时就会出现问题：

```C++
void f(int v1, int &v2){
    cout << v1 << " " << ++v2 << endl;
}

flip1(f, j, 42)；  //f不会改变j的值
```

因为flip1的实例化是：

```C+=
void flip1(void(*)(int, int&), int t1, int t2);
```

****

**通过将一个函数参数定义为一个指向模板类型参数的右值引用，我们可以保持其对应实参的所有类型信息。**因为引用类型中的const是底层的，所以引用参数使得我们可以保持const属性：

```C++
template<typename F, typename T1, typename T2>
void flip2(F f, T1 &&t1, T2 &&t2){
    f(t2, t1);
}
```

这个版本的flip2对于接受一个左值引用的函数工作得很好，但不能用于接受接受右值引用参数的函数：

```C++
void g(int &&i, int &j){
    cout << j << " " << j << endl;
}
```

当我们试图通过flip2调用g：

```C++
flip2(g, i, 42);  // 错误，不能从一个左值实例化int&&
```

该调用中对g的调用将传递给g的右值引用参数一个左值。

****

在调用中使用forward保持类型信息，forward定义在头文件utility中。for必须通过显式模板实参来调用，它返回该显式实参类型的右值引用。

通常情况下我们使用forward传递定义为模板类型参数的右值引用的函数参数：

```C++
template<typename F, typename T1, typename T2>
void flip2(F f, T1 &&t1, T2 &&t2){
    f(std::forward<T2>(t2), std::forward<T1>(t1));
}
```

**当用于一个指向模板参数类型的右值引用函数参数时，forward会保存实参类型所有的细节。**

## 重载与模板

函数模板可以被另一个模板或一个普通非模板函数重载。与往常一样，名字相同的函数必须具有不同数量或者类型的参数。

当涉及函数模板，函数匹配规则会在这几个方面受到影响：

1）对于一个调用，候选函数包括所有模板实参推断成功的函数模板实例；

2）候选的函数模板总是可行的，因为模板实参推断会排除任何不可行的模板；

3）可行函数按类型转换来排序。可以用于函数模板调用的类型转换是非常有限的；

4）如果恰有一个函数提供比任何其他函数都更好的匹配，则选择此函数。**但是，如果有多个函数提供同样好的匹配，则：**

​		——如果同样好的函数中只有一个是非函数模板，则选择此函数。

​		——如果同样好的函数中没有非模板函数，而有多个函数模板，且其中一个模板比其他模板更特例化，则选择此模板。

​		——否则，此调用有意义。

**正确定义一组重载的函数模板需要对类型间的关系以及模板函数允许的有限的实参类型转换有深刻的理解。**

****

作为一个例子，构造一组名为debug_rep的函数，每个函数都返回一个给定对象的string表示。

最通用版本：

```C++
template <typename T> string debug_rep(const T &t){
    ostringstream ret;
    ret << t;
    return ret.str();  //返回ret绑定的string的一个副本
}
```

此函数可以用来生成一个对象对应的string表示，该对象可以是任意具备输出运算符的类型。

打印指针的版本：

```C++
template <typename T> string debug_rep(T *p){
    ostringstream ret;
    ret << "pointer: " << p;  //打印指针本身的值
    if(p)
        ret << " " << debug_rep(*p);  //打印p指向的值
    else
        ret << " null pointer";
    return ret.str();  //返回ret绑定的string的一个副本
}
```

当我们用一个指针调用debug_rep：

```C++
cout << debug_rep(&s) << endl;
```

两个函数都生成了可行的实例：

```C++
debug_rep(const string* &);  //第一个版本实例化
debug_rep(string*);  //第二个版本实例化，显然这个更匹配
```

当我们用一个const指针调用debug_rep:

```C++
const string *sp = &s;
cout << debug_rep(&sp) << endl;
```

两个函数同样都生成了可行的实例：

```C++
debug_rep(const string* &);  //第一个版本实例化
debug_rep(const string*);  //第二个版本实例化
```

此调用会被解析为debug_rep(T*)，因为规则4（如果同样好的函数中没有非模板函数，而有多个函数模板，且其中一个模板比其他模板更特例化，则选择此模板）说明此模板更特例。

****

定义一个普通非模板版本的debug_rep打印双引号包围的string：

```C++
string debug_rep(const string &s){
    return '""' + s + '""';
}
```

现在，当我们对一个string调用debug_rep时：

```C++
string s("hi");
cout << debug_rep(s) << endl;
```

有两个同样好的函数：

```C++
debug_rep<string>(const string &);
debug_rep(const string &);
```

**对于一个调用，如果一个非函数模板与一个函数模板提供了同样好的匹配，选择非模板版本。**

****

考虑C风格字符串指针和字符串字面常量的调用：

```C++
cout << debug_rep("hi world!") <<endl;  //调用debug_rep(T*)
```

三个版本都可行：

```C++
debug_rep(const T&);  //T被绑定到char[10]
debug_rep(T*);  //T被绑定到const char
debug_rep(const string &);  //要求从const char* 到string的类型转换    
```

debug_rep(T\*)要求数组到指针的转换，debug_rep(const string &)要求const char* 到用户定义的string的转换。两个版本都可以，但T*更特例。

**在定义任何函数之前，记得声明所有重载的函数版本。这样就不必担心编译器由于未遇到希望调用的函数而实例化一个并非我们所需要的版本。**

## 可变参数模板

可变参数模板就是一个可以接受可变数目参数的函数模板或者模板类。可变数目的参数被称为**参数包**。存在两种参数包：**模板参数包**，表示零个或多个模板参数；**函数参数包**，表示零个或多个函数参数。

我们用一个省略号指出模板参数或者函数参数表示一个包。

在一个模板参数列表中，class...或者 typename...指出接下来的参数表示零个或多个类型的列表。

在函数参数列表中，如果一个参数的类型是一个模板参数包，则此参数也是一个函数参数包：

```C++
template <typename T, typename... Args>
void foo(const T& t, const Args& ... rest);
```

对于一个可变参数模板，编译器会推断包中参数的数目：

```c++
int i = 0; double d = 3.14; string s = "how now brown cow";
foo(i, s, 42, d);  //包中有3个参数
foo(s, 42, "hi");  //包中有两个参数
foo(d, s);  //包中有一个参数
foo("hi");  //空包
```

编译器会实例化出四个不同的版本：

```C++
void foo(const int&, const string&, const int&, const double&);
void foo(const string&, const int&, const char[3]&);
void foo(const double&, const string&);
void foo(const char[3]&);
```

当我们需要知道包中有多少元素时，可以使用**sizeof...**运算符：

```C++
template <typename ... Args> void g(Args... args){
    cout << sizeof ...(Args) << endl;  // 类型参数的数目
    cout << sizeof ...(args) << endl;  // 函数参数的数目
}
```

### 编写可变参数函数模板

同样，我们可以使用一个initializer_list来定义一个可接受可变数目实参的函数。但是，**所有实参必须具有相同的类型(或是可以转换为一个公共类型)**。

当我们既不知道想要处理的实参的数目也不知道它们的类型时，可变参数函数是很有用的。

可变参数函数通常是递归的：

```C++
// 用来终止递归的函数，必须在可变参数版本定义之前声明
template <typename T>
ostream &print(ostream &os, const T &t){
    return os << t;
}

template <typename T, typename ... Args>
ostream &print(ostream &os, const T &t, const Args& ... rest){
    os << t << ", ";
    return print(os, rest...);  //打印其它参数
}
```

**当定义可变参数版本的print时，非可变参数版本的声明必须在作用域中。否则，可变参数版本会无限递归。**

### 包扩展

对于一个参数包，除了获取它的大小外，我们能对它做的唯一的事情就是**扩展**。当扩展一个包时，我们还要提供用于每个扩展元素的**模式**。

扩展一个包就是将它分解为构成的元素，对每个元素应用模式，**获得扩展后的列表。**

我们通过在模式右边放一个省略号来触发扩展操作：

```C++
template <typename T, typename ... Args>
ostream &print(ostream &os, const T &t, const Args& ... rest){  // 扩展Args
    os << t << ", ";
    return print(os, rest...);  //扩展rest
}
```

对Args的扩展中，编译器将模式const Args&应用到模板参数包Args中的每个元素。因此，此模式的扩展结果是一个逗号分隔的零个或多个类型的列表，每个类型都形如const type&。

C++还允许更复杂的扩展模式。比如我们可以编写第二个可变参数函数，对其每个实参调用debug_rep，然后调用print打印结果string:

```C++
template <typename... Args>
ostream &errorMsg(ostream &os, const Args&... rest){
    return print(os, debug_rep(rest)...);
}
```

模式debug_rep希望我们对函数参数包中的每个元素调用debug_rep。扩展结果将是一个逗号分隔的debug_rep调用列表，即，下面调用：

```C++
errorMsg(cerr, fcnName, code.num(), otherData, "other", item);
```

就好像：

```C++
errorMsg(cerr, debug_rep(fcnName), debug_rep(code.num()), debug_rep(otherData), debug_rep("other"), debug_rep(item);
```

如果写成这样：

```
return print(os, debug_rep(rest...));
```

会编译失败，因为debug_rep不接受可变实参。

### 转发参数包

我们可以组合使用可变参数模板与forward机制来编写函数，实现将实参不变地传递给其它函数：

```C++
class StrVec{
public:
    template<class... Args> void emplace_back(Args&&...);
};
```

为了保持实参中的类型信息，必须将emplace_back的函数参数定义为模板类型参数的右值引用。

当emplace_back将这些实参传递给construct时，我们必须是使用forward来保持实参的原始类型：

```c++
template <class... Args>
inline void StrVec::emplace_back(Args&&... args){
	chk_n_alloc();
    alloc.construct(first_free++, std::forward<Args>(args)...)
}
```

construct调用的扩展为：

```c++
std::forward<Args>(args)...
```

它既扩展了模板参数包Args，又扩展了函数参数包args。此模式生成如下形式的元素：

```C++
std::forward<Ti>(ti);
```

通过在此调用中使用forward，我们包中如果用一个右值调用emplace_back，则construct也会得到一个右值：

```C++
svec.emplace_back(s1+s2);  //使用移动构造函数
```

**可变参数函数通常它们的参数转发给其它函数。这种函数通常具有与我们的emplace_back函数一样的形式：**

```C++
template <typename... Args>
void fun(Args&&... args){
    // work的实参既扩展了Args又扩展了args
    work(std::forward<Args>(args)...)
}
```

## 模板特例化

当我们不能（或者不希望）使用模板版本时，可以定义类或函数模板的一个特例化版本:

```C++
template <typename T> 
int compare(const T&, const T&);

template <size_t N, size_t M> 
int compare(const (&)[N], const (&)[N]);
```

第二个版本用来处理字符串字面常量，但当我们传递给它字符指针时，还是会调用第一个版本：

```C++
const char *p1 = "hi", *p2 = "mom";
compare(p1, p2);  // 调用第一个模板
compare("hi", "mom");  // 调用有两个非类型参数的版本
```

我们无法将一个指针转换为一个数组的引用。为了处理字符指针，我们可以为第一个版本的compare定义一个**模板特例化**：

```C++
template<>
int compare(const char* const &p1, const char* const &p2){
    return strcmp(p1, p2);
}
```

当我们特例化一个函数模板时，必须为原模版中的每个模板参数都提供实参。**为了指出我们正在实例化一个模板，我们应使用关键字template后跟一个空尖括号对。空尖括号对指出我们将为原模版的所有模板参数提供实参。**

****

一个特例化版本本质上是一个**实例**，而非函数名的一个重载版本，即：特例化版本并不会影响函数的重载。

为了特例化一个模板，原模版的声明必须在作用域中，而且在任何使用模板实例的代码之前。

**模板及其特例化版本应该声明在同一个头文件中，所有同名模板的声明应该放在前面，然后是这些模板的特例化版本。**

****

类模板同样可以特例化，定义特例化版本必须在原模版定义所在的命名空间中特例化它。

为了特例化hash，我们可以向命名空间中添加成员，为了达到这一目的，首先必须打开命名空间：

```C++
// 打开命名空间，以便特例化std::hash
namespace std {
    
}  // 关闭命名空间
```

在花括号对之间的任何定义都将成为命名空间std的一部分:

```C++
// 打开命名空间，以便特例化std::hash
namespace std {
template <>
struct hash<Sales_data>{
	typedef size_t result_type;
    typedef Sales_Data argument_type;
    size_t operator()(const Sales_data& s) const;
};
size_t
hash<Sales_data>::operator()(const Sales_data& s){
    return hash<string>()(s.bookNo) ^
           hash<unsigned>()(s.units.sold) ^
           hash<double>()(s.revenue);
}
}  // 关闭命名空间
```

我们可以在类内或类外定义特例化版本的成员。重载的调用运算符必须为给定类型的值定义一个哈希函数。

假定我们的特例化版本在作用域中，当将Sales_data作为容器的关键字类型时，编译器就会自动使用此特例化版本：

```C++
unordered_multiset<Sales_data> SDset;  //使用hash<Sales_data>和Sales_data中的operator==
```

由于hash<Sales_data>使用私有成员，Sales_data必须将他声明为友元：

```C++
template <class T> class std::hash;  //友元声明所需要的
class Sales_data{
friend class std::hash<Sales_data>;
};
```

由于此定义在std命名空间内，我们必须记得在friend声明中使用std::hash。

****

与函数模板不同，类模板的特例化不需要为其所有的模板参数提供实参。我们可以只指定一部分，或是参数的一部分而非全部特性。

**一个类模板的部分特例化本身是一个模板**，使用它时用户还必须为那些在特例化版本中未指定的模板参数提供实参。

**只能部分特例化类模板，而不能部分特例化函数模板。**

举个例子:

```C++
// 原始的、最通用的版本
template <class T> struct remove_reference{
    typedef T type;
};

// 部分特例化版本，将用于左值引用和右值引用
template <class T> struct remove_reference<T&>{
    typedef T type;
};
template <class T> struct remove_reference<T&&>{
    typedef T type;
};
```

部分特例化版本的名字与原模版的名字相同。对每个未完全确定类型的模板参数，在特例化版本的模板参数列表中都有一项与之对应。

****

我们也可以只特例化特定成员函数而不是特例化整个模板：

```C++
template <typename T> struct Foo{
    Foo(const T&t = T()) : mem(t) { }
    void Bar() { }
    T mem;
};

template <>
void Foo<int>::Bar(){
    //进行int的特例化处理
}

Foo<string> fs;  //实例化Foo<string>::Foo()
fs.Bar();  //实例化Foo<int>::Bar()
Foo<int> fi;  //实例化Foo<int>::Foo()
fi.Bar();  //使用特例化版本
```

当我们用int之外的任何类型使用Foo，其成员都会像往常一样进行实例化。
