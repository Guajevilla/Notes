# C++
## 计算机指令系统
1. 是计算机硬件能识别的语言的集合
2. 是硬件和软件的主要界面（软件=程序+文档）
## 面向对象的基本概念
### 对象
系统中用来描述客观事物的一个实体
### 抽象与分类
分类所依据的原则即为抽象，抽象出同一类别对象的共同属性和行为形成类
### 封装
隐蔽对象的内部细节，只保留有限的对外接口，使用方便，安全性好
### 继承
改造和扩展已有类以形成新类
### 多态
同样的消息作用在不同对象上有可能引起不同的行为
## 程序的开发过程
### 源程序
源语言编写的程序
### 目标程序
源程序通过翻译程序加工以后生成的机器语言程序
### 可执行程序
连接目标程序以及库中的某些文件，生成一个可执行文件。（如windows中的.exe）
（目标程序中可能需要调用一些基本库中的源码，所以需要连接起来）
### 三种不同类型的翻译程序
#### 汇编程序
汇编语言源程序 翻译成 目标程序
#### 编译程序
高级语言源程序(如C++） 翻译成 目标程序
#### 解释程序
高级语言源程序(如C++） 翻译成 机器指令；边翻译边执行（类似debug），执行一句翻译一句
Java程序就是半编译半解释的，目的是为了跨平台（执行过程是先将源程序编译成中间二进制代码，这样的代码在所有平台的Java虚拟机上都可以识别，这样在不同平台的虚拟机上再解释成本机可执行的可执行文件，所以称之为半编译半解释）
而C++则直接编译成本地机器语言代码。
### C++程序开发过程
1. 算法与数据结构设计
2. 源程序编辑
3. 编译
4. 连接
5. 测试
6. 调试
## 计算机中的信息与存储单位
### 计算机基本功能
算术运算 和 逻辑运算
### 计算机中的信息
#### 控制信息
指挥计算机操作（如指令集）
#### 数据信息
计算机程序加工的对象
数据信息包括 数值信息（定点数，浮点数） 和 非数值信息（字符数据，逻辑数据）
### 信息的存储单位
#### 位（bit，b）
数据的最小单位，表示一位二进制信息
#### 字节（byte， B）
八位二进制数字组成（1 byte = 8 bit）
千字节 1KB = 1024 B
兆字节 1MB = 1024 K
吉字节 1GB = 1024 M
## 计算机的数字系统
二进制系统，只有0，1
### 关于八进制和十六进制
#### 八进制
逢8进1，8 = 2^3^，3个二进制位正好构成一个八进制位（三位一转）
#### 十六进制
逢16进1，16 = 2^4^，4个二进制位正好构成一个十六进制位（四位一转）
### 十进制整数转化为R进制
除以R取余
### 十进制小数转化为R进制
乘R取整
0.3125 * 2 = 0.625
0.625 * 2 = 1.25
0.25 * 2 = 0.5
0.5 * 2 = 1.0
0.3125~10~ = 0.0101~2~
## 数据的编码表示
### 原码
直接用 0 表示正号，1 表示负号
缺点：
1. 0 的表示不唯一：+0 = -0（00000 = 10000）
2. 进行四则运算时符号需单独处理，运算规则复杂
### 补码
1. 0的表示唯一
2. 符号位可作为数值参加运算
3. 补码运算的结果仍为补码
4. 补码再求补即为原码

若负数之和为正数 或 正数之和为负数 说明运算结果溢出
#### 模数
n 位二进制整数的模数为2^n^
n 位小数的模数为2
#### 补数
一个数减去另一个数（加一个负数）等于第一个数加第二个数的补数
8 + (-2) = 8 + 10 (mod 12) = 6
#### 反码
1. 负整数
    原码符号位不变（仍是1）其余各位取反
    e.g. x = -1100110 
          [x]~原~ = 11100110
          [x]~反~ = 10011001
2. 正整数
    原码就是补码
#### 补码的计算规则
用反码作为中间码

负数补码= 反码 + 1
正数补码 = 原码
### 小数的表示
#### 定点表示
定好某一位之前是整数，之后是小数（无法表示比较大的数，现在不常用）
#### 浮点表示
浮点数：小数点浮动表示
N = M * 2^E^
E：2的幂次，称为数N的阶码，反映了该浮点数所表示的数据范围
M：N的尾数，其位数反映了数据的精度
### 字符的表示
通过编码表示
西文字符：ASCII码，7位二进制数表示一个字符，最多可表示2^7^ = 128个字符
汉字编码：中国国标 GB 18030
## C++特点
兼容C语言，支持面向过程的程序设计
支持面向对象的方法
支持泛型程序设计方法
## C++基本数据类型
### C++能处理的基本数据类型
整数类型，实数类型，字符类型，布尔类型
### 程序中的数据
常量：在源程序中直接写明的数据，其值在整个程序运行期间不可改变
变量：其值在整个程序运行期间允许改变
### 整数类型
#### int
基本整数类型：int，其字节数在C++标准中没有规定，取决于机器字长
#### 按符号分
有符号型（signed）
无符号型（unsigned）
#### 按数据范围分
短整数（short）
长整数（long）
长长整数（long long）
C++标准中对每种数据类型的字节数与取值范围并没有明确规定，只规定其字节数大小顺序满足：
(signed / unsigned) signed char <= (unsigned) short int <=  (unsigned) int <=  (unsigned) long int <= long long int
### 浮点数类型
**千万不要在程序中比较两个浮点数是否相等，因为浮点数是近似存储的**,用abs(x-y) < 1e-10
单精度（float）
双精度（double）
扩展精度（long double）
同样C++标准中对每种数据类型的字节数与取值范围并没有明确规定，只规定其字节数大小顺序
### 字符类型（char）
容纳单个字符的编码，实质上存储的也是整数，大多数系统是按ASCII码编码的
每一个单独的字符占一个字节，ASCII码是一个七位二进制码，一个字节8位正好
### 字符串类型
C++标准中只有字符串常量，没有字符串变量，C++中可以用字符数组的方式存放（C风格的字符串，现在不鼓励使用）
标准库中有一个string类（C++风格的字符串）
### 常量
直接使用符号（文字）表示的值
#### 整数常量
十进制：若干0~9的数字，但不能以0开头
八进制：前导0+若干个0~7的数字
十六进制：前导0x+若干个0~9的数字及A~F的字母（不区分大小写）

后缀（不区分大小写）：
    后缀 L 表示类型至少是 long
    后缀 LL 表示类型是 long long
    后缀 U 表示 unsigned 类型 
#### 浮点数常量
以文字形式出现的实数
一般形式：12.5
指数形式：0.345E+2，-34.4E-3
浮点常量默认为 double 型，若后缀F（f）即为 float 型，如12.3f
#### C风格字符串常量
一对双引号括起来的字符序列
在内存中按串中字符的排列次序顺序存放，每个字符占一个字节
在末尾添加 '\0' 作为结尾标记
eg："CHINA": 对应存储 C H I N A \0 6个字节
        "a": a  \0  2个字节
        'a': a        1个字节
通过添加前缀可以改变字符常量或字符串常量的类型：
|  前缀  |           含义           |      类型     |
|    u    |  unicode 16字符   |  char16_t  |
|    U   |  unicode 32字符   |  char32_t  |
|    L    |          宽字符         |   wchar_t   |
|   u8   |          UTF-8         |      char     |
### 变量
变量定义最好同时初始化，不然其值就是垃圾值，4种初始化方式：
int a = 0
int a(0)
int a = {0}
int a{0}
其中使用大括号的初始化方式称为列表初始化，列表初始化时不允许信息丢失。例如用double值初始化int变量就会造成数据丢失（ int a = 1.1）
### 符号常量
定义形式：
const int a = 1或
int const a = 1
符号常量在定义时一定要初始化，程序中间不能改变其值
## 基本符号运算符
整数相除结果去掉小数部分。
### 自增、自减
前置：++i 先加1 再参与其他运算
后置：i++ 先取副本参与其他运算，再加1
### 赋值表达式
赋值表达式即 =
复合的赋值运算符：
+=，-=，*=，/=，%=，<<=，>>=，&=，^=，|=
x \*= y + 8     等价于     x = x \* (y + 8)
### 逗号表达式
逗号连接左右两个表达式，先计算逗号左边表达式，再算右边的表达式，都算完后以右边表达式作为结果
e.g: a = 3 \* 5, a * 4    最终输出 a=60
### 逻辑运算符
按优先级排列：
!非 &&与 ||或
其中与 && 有短路特性，如果符号左边的已经为假，其右边的表达式不会计算，直接结果为假
同理或 || 也是，如果符号左边的已经为真，其右边的表达式不会计算，直接结果为真
### 条件表达式
一般类型：
表达式1 ? 表达式2 : 表达式3
表达式1必须是bool类型，若1为True，执行2，否则执行3，最终输出类型是2和3中优先级高的类型
## sizeof运算，位运算
### sizeof
求一个变量或一种数据类型或对象所占字节数的运算。如：`sizeof(short) 或 sizeof x`
### 位运算
#### 按位与 &
每一位进行逻辑与操作。
用途举例：
1. 将某位置0，其它位不变：将char型变量a最低位置0：`a = a & 0xfe` 0xfe即为1111 1110
2. 取指定位：对于char c；int a；取出a的低字节置于c中：`c = a & 0xff` 0xff即为 0000 0000 1111 1111
#### 按位或 |
每一位进行逻辑或操作。
用途举例：
将某些位置1，其它位不变：将int a的低字节置1：`a = a | 0xff`这样低8位全为1，高位不变
#### 按位异或 ^
两位相同，该位为0；两位不同，该位为1
用途举例：
将特定位翻转
#### 按位取反 ~
对一个二进制数每一位都取反
#### 移位（<<, >>）
左移后，低位补0，高位舍弃；相当于乘以2（当整数左移出现负数说明溢出）
右移后，低位舍弃，高位若是无符号数：补0，高位若是有符号数：补 符号位；相当于除以2
## 类型转换
### 显式转换
类型说明符（表达式）：`int (a)`
（类型说明符）表达式：`(int) a`
类型转换操作符 <类型说明符>（表达式）（C++风格）`static_cast<int>(a)`
类型转换操作符包括：const_cast, dynamic_cast, reinterpret_cast, static_cast(常用)
static_cast：静态转换，编译时执行，内置基本类型间的转换，没有运行时类型检查来保证表达式的安全性
const_cast：去常转换，可以转化为常量或常量转变量
reinterpret_cast：重解释型转换，可将任意内置数据类型转换为其他数据结构；也可将任意类型的指针转换为其他类型
dynamic_cast：将表达式转化为type_id类型对象，type_id必须是类的指针，类的引用或空指针
## if语句
`if (x > y) cout << x`
## switch语句
```C++
int day;
switch(day){
case 1:    // 这是共用表达式的写法
case 2:
case 3:
case 4:
case 5:
    cout << “Weekday” << endl; 
case 0:
case 6:
    cout << “Weekend” << endl; 
    break;
default：
    cout << "Out of range";
    break;
}
```
## while语句
```C++
int i = 1, sum = 0;
while (i <= 10)
{
    sum += i;
    i++;
}
```
## do while语句
先执行一遍循环语句再判断：将输入数字各位反转
```C++
int n, right_digit;
cout << "Enter the number:";
cin >> n;
cout << "The reversed number is ";
do{
    right_digit = n % 10;
    cout << right_digit;
    n /= 10;
}while (n != 0);
cout << endl;
```
## for语句
```C++
int sum = 0, n = 1000;
for (int i = 1; i <= n; i++)
    sum += i；
```
### 范围for循环（C++11）
for (声明 ：表达式) 主要用于遍历容器中的序列
```C++
int array[3] = {1, 2, 3};    //数组是最简单的容器
for (int &e: array)
    e += 2;
//int *p;            //等效
//for (p = array; p < array + sizeof(array)/sizeof(int); ++p)
//    *p += 2;
```
## 自定义类型
typedef 已有类型名 新类型名：`typedef double Area, Volume`
using 新类型名 = 已有类型名：`using Area = double`
### 枚举类型
不限定作用域枚举类型：
enum 枚举类型名 {变量值列表};
```C++
enum Weekday
{SUN, MON, TUE, WED, THU, FRI, SAT};
Weekday a;
a = MON;
cout << a;  // 输出是1
```
枚举元素有默认值，依次为0, 1, 2....   即默认SUN = 0, MON = 1, ... , SAT = 6
也可在声明时另行指定枚举元素的值：
```C++
enum Weekday
{SUN = 7, MON = 1, TUE, WED, THU, FRI, SAT};
```
要是不写MON=1，那么MON的值是8，现在上面的代码TUE默认值是2。
枚举元素是常量，不能对其赋值；不能写：SUN = 0，但可以先强制类型转换，但若整数不在枚举范围内，类型转换会失败。
枚举值可以进行关系运算。
整数值不能直接赋给枚举变量，但枚举值可以赋给整型变量。
```C++
enum GameResult
{WIN, LOSE, TIE, CANCEL};

int main()
{
    GameResult result;
    enum GameResult omit = CANCEL;    // 和上面的定义方法等价
    for (int i = WIN; i <= CANCEL; i++)
    {
        result = GameResult(count);
        if (result == omit)
            cout << "The game is cancelled" << endl;
        else
        {
            if result == WIN
                cout << "We won" << endl;
            else
                cout << "We lost" << endl;
        }
    }
    return 0;
}
```
## auto类型与decltype类型
auto：编译器通过初始值自动推断变量类型
decltype：定义一个变量与某一表达式的类型相同，但并不用表达式的值初始化该变量：decltype(i) j = 2
## 函数
`double power (double x, int n)`
## 函数的参数传递
形参在调用前不占存储空间。
实参可以是常量变量或表达式
单向传递 == 值传递
双向传递 == 引用传递
常引用做参数可保证实参数据的安全性（因为引用的内存开销更小，有时候会想使用引用的办法但是却不想改变值，这时可以用常引用）
## 引用类型
`int &ri = i`
一旦一个引用被初始化后，就不能改为指向其他对象
引用可以作为形参
```C++
void swap(int& x, int& y)
{
    int t = x;
    x = y;
    y = t;
}
```
## 含有可变参数的函数
1. 若所有实参类型相同，可传递一个名为 initializer_list 的标准库类型
2. 若所有实参类型不同，可编写可变参数的模板（TODO：第九章）

initializer_list 用于表示某种特定类型的值的数组，其定义在同名头文件中（类模板）
`initializer_list<int> ls;`
initializer_list对象中的元素永远是常量值，因此我们无法改变对象中元素的值
含有initializer_list形参的函数也可同时拥有其他形参
## 内联函数
声明时使用关键字 inline ，但inline相当于一个建议，可能编译器不会将该函数作为内联处理
内联函数体中不能有循环和switch语句
内联函数的定义必须出现在第一次被调用前
内联函数中不能进行异常接口声明
```C++
const double PI = 3.1415926;
inline double calArea(double radius){
    return PI * radius * radius;
}
```
## constexpr（++11）
constexpr 修饰的函数在其所有参数都是constexpr（常量表达式）时一定返回 constexpr，函数体内只有一个return语句
```C++
constexpr int get_size(){return 20;}

constexpr int foo = get_size()
```
## 带默认参数值的函数
`int add(int x, int y = 5, int z = 5){}`
函数原型的声明 和 函数的定义 哪个在前，默认值就写在哪，在后面的那个**不能**写默认值。
## 函数重载
C++允许功能相近的函数在相同的作用域内以相同函数名声明，从而形成重载，方便使用。
```C++
int add(int x, int y);
int add(int x, int y, int z);
```
重载函数的形参必须**个数**或**类型**不同
编译器**不以**形参名，**返回值**来区分重载函数
## C++系统函数
数学函数头文件include <cmath>
## 对象与类基本特点
对象：现实中对象的虚拟，具有属性和行为
类：同一类对象的共同属性和行为，对象是类的实例
### 抽象
对同一类对象的共用属性和行为进行概括，形成类
抽象的实现是 类
数据抽象（成员变量）和代码抽象（成员函数）
### 封装
将抽象出的数据，代码成员相结合，将其视为一个整体
增强安全性，简化编程，使用者不必了解的具体实现细节，只需通过外部接口，以特定的访问权限来使用类成员
### 继承
在已有类的基础上进行拓展形成新的类
### 多态
同一名称，不同功能实现方式
目的是达到行为标识统一，减少程序中标识符个数
## 类和对象的定义
```C++
class 类名称
{
public:
    公有成员（外部接口）
private:
    私有成员
protected:
    保护型成员
};
```
可以直接在类内初始化变量，也可在构造函数中
### 类成员的访问控制
#### public
任何外部函数都可以访问公有类型数据和函数
#### private
只允许本类中函数访问，任何类外部函数都不可以访问私有类型数据和函数
private关键字可省略，即 未说明的都是private
#### protected
与private类似，在继承和派生时允许（TODO）
```C++
class Clock
{
public:
    Clock(int newH, int newM = 0, int newS = 0);
    void setTime(int newH = 0, int newM = 0, int newS = 0)；
    void showTime();
private:
    int hour, minute, second;
};

Clock::Clock(int newH, int newM = 0, int newS = 0): hour(newH), minute(newM), second(newS){    // 简单初始化
}

void Clock::setTime(int newH, int newM, int newS)
{
    hour = newH;
    minute = newM;
    second = newS;
}

void Clock::showTime()
{
    cout << hour << ":" << minute << ":" << second;
}

int main(){
    Clock myClock;
    myClock.setTime(8, 30, 30);
    myClock.showTime();
    return 0；
}
```
## 构造函数
函数与类名相同
不能定义返回值类型，也不能有return语句
可以是内联函数，可以重载，可以有默认值
`Clock::Clock(int newH, int newM = 0, int newS = 0): hour(newH), minute(newM), second(newS){    // 简单初始化
}`
### 默认构造函数
调用时可以不需要实参的构造函数，可能是参数表为空，也可能全部参数都有默认值
类内不能同时出现多个默认构造函数！
很多情况下都会给类添加默认构造函数，比如在使用组合类的时候，如果在组合类中的构造函数中没有写部件类的构造函数，这时会自动调用部件类的默认构造函数，此时如果没有默认构造函数就会报错。
```C++
Clock();
Clock(int newH = 0, int newM = 0, int newS = 0);
Clock::Clock(): hour(0), minute(0), second(0){}
```
这两个构造函数不能同时出现，不然会出现**编译错误**
### 隐含生成的构造函数
程序中未定义构造函数时，编译器自动生成
参数列表为空，不为数据成员设置初始值
若类内定义了成员初始值，则使用类内初始值，否则以默认方式初始化（基本数据类型默认方式初始化就是垃圾值）
#### = default
当自己定义了构造函数，但仍想编译器构造一个默认构造函数时，使用 =default
```C++
Clock() = default;
Clock(int newH = 0, int newM = 0, int newS = 0);
}
```
## 委托构造函数
很多构造函数初始化算法相同，函数体相同，这时不想重复写同一函数体多次，这样以后修改也好改
```C++
Clock::Clock(int newH, int newM, int newS): hour(newH), minute(newM), second(newS){}
Clock::Clock(): hour(0), minute(0), second(0){}        //默认构造函数
```
可写成：
```C++
Clock::Clock(int newH, int newM, int newS): hour(newH), minute(newM), second(newS){}
Clock::Clock(): Clock(0, 0, 0){}
```
## 复制（拷贝）构造函数
用一个已经存在的对象初始化一个新的对象
默认的复制构造函数只会实现两个对象的数据成员的一一对应复制
复制构造函数的形参是本类对象的**引用**
也不能使用return语句


类名(const 类名 &对象名)
```C++
class 类名称
{
public:
    类名(const 类名 &对象名)
    公有成员（外部接口）
private:
    私有成员
protected:
    保护型成员
};
```
### 复制构造函数被调用的 3 种情况
1. 定义一个对象时，用本类的另一个对象作为初始值，发生复制构造
2. 若函数形参是类的对象，调用函数时，将使用实参对象初始化形参对象，发生复制构造
3. 若函数的返回值是类的对象，函数执行完返回主调函数时，将使用return语句中的对象初始化一个临时无名对象传递给主调函数，发生复制构造（这种情况也可以通过移动构造避免不必要的复制 TODO）
### 若不希望对象被复制构造
C++98：将复制构造函数声明为private，且不提供函数的实现
C++11：= delete；`Clock(const Clock& p) = delete`
## 析构函数
析构函数完成对象被删除前的一些清理工作，对象的生存期结束时系统自动调用析构函数
程序中未定义析构函数时，编译器自动生成默认的析构函数，其函数体为空
没有参数表，没有return
~类名();

```C++
class Point
{
public:
    Point(int xx, int yy);
    Point(const Point &p);
    ~Point();
private:
    int x, y;
};

Point::Point(int xx, int yy)
{
    x = xx;
    y = yy;
}

Point::~Point(){
}
```
## 类的组合
类中的成员可以是另一个类的对象，可以在已有抽象的基础上实现更复杂的抽象
### 构造函数设计
不仅要负责对本类中的基本类型成员数据初始化，还要对 对象成员初始化
声明形式：
类名::类名（对象成员所需形参，本类成员形参）:
    对象1(参数)，对象2(参数)，......{
// 函数体其他语句    
}

#### 构造时的初始化次序
首先对构造函数初始化列表中出现的成员（包括基本类型成员和对象成员）进行初始化；
初始化顺序是成员在类体中**定义的次序**
```C++
class Point
{
public:
    Point(int xx, int yy);
    Point(const Point &p);
    ~Point();
    int getX(){return x;}
    int getY(){return y;}
private:
    int x, y;
};

Point::Point(Point &p)    //拷贝构造函数
{
    x = p.x;
    y = p.y;
}

Point::Point(int xx, int yy)
{
    x = xx;
    y = yy;
}

Point::~Point(){
}


class Line
{
public:
    Line(Point xp1, Point xp2);
    Line(Line &l);
    double getLen() {return len;}
private:
    Point p1, p2;
    double len
};

Line::Line(Point xp1, Point xp2):p1(xp1), p2(xp2){
    double x = static_cast<double>(p1.getX() - p2.getX());
    double y = static_cast<double>(p1.getY() - p2.getY());
    len = sqrt(x * x + y * y);
}

Line::Line(Line &1):p1(l.p1), p2(l.p2){    //组合类的拷贝构造函数
    len = l.len;
}
```
## 前向引用声明
类应该先声明后使用
若需要在某个类的声明之前引用该类，则需要前向引用声明
前向引用声明只引为程序入一个标识符，具体声明在其他地方
```C++
class B;    //前向引用声明
class A{
public:
    void f(B b);
};


class B{
public:
    void g(A a);
};
```
在提供B的完整声明之前，不能声明B类的对象，也不能在内联函数中使用B的对象。
当使用前向引用声明时，只能使用被声明的符号B，不能涉及类的细节
```C++
class A;    //前向引用声明
class B{
public:
    A x;    //错误：类A的声明尚不完善
};


class A{
public:
    B y;
};
```
## UML简介
有三个基本部分：
事物（Things）
关系（Relationships）（依赖关系，关联，包含--聚集，继承--泛化）
图（Diagrams）
## 结构体
struct是一种特殊的类
类的缺省访问权限是private
结构体的缺省访问权限是public
### 什么时候用结构体不用类
定义主要用来保存数据，而没有什么操作的类型
### 结构体初始化
若 结构体所有数据成员都是public，没有用户定义的构造函数，没有基类和虚函数 其变量初始化可用：
类型名 变量名 = {初值1，初值2,......}
```C++
struct Student{
    int num;
    string name;
    char sex;
    int age;
};

Student stu = {2008, "Suede", 'M', 19};
```
## 联合体 union
缺省访问权限是public
union 联合体名称{
    公有成员
private:
    私有成员
protected:
    保护型成员
};
成员共用同一组内存单元
任何两个成员不会同时有效，分配空间按最多字节数的成员分配
```C++
union Mark{
    char grade;
    bool pass;
    int percent;
};
```
无名联合体
```C++
union{
    float f;
    int i;
};
//程序里可直接写：
i = 10;
f = 2.2;    //再定义f时i的值就不存在了
```
一个例子:
```C++
class ExamInfo{
private:
    string name;    //课程名
    enum {GRADE, PASS, PERCENTAGE} mode;    //计分方式
    union{
        char grade;
        bool pass;
        int percent;
    };
public:
    //三种构造函数，分别对应三种不同的计分方式
    ExamInfo（string name, char grade）:name(name), mode(GRADE), grade(grade){}
    ExamInfo（string name, bool pass:name(name), mode(PASS), pass(pass){}
    ExamInfo（string name, int percent:name(name), mode(PERCENTAGE), percent(percent){}
    void show();
};

void ExamInfo::show(){
    cout << name << ": ";
    switch (mode){
        case GRADE: cout << grade; break;
        case PASS: cout << (pass ? "PASS" : "FAIL"); break;
        case PERCENTAGE: cout << percent; break;
    }
}
```
## 枚举类(C++11)
enum class 枚举类型名：底层类型{枚举值列表}；
例子：
```C++
enum class Type {General = 1, Light, Medium, Heavy};    //底层类型默认是int
enum class Type:char {General, Light, Medium, Heavy};
```
### 枚举类优势
1. 强作用域：其作用域限制在枚举类中
例子：使用Type的枚举值General时：Type::General ，这样可以避免冲突
2. 转换限制：枚举类对象不可以与整型隐式地互相转换
3. 可以指定底层类型
```C++
enum class Side {Right, Left};
enum class Thing {Right， Wrong};
Side s = Side::Right;
Thing w = Thing::Right;
cout << (s == w);    //编译错误，无法直接比较不同枚举类
return 0;
```
## 作用域
### 函数原型作用域
就是函数的形参表始于（，终于）
### 局部作用域（块）
函数体，对每个局部变量都有相应的局部作用域
### 类作用域
类体和函数成员体
#### 在类作用域外访问类的成员：
静态成员：通过类名，或者该类的对象名，对象引用访问
非静态成员：通过类名，或者该类的对象名，对象引用，对象指针访问
### 文件作用域
该C++文件
## 对象的生存期
### 静态生存期
### 动态生存期
## 静态数据成员
用static声明
为类的所有对象共享，静态数据成员具有静态生存期
必须在**类外定义和初始化**
调用的时候要加上 类名:: 形式修饰（类内外）

```C++
class Point
{
public:
    Point(int xx, int yy);
    Point(const Point &p);
    ~Point(){count--;}
    int getX(){return x;}
    int getY(){return y;}
    int showCount(){return count;}
private:
    int x, y;
    static int count;    //静态数据成员声明
};

int Point::count = 0;    //静态数据成员定义和初始化

Point::Point(Point &p)    //拷贝构造函数
{
    x = p.x;
    y = p.y;
}

Point::Point(int xx, int yy)
{
    x = xx;
    y = yy;
    Point::count += 1；
}
```
这种情况下有一个问题：
当Point类还未调用过时，如果我想知道count的值怎么办？由于count是私有，这时在类外无法直接访问。
## 友元
通过将一个模块声明为另一模块的友元，这个模块就能访问到本来被隐藏的信息。
为了保证数据的完整性，及数据封装与隐藏的原则，要求慎用友元
可以声明友元类和友元函数
### 友元函数
在类声明中由 friend 关键字修饰的 **非成员函数**，在其函数体中可以直接通过对象名访问 private 和 protected 成员
```C++
class Point
{
public:
    Point(int x=0, int y=0): x(x), y(y){}
    int getX(){return x;}
    int getY(){return y;}
    friend float dist(const Point &a, const Point &b);
private:
    int x, y;
};

//这样可以提高访问效率；传递对象的时候一般用引用，因为传值的话效率不高
float dist(const Point &a, const Point &b){
    double x = a.x - b.x;
    double y = a.y - b.y;
    return static_cast<float>(sqrt(x*x + y*y));
}
```
### 友元类
友元类的所有成员都可以访问对方类的私有成员
类的友元关系是单向的
```C++
class A
{
    friend class B;    //声明友元类
public:
    void display(){cout << x << endl;}
private:
    int x;
};


class B
{
public:
    void display();
    void set(int i);
private:
    A a;
};

void B::set(int i){
    a.x = i;
}

void B::display(){
    a.display();
}
```
## 共享数据的保护
### 常类型
常对象：必须初始化，不能被更新 `const Student stu(2018, "Mike");`

常成员:
常成员函数：`int getMark("Mike")const;`
使用时不更新对象的数据成员，通过常对象，只能调用其常成员函数
const关键字可用于重载函数的区分
常数据成员
```C++
class R
{
public:
    R(int r1, int r2): r1(r1), r2(r2){}
    void print();
    void print() const;
private:
    int r1, r2;
};

void R::print(){
    cout << r1 << ";" << r2 << endl;
}

void print() const{
    cout << r1 << ";" << r2 << endl;
}


int main(){
    R a(5, 4);
    a.print();        //如果没有第一个不带const的函数，会调用常成员函数
    const R b(20, 52);
    b.print();        //只能调用常成员函数
    return 0;
}
```
常引用：`const int &a;`友元中常用（高效率又有安全性）
常数组：`const int a[10];`
常指针：
## 多文件结构和预编译命令
类的声明：.h文件
类的实现：.cpp文件
类的使用：main()所在的 .cpp文件
自己写的 .h文件用""括起来。系统自带的用<>
### 编译与连接
以point.h，point.cpp，main.cpp 为例：
![Screenshot_2020-02-15-22-19-14-793_tv](_v_images/20200215222150257_3565.jpg =704x)
### 编译预处理指令
#### #include包含
#include "文件名"现在当前目录下搜索，若无再按标准方式搜索
#### #define宏定义
很多情况下定义符号常量被const取代，定义带参数宏被内联函数取代
#### #undef 取消定义
取消由 #define 定义的宏
#### #if，#else，#elif，#ifdef，#ifndef，#endif条件编译
当满足 #if 后的表达式时编译后续 #endif 前正文
#ifdef 如果标识符定义过且未被undef删除
#ifndef 用于避免多次重复包含头文件
### 外部变量
extern
### 外部函数
使用前先声明即可
### 匿名命名空间
在匿名命名空间中定义的变量和函数不会暴露给其他编译单元
```C++
namespace{
    int x;
    void f(n){n++;}
}
```
## 数组
`int a[10][20];`
数组元素在内存中顺序存放，他们的地址是连续的
二维数组的存储：按行存放
数组名是数组**首元素**的内存地址，数组名是一个常量，不能被赋值
初始化：
```C++
static int a[5] = {0, 1, 2, 3, 4};
static int a[10] = {0, 1, 2, 3, 4};    //只给一部分元素指定初值
static int a[] = {0, 1, 2, 3, 4};    //列出全部数组元素初值时，可以不指定数组长度

static int a[3][4] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11};
static int a[3][4] = {{0, 1, 2, 3}, {4, 5, 6, 7}, {8, 9, 10, 11}};    //两种定义方式一样
static int a[3][4] = {{0, 1}, {4}, {8, 9, 10}};    //只给一部分元素指定初值
static int a[][4] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11};    //列出全部数组元素初值时，可以不指定数组长度
static int a[][4] = {{0, 1, 2, 3}, {4, 5, 6, 7}, {8, 9, 10, 11}};
```
static 数组初始值为0，其他形式初值是垃圾值，当部分元素初始化时，剩下的未显示初始化元素初始化为0
数组名作参数，形参和实参都应该是数组名，类型要一样，**传的是数组首地址，对形参数组的改变会直接影响到实参数组**
```C++
void rowSum(int a[][4], int nRow）{    //这样可以输入行数不定的数组
    for (int i = 0; i < nRow; i++){
        for (int j = 1; j < 4; j++)
            a[i][0] += a[i][j];
    }
}
```
对象也可以组成对象数组，每个单个对象都会调用其构造函数
`Point a[2] = {Point(1, 2), Point(3, 2)};`
如果没有为数组元素指定显式初始值，数组元素调用默认构造函数（所以最好对象要定义一个默认构造函数）
## 指针
```C++
static int i;
static int *ptr = &i;    //&取地址
cout << *ptr;     //*寻址

short a[4];
short *p = a;    //数组名相当于首地址
```
可以用一个已有合法值的指针初始化另一指针变量
不可以用一个内部非静态变量初始化static指针
变量和指针类型必须一致，且变量必须在指针初始化前声明过
定义指针时写int是为了在用指针的时候知道所指对象的大小
指针运算符：*    寻址
地址运算符：&   取地址
### 指针变量的赋值
指针名 = 地址
整数0可以表示空指针（C++11规定是：nullptr关键字）
允许定义指向void类型的指针，表示该指针可以存放任何类型对象的地址，这种指针只能用于存放地址，不能被访问`void *general;`
如果想访问可以先强制类型转化再访问：`int *ptr = static_cast<int*>(general);`
### 指向常量的指针
const 指针`const int *ptr;`
不能通过指向常量的指针改变所指对象的值，但指针本身可以改变，可以指向别的对象
```C++
//即使原来i不是常量，p定义的是指向常量的指针，也不能改变a的值，否则编译出错
int i, j;
const int *p = &i;    //&取地址
p = &j;               //p指针本身可改变
*p = 1;               //编译出错
```
### 指针类型是常量
指针本身不可以改变
`int * const ptr = &a;`
### 指针的算术运算
加减，表示指针向前后移
前后移动的时候，移动的距离取决于数据类型，即如果是int，自增1，则后移到下一个int数
### 指针的关系运算
指向相同类型数据的指针间可以进行各种关系运算。
此外有一个例外，指针可以与0进行判断，判断其是否空指针
### 指向数组的指针
```C++
int a[10], *p;
p = &a[0];
p = a;        //等价
```
等效的形式：`*p == a[0];    *(p+i) == *(a+i) == a[i] == p[i];`
### 指针数组
```C++
line1[] = {1};
line2[] = {1, 2};
line3[] = {3, 2, 1};
int *p[3] = {line1, line2, line3};
cout << p[2][0];        //输出3
```
这里指针可以当数组名用，但是一定要清楚指针数组的行与行之间不是连续的
### 指针做函数参数
需要数据双向传递时使用，且需要传递一组数据时，只传首地址运行效率更高
1. 双向传递
```C++
void splitFloat(float x, int *intPart, float *fracPart)
{
    *intPart = static_cast<int>(x);
    *fracPart = x - *intPart;
}

float x = 12.3;
float f;
int n;
splitFloat(x, &n, &f);
```
2. 为了运行效率更高：

```C++
void print(const int *p, int n)
{
    for (int i = 0; i < n; i++)
        cout << *(p+i) << ", ";
}

float x = 12.3;
float f;
int n;
splitFloat(x, &n, &f);
```
### 指针类型的函数
函数的返回值是指针
存储类型 数据类型 *函数名();
此时，**不能将非静态局部地址用作函数返回值**！（非静态局部变量在函数运行结束时会被释放）
可以返回数组，数组的某个元素或者通过动态内存分配 new 操作取得内存地址，但在外层空间后要记得 delete 释放其内存。（防止内存泄露）
```C++
int *print(int *p, int n)
{
    for (int i = 0; i < n; i++)
    {
        cout << *(p+i) << ", ";
        *(p+i) += 1;
    }
    //return &p[0];
    return p;
}

int a[] = {1,2,3,4,5};
int &p = *print(a, 5);
```
```C++
int* search(int *a, int num){
    for (int i=0; i<num; i++)
    {
        if (a[i] == 0)
            return &a[i];
    }
}

int main(){
    int array[5];
    for (int i = 0; i < 5; i++)
        cin >> array[i];
    int* zeroptr = search(array, 5);
    return 0;
}
```
### 指向函数的指针
存储类型 数据类型 （*函数指针名）()        //多了括号
函数指针指向程序代码存储区
#### 函数指针的典型用途
**实现函数回调**：
通过函数指针可以调用函数（如在另一个函数中作为参数传入）
调用者不关心谁是被调用者
```C++
int compute(int x, int y, int(*func)(int, int))    //函数需声明原型限制条件
{
    return func(x, y);
}

int max(int x, int y)
{
    return ((x > y ? x: y);)
}

int min(int x, int y)
{
    return ((x < y ? x: y);)
}

int sum(int x, int y)
{
    return x + y;
}


int x = 1;
int y = 5;
cout << compute(x, y, &max) << endl;
cout << compute(x, y, &sum);        //这里函数名前要加 &
```
### 对象指针
```C++
Point a(5, 10);
Point *ptr;
ptr = &a;
//通过指针访问对象成员
ptr -> getx();
(*ptr).getx();    //等价
```
### this指针
指向当前对象自己的指针
隐含与类的每一个非静态成员函数中，用于指出成员函数所操作的对象
当通过一个对象调用成员函数时，系统先将该对象的地址赋给 this，然后调用成员函数，成员函数对对象的数据成员进行操作时，就隐含使用了this指针。
```C++
int Point::getX()
{
    return x;
    //等价于
    return this -> x;
}
```
## 动态内存分配
动态申请内存：new，可用于程序中位置大小数据
new 类型名T（初始化参数表）
功能：
在程序执行期间申请用于存放T类型对象的内存空间，并依初值列表赋以初值
返回结果：
成功：T类型的指针；失败：抛出异常

释放内存：delete
delete 指针p
p必须是new操作的返回值
```C++
Point *p1 = new Point;
delete p1;
//删除的只是原对象，而不是指针，所以这个指针变量还可以接受别的赋值
p1 = new Point(1, 2);
delete p1;
```
### 申请和释放动态数组
分配：new 类型名T [数组第1维长度][第2维长度]...

释放：delete[] 数组名p
不写[]，释放的仅仅是数组首元素的地址
```C++
Point *p = new Point[2];
p[0].move(5, 10);
p[1].move(15, 2);
delete[] p;

char (*fp)[3];
fp = new char[2][3];
delete[] fp;
```
多维数组：
```C++
int (*cp)[9][8] = new int[7][9][8];        //三维数组需要二维指针存放地址
for (int i = 0; i < 7; i++)
    for (int j = 0; j < 9; j++)
        for (int k = 0; k < 8; k++)
            *(*(*(cp + i) + j) + k) = (i*100+j*10+k);    //需要三级运算

for (int i = 0; i < 7; i++){
    for (int j = 0; j < 9; j++){
        for (int k = 0; k < 8; k++)
            cout << cp[i][j][k] << " ";
        cout << endl;
    }
    cout << endl;
}
delete[] cp;
```
### 将动态数组封装成类
更加简洁便于管理
```C++
class ArrayOfPoints{            //动态数组类
public:
    ArrayOfPoints(int size):size(size){
        points = new Point[size];
    }
    ~ArrayOfPoints(){
        delete[] points;
    }
    Point& element(int index){        //引用类型，返回引用，这样可以由外部改变类内部的值
        assert(index >= 0 && index < size);
        return points[index];
    }
private:
    Point *points;    //指向动态数组首地址
    int size;
};


ArrayOfPoints points(count);
points.element(0).move(5, 0);
points.element(1).move(5, 2);
```
## 智能指针（C++11）
### unique_ptr（一对一）
不允许多个指针共享资源，可用标准库中move函数转移指针
### shared_ptr
多个指针共享资源
### weak_ptr
可复制shared_ptr，但其构造或者释放对资源不产生影响
## vector
可封装任何类型的动态数组，自动创建和删除
```C++
#include <vector>
vector<double> arr(5);
arr.size()        //获取容器长度
cin >> arr[0];    //vector中重载了[]操作，所以可以直接下标访问

double average(const vector<double> &arr)    //引用效率更高
{
    double sum = 0;
//    for (i = 0; i < arr.size(); i++)
//        sum += arr[i];
    for (auto i = arr.begin(); i != arr.end(); ++i)
        sum += *i;    //有*号
//    for (auto e: arr)
//        sum += e;    //直接用
    return sum/arr.size();
}

cout << average(arr);
```
vector对象名不表示数组首地址
## 深层复制和浅层复制
浅层复制：实现对象间数据元素的一一对应复制
深层复制：当被复制对象数据成员是指针类型时，不是复制该指针成员本身，而是指针所指对象
也就是说浅层复制复制的时候直接复制的是地址值，此时，被复制指针和复制指针指向同一地址！
这个时候，若改变一个指针指向对象的值，另一个也会变；而且，在析构的时候会出现**运行错误**。这是因为先析构被复制指针时已经将对应地址释放，在析构复制指针时找不到这个地址了

这种时候要自己定义拷贝构造函数，再申请另一块内存，存下指针所指对象的值
## 移动构造（C++11）
将源对象的控制权全部交给目标对象
当临时对象在被复制后，就不再被利用了，这时完全可以把临时对象的资源直接移动，避免多余的复制操作
![Screenshot_2020-02-17-23-24-11-928_tv](_v_images/20200217232716631_9431.jpg =700x)
&&右值引用，函数返回的临时变量是右值
```C++
class IntNum{
public:
    IntNum(int x = 0):xptr(new int(x)){
        cout << "Constructor" << endl;
    }
    IntNum(const IntNum &n):xptr(new int(*n.xptr)){    //深层复制构造函数
        cout << "Copy constructor" << endl;
    }
    IntNum(IntNum &&n):xptr(n.xptr){    //移动构造函数
        n.xptr = nullptr;
        cout << "Move constructor" << endl;
    }
    ~IntNum(){
        delete xptr;
        cout << "Destructor" << endl;
    }
    int getInt(){return *xptr;}
private:
    int *xptr;
}

IntNum getNum(){    //用复制构造函数，整个过程就会调用构造函数，拷贝构造和析构
    IntNum a;
    return a;
}

IntNum getNum(){    //用移动构造函数，整个过程调用构造函数，移动构造和析构，不用将一块内存赋值又删除
    IntNum a;
    return a;
}
```
## C风格字符串
各个字符连续顺序存放，每个字符占一个字节，以'\0'结尾
所以这种时候，相当于一个数组，可以用char常量指针指向首地址的方式使用
```C++
const char *string1 = "program";
char str[8] = {'p', 'r', 'o', 'g', 'r', 'a', 'm', '\0'};
char str[8] = "program";
```
## string
#include <string>
常用操作：s+t；s = t（复制）；s < t（字典序）；s[i]；s.length()
输入整行字符串：getline()
```C++
getline(cin, s);        //默认回车作为分隔符
getline(cin, s, ',');    //逗号作为分隔符
```
## 继承与派生
直接基类，间接基类，派生类
```C++
class Derived: public Base1, private Base2
{
public:
    Derived();
    ~Derived();
}
```
默认情况下派生类包含了基类中除构造和析构函数之外的所有成员。
C++11规定可用using语句继承基类构造函数：`using B::B`
如果派生类中声明了一个和基类中同名的成员，基类成员被其覆盖
### 继承方式
#### public
可访问基类public，protected，不可直接访问private
通过派生类的对象，只能访问从基类继承的public
访问属性都保持不变 
#### private
可访问基类public，protected，不可直接访问private
通过派生类的对象，不能访问从基类继承的任何函数
访问属性都变成private
#### protected
可访问基类public，protected，不可直接访问private
通过派生类的对象，不能访问从基类继承的任何函数
访问属性 public 变 protected，其他保持不变 
## 基类与派生类的转换
公有派生类对象可以直接被当做基类对象使用
```C++
class Base{
public:
    void display() const{
        cout << "Base::display()" << endl;
    }
};


class Derived: public Base{
public:
    void display() const{
        cout << "Derived::display()" << endl;
    }
};


void fun(Base *ptr){
    ptr -> display();
}

Base base;
Derived derived;
fun(&base);            //输出"Base::display()"
fun(&derived);        //也是输出"Base::display()"
```
**不要重新定义继承而来的非虚函数**
## 派生类的构造函数
默认基类构造函数不被继承，派生类需要定义自己的构造函数
C++11规定，可用using语句继承基类构造函数 `using B::B;`，但是只能初始化从基类继承的成员

一般都需要自定义构造函数：
新增成员需要在派生类的构造函数中进行初始化
继承的成员自动调用基类构造函数进行初始化，但派生类构造函数还需要给基类构造函数传递参数
派生类名::派生类名（基类所需形参，本类成员所需形参): 基类名(参数表)，本类成员初始化列表 {...}
```C++
class B{
public:
    B();
    B(int i);
    ~B(){cout << "B's default destructor" << endl;}
    void print() const {cout << b << endl;}
private:
    int b;
};

B::B(){
    b = 0;
    cout << "B's default constructor" << endl;
}

B::B(int i){
    b = i;
    cout << "B's constructor" << endl;
}


class C: public B{
public:
    C();
    C(int i, int j);
    ~C(){cout << "C's default destructor" << endl;}
    void print() const;
private:
    int c;
};

C::C(){
    c = 0;
    cout << "C's default constructor" << endl;
}

C::C(int i, int j): B(i), c(j){
    cout << "C's constructor" << endl;
}

void C::print() const{
    B::print();
    cout << c << endl;
}

int main(){
    C obj(5, 6);
    obj.print();
    return 0;
}
// B's constructor
// C's constructor
// 5
// 6
// C's default destructor
// B's default destructor
```
多继承且有对象成员（组合类）时派生类的构造函数：
派生类名::派生类名（形参表): 
基类名1(参数表)，基类名2(参数表)......
本类成员（含对象成员）初始化列表 {
    //其他初始化
};

构造函数执行次序：
1. 基类构造函数，顺序按被继承声明时的顺序，从左到右（class C: public B, public A, public G......{）当构造函数中没有给某个基类传递参数时，调用其默认构造函数，注意，不是不调用了！
2. 初始化列表中的成员进行初始化
3. 函数体
## 派生类的复制构造函数
没声明时，会生成一个隐含的复制构造函数，会先调用基类复制构造函数，再为派生类新增成员执行复制

若声明了，则要给基类复制构造函数传参，但是复制构造函数只能接受一个参数，既用来 初始化派生类定义的成员，也被传递给基类复制构造函数。
`C::C(const C &c1): B(c1) {...}`
基类复制构造函数形参是基类对象的引用，而实参也可以是派生类对象的引用
## 派生类的析构函数
析构函数不被继承，派生类若需要，要自行声明，由于析构函数没有参数，不需要显示地调用基类析构函数
声明方式与没有继承时候一样
析构函数执行次序：
1. 派生类析构函数函数体，如果有组合类，按出现次序逆序调用（先构造的后析构）
2. 基类析构函数，顺序按被继承声明时的逆序，从右到左（class C: public B, public A, public G......{）
## 访问从基类继承的成员
### 当派生类与基类中有相同成员时
若无特别限定，通过派生类对象使用的是派生类中同名成员
若要访问基类中的同名成员，需使用基类名和作用域操作符（::）来限定 `derived.Base::fun()`，`p -> Base::fun()`
### 二义性问题
若从不同基类继承了同名成员，但在派生类中没有定义同名成员时存在二义性
需要用类名限定，也可以利用上面所提到的覆盖，你在派生类中声明一个同名成员，用基类成员实现
## 虚基类
当派生类从多个基类派生，而这些基类又有共同基类时，在访问这共同基类时会出现冗余并可能带来不一致性
虚基类声明：以 virtual 说明基类继承方式`class B: virtual public B`
在第一级继承时要将共同基类设计为虚基类
可以为最远的派生类提供唯一基类成员，而不重复产生多次复制
这样，在派生类的对象中，就可以直接访问虚基类的成员 `derived.base_fun()`

**注意区分虚基类与虚函数！完全不同！**
虚基类是消除标识符的二义性和信息冗余
虚函数是实现动态多态性的基础

### 虚基类的构造函数（慎用）
将建立对象时所指的的类称为最远派生类。虚基类的成员是由最远派生类的构造函数通过调用虚基类的构造函数进行初始化的。
在整个继承结构中，直接或间接继承虚基类的所有派生类都必须在构造函数的成员初始化表中为虚基类的构造函数列出参数，若未列出，则表示调用该虚基类的默认构造函数。
建立对象时，只有最远派生类的构造函数调用虚基类的构造函数，其他类对虚基类构造函数的调用被忽略。

简单来说，不管最远派生类是否继承了虚基类，在构造函数中都要给虚基类传递参数，而且其他的基类中如果调用了虚基类的构造函数，此时不从该途径执行。
## 多态性
多态通过绑定（将一个标识符与一段函数代码结合起来）实现
编译时绑定--早绑定（编译阶段编译器完成）（如函数重载）
运行时绑定--晚绑定，动态绑定
## 运算符重载
不能重载的运算符："."，".*"，"::"，"?:"
重载后运算符的优先性和结合性都不会改变
可以重载为类的非静态成员函数或非成员函数
### 双目运算符重载为成员函数
函数类型 operator 运算符（形参）
参数个数等于原操作个数-1（后置++、--除外），因为形参里放的是第二个操作数及以后的
所以前置单目运算符甚至是空参数表

若要重载符号 B 为类成员函数以达到 op1 B op2 表达式的作用，其中 op1 是A类对象，B是A的成员函数，形参类型是op2所属类型
op1 B op2 == op1.operator B(op2)
```C++
class Complex{
public:
    Complex(double r=0.0, double i=0.0): real(r), imag(i){}
    Complex operator +(const Complex &c2) const;
    Complex operator -(const Complex &c2) const;
    void display() const;
private:
    double real;
    double imag;
};

Complex Complex::operator +(const Complex &c2) const{
    return Complex(real + c2.real, imag + c2.imag);
}

Complex Complex::operator -(const Complex &c2) const{
    return Complex(real - c2.real, imag - c2.imag);
}

void Complex::display() const{
    cout << "(" << real << "," << imag << ")" << endl;
}
```
### 单目运算符重载为成员函数
#### 前置单目
若要重载符号 U 为类成员函数以达到 B op 表达式的作用，其中 op 是A类对象，U是A的成员函数，无形参
U op == op.operator U()
#### 后置单目（i++，i--）
为了区分名字相同的前置后置，只能在参数表上做手脚，这个参数只用来区分，无实际意义
若要重载符号 U 为类成员函数以达到 op U 表达式的作用，其中 op 是A类对象，U是A的成员函数，有一个 int 类型形参
op++ == op.operator ++(0)
```C++
class Clock{
public:
    Clock(int hour=0, int minute=0, int second=0){//略}
    Clock& operator ++();
    Clock operator ++(int);
    void display() const;
private:
    int hour, minute, second;
};

Clock &Clock::operator ++(){    //前置
    second++;
    //条件处理，略
    return *this;
}

Clock Clock::operator ++(int){    //后置通过调用前置实现 i++
    Clock old = *this;
    ++(*this);
    return old;        //返回的是副本，右值，无法改变本对象
}

void Clock::display() const{
    cout << hour << ":" << minute << ":" << second << endl;
}
```
### 运算符重载为非成员函数
左操作数不是该类的对象，或者类不是我们自定义，不能更改时使用
如 "<<" 左边是cout，无法改（cout << a << b == operator << (operator << (cout, a), b);）
此时函数的形参表示从左到右所以的操作数，参数个数=原操作数个数（除后置++，--），要求至少有一个**自定义类型的**参数
后置单目运算符++和--（i++）的形参表中要增加一个int，但不用写形参名
若运算符的重载函数需要操作类的私有成员，可以将该函数声明为类的友元

op1 B op2 == operator B(op1， op2)    //双目
B op == operator B(op)                          //前置单目 ++i
op B == operator B(op，0)                    //后置单目 i++

```C++
class Complex{
public:
    Complex(double r=0.0, double i=0.0): real(r), imag(i){}
    friend Complex operator +(const Complex &c1, const Complex &c2);
    friend Complex operator -(const Complex &c1, const Complex &c2);
    friend ostream &operator <<(ostream &out, const Complex &c);
private:
    double real;
    double imag;
};


Complex operator +(const Complex &c1, const Complex &c2){
    return Complex(c1.real + c2.real, c1.imag + c2.imag);
}

Complex operator -(const Complex &c1, const Complex &c2){
    return Complex(c1.real - c2.real, c1.imag - c2.imag);
}

ostream &operator <<(ostream &out, const Complex &c){
    out << "(" << c.real << "," << c.imag << ")";
    return out;
}


Complex c1(5, 4), c2(2, 10), c3;
cout << c1 << endl;
c3 = c2 - c1;
cout << c3 << endl;
```
## 虚函数
实现动态绑定的函数，是实现运行时多态的基础

虚函数是通过基类的指针和引用访问派生类对象的成员
（原本非虚函数：派生类的对象可以初始化基类的引用，派生类的地址可以赋值给基类的指针）

虚函数必须是非静态的成员函数，虚函数经过派生后，就可以实现运行过程中的多态
如1357行的函数：
```C++
void fun(Base *ptr){
    ptr -> display();
}
```
由于函数声明的是Base类型的对象，编译后也就静态编译成了Base类，所以即使继承的Derived类自己有相应的 display 函数，在调用 fun 的时候也只会调用基类Base的display函数
解决：
通过虚函数实现运行时多态，加virtual关键字
由于内联函数是在编译时直接把代码嵌入，所以虚函数不能是内联函数
```C++
class Base{
public:
    virtual void display() const;    //加virtual关键字，告诉编译器，在编译时不要做静态绑定，而且虚函数不能写成内联
};

void Base::display() const{
        cout << "Base::display()" << endl;
}


class Derived: public Base{
public:
    virtual void display() const;
};

void Derived::display() const{
        cout << "Derived::display()" << endl;
}


void fun(Base *ptr){
    ptr -> display();
}

Base base;
Derived derived;
fun(&base);            //输出"Base::display()"
fun(&derived);        //输出"Derived::display()"
```
在派生类中可以对基类中成员函数进行覆盖，而且还会隐藏基类中同名函数的所有其他重载形式（这时需要用类名::的方式访问）
派生类的virtual可以不写（一般都会写），只要与基类中虚函数名称，返回值，参数表相同则视为虚函数
### 什么函数可以是虚函数
一般成员函数，析构函数可以是虚函数
**构造函数不可以是虚函数**
### 虚析构函数
如果打算允许人通过基类指针调用派生类的析构函数时，就需要让基类析构函数为虚函数
因为前面的虚函数都是类中同名所以带来的问题，而析构函数都可以用delete关键字来调用，所以要用函数参数是基类指针时，析构函数是虚函数
## 虚表和动态绑定
每个多态类有一个虚表
虚表中有当前类的各个虚函数的入口地址
每个对象有一个纸箱当前类的虚表的指针

动态绑定的实现：
构造函数中为对象的虚指针赋值
通过多态类型的指针或引用调用成员函数时，通过虚指针找到虚表，进而找到所调用的虚函数的入口地址，通过该入口地址调用虚函数
![虚表](_v_images/20200219233622476_26678.png =800x)
## 抽象类
纯虚函数，在基类中声明的虚函数，在基类中没有定义具体操作内容，要求各派生类根据实际情况定义自己的版本
virtual 函数类型 函数名 （参数表）= 0;
有纯虚函数的类即为抽象类，由于参数没定，抽象类不能实例化，只能当做基类

作用：
将有关数据和行为组织在一个继承层次结构中，保证派生类具有要求的行为
对于暂时无法实现的函数，可以声明为纯虚函数，留给派生类去实现
```C++
class Base1{
public:
    virtual void display() const = 0;    //纯虚函数
};


class Base2: public Base1{
public:
    virtual void display() const;        //覆盖基类的纯虚函数
};

void Base2::display() const{
        cout << "Base2::display()" << endl;
}


class Derived: public Base2{
public:
    virtual void display() const;
};

void Derived::display() const{
        cout << "Derived::display()" << endl;
}


void fun(Base1 *ptr){        //这里还可已定义形参类型是base1的指针，但是不能定义base1的对象了
    ptr -> display();
}

Base2 base2;
Derived derived;
fun(&base2);            //输出"Base2::display()"
fun(&derived);        //输出"Derived::display()"
```
## override & final (C++11)
多态行为的基础：基类声明虚函数，派生类声明一个函数覆盖该虚函数
覆盖要求：函数签名完全一致
函数签名包括：函数名 参数列表 const
C++11引入显式函数覆盖，在编译器而非运行期捕获这种覆盖不成功带来的运行错误
override 关键字，这样编译器就会去寻找是否符合虚函数，不符合就直接报编译错误，而不是在运行时隐性出错不好找

若不希望某个类被继承，或者类中某个函数不希望被覆盖，可以加 final关键字
```C++
struct Base1 final {};

struct Base2{
    virtual void f() final;
};

struct Derived: Base2{
    void f();            //报编译错误
};
```
## 函数模板
如前文的重载加，减，绝对值等函数
定义语法：  
template <模板参数表>
函数定义

模板参数表的内容：
类型参数：class（或typename）标识符
常量参数：类型说明符 标识符
模板参数：template <参数表> class 标识符
```C++
template<typename T>
T abs(T x){
    return x < 0? -x: x;
}

template <class T>
void outputArray(const T *array, int count){    //如果输入的array是对象数组，还需要重载 << 运算符
    for (int i = 0; i < count; i++)
        cout << array[i] << " ";
    cout << endl;
}
```
一个函数模板并非自动可处理所有类型的数据
只有能够进行函数模板中运算的类型，才可以作为形参类型，不然需要重载相关运算符
## 类模板
定义语法：  
template <模板参数表>
class 类名{};
如果需要在类模板以外定义其成员函数，则需在签名加 template <模板参数表>
```C++
struct Student{
    int id;
    float gpa;
};

template <class T>
class Store{
private:
    T item;                       //存放任意类型数据
    bool haveValue;              //标记item是否存入内容
public:
    Store();
    T &getElem();                //提取数据
    void putElem(const T &x);    //存入数据
}

template <class T>
Store<T>::Store(): haveValue(false){}

template <class T>
T &Store<T>::getElem(){
    if (!haveValue){
        cout << "No item" << endl;
        exit(1);        //使程序完全退出
    }
    return item;
}

template <class T>
void Store<T>::putElem(const T &x){
    haveValue = true;
    item = x;
}


int main(){
    Store<int> s1;
    Store<double> s2;
    
    Student g = {1000, 23};
    Store<Student> s3;        //模板可以是类，但是这时候要注意一些自定义对象非法的操作，如+，-。。。。
    return 0;
}
```
## 线性群体
线性群体元素按位置排列有序
可以 直接访问（数组）、顺序（链表）访问和索引访问
## 数组类
动态数组由一系列位置连续的，任意数量，相同类型的元素组成
这一节是实现一个相对vector更简易的动态数组类模板
### 为什么有的函数返回引用
引用时，函数返回值作为引用是左值，可以在后面改变对象的值（比如重载[]时）
对象转化为指针运算符：重载*
## 链表与结点类模板
```C++
template <class T>
class Node{
private:
    Node<T> *next;
public:
    T data;
    Node(const T& data, Node<T> *next = 0);    //构造函数
    void insertAfter(Node<T> *p);
    Node<T> *deleteAfter();
    const Node<T> *nextNode() const;
};

template <class T>
Node<T>::Node(const T& data, Node<T> *next = 0):data(data), next(next) {}

template <class T>
void Node<T>::insertAfter(Node<T> *p){
    p -> next = next;
    next = p;
}

template <class T>
Node<T> *Node<T>::deleteAfter(){
    Node<T> *tempPtr = next;
    if (next == 0)
        return 0;
    next = tempPtr -> next;
    return tempPtr;        //tempPtr交给外层函数决定何时释放
}

template <class T>
const Node<T> *Node<T>::nextNode() const{
    return next;
}


template <class T>

class LinkedList{
private:
    Node<T> *front, *rear;
    Node<T> *prePtr, *curPtr;    //记录位置的指针，在插入、删除操作中更新
    int size;
    int position;    //当前元素在表中的位置序号，由函数reset使用
    Node<T> *newNode(const T &item, Node<T> *ptrNext = nullptr);    //产生新结点
    void freeNode(Node<T> *p);        //释放结点
    void copy(const LinkedList<T> &L);    //复制构造函数子模块，在调用复制构造函数和 = 操作时调用
public:
    LinkedList();
    LinkedList(const LinkedList<T> &L);    //复制构造函数
    ~LinkedList();
    LinkedList<T> & operator =(const LinkedList<T> &L);
    
    int getSize() const;
    bool isEmpty() const;
    
    void insertFront(const T &item);    //表头插入
    void insertRear(const T &item);     //表尾
    void insertAt(const T &item);       //当前结点前插入
    void insertAfter(const T &item);       //当前结点后插入

    T deleteFront();            //删除头结点
    void deleteCurrent();       //删除当前结点
    T& data();                        //返回对当前结点成员数据的引用
    const T& void T&data() const;     //返回对当前结点成员数据的常引用

    void clear();    //清空链表，释放所有结点。被析构函数，operator= 调用
};
```
## 栈类模板
后进先出
```C++
template <class T, int SIZE = 50>
class Stack{
private:
    T list[SIZE];
    int top;
public:
    Stack()
    void push(const T &item);
    T pop();
    void clear();
    const T &peek() const;
    bool isEmpty() const;
    bool isFull() const;
};
```
## 队列模板
先进先出
假溢出（队尾=队头!=首地址）：用循环队列处理 rear = (rear + 1) % SIZE;
## 排序
主关键字：数据元素中某数据项的值，主关键字不能重复
### 选择排序
每次找最小
```C++
template <class T>
void mySwap(T &x, T &y){
    T temp = x;
    x = y;
    y = temp;
}

template <class T>
void selectionSort(T a[], int n){
    for (int i = 0; i < n - 1; i++){
        int leastIndex = i;
        for  (int j = i + 1; j < n - 1; j++)
            if (a[j] < a[leastIndex])
                leastIndex = j;
        mySwap(a[i], a[leastIndex]);
    }
}
```
### 交换排序
冒泡：相邻俩数两两比较
```C++
template <class T>
void mySwap(T &x, T &y){
    T temp = x;
    x = y;
    y = temp;
}

template <class T>
void bubbleSort(T a[], int n){
    int i = n - 1;
    while (i > 0){
        int lastExchangeIndex = 0;    //对基本有序的处理
        for (int j = 0; j < i; j++)
            if (a[j+1] < a[j]){
                mySwap(a[j], a[j+1]);
                lastExchangeIndex = j;
            }
            i = lastExchangeIndex;
    }
}
```
## 查找
以折半查找为例：
```C++
template <class T>
int binSearch(const T list[], int n, const T &key){
    int low = 0;
    int high = n - 1;
    while (low <= high){
        int mid = (low + high) / 2;
        if (key == list[mid])
            return mid;
        else if (key < list[mid])
            high = mid - 1;
        else
            low = mid + 1;
    }
    return -1;
}
```
## 泛型程序设计
编写不依赖于具体数据类型的程序，将算法抽象出来，典型范例：STL
概念，子概念Comparable，Assignable，Sortable
模型：符合某概念的数据类型：int是Comparable概念的模型，静态数组不是Assignable概念的模型

许多STL实现代码是使用概念命名模板参数的

## STL(Standard Template Library)
使用STL模板时，类型参数只要满足概念要求就可以使用，不管是否是自定义类型
包括：
输入输出类
容器与抽象数据类型
存储管理类
算法类
错误处理
运行环境支持
### 基本组件
#### 容器
容纳一组元素的对象
基本容器模板
1. 顺序容器：array，vector，deque(双端队列)，forward_list(单向链表)，list(列表)
2. （有序）关联容器：set，multiset(多重集合)，map，multimap(多重映射)
3. 无序关联容器：unordered_set，unordered_multiset，unordered_map，unordered_multimap
容器适配器：stack，queue，priority_queue(优先队列)
#### 迭代器
提供了顺序访问容器中每个元素的方法
用 * 访问迭代器所指元素；若元素是结构体，还可以使用 -> 直接访问元素成员
++ 访问下一个迭代器，有的还支持--
使用独立于STL容器的迭代器需要包含头文件<iterator>
#### 函数对象
一个行为类似函数的对象，可以像调用函数一样调用，是泛化的函数，任何普通函数和任何重载了 () 运算符的类的对象都可以作为函数对象使用
使用STL函数对象需要包含头文件<functional>
#### 算法
`#include <algorithm>`
STL将迭代器作为算法的参数，通过迭代器来访问容器，而不是将容器直接作为算法的参数
```C++
//输入十个数取相反数
const int N = 5;
vector<int> s(N);
for (int i = 0; i < N; i++)
    cin >> s[i];
transform(s.begin(), s.end(), ostream_iterator<int>(cout, " "), negate<int>());
//transform算法，前三个参数是迭代器，第三个参数是计算结果按输出流迭代器指示的cout输出，negate表示求相反数
cout << endl;
```

`template <class InputIterator, class OutputIterator, class UnaryFunction>
OutputIterator transform(InputIterator first, InputIterator last, OutputIterator result, UnaryFunction op);`
## 迭代器
迭代器是算法和容器的桥梁
算法不能直接操作容器中数据，而是通过迭代器间接操作
### 输入流迭代器
istream_iterator<T>
以输入流（如cin）为参数构造，
可用 *(p++)获得下一个输入的元素
### 输出流迭代器
ostream_iterator<T>
以输出流（如cout）为参数构造，
可用 *(p++) = x 将x输出到输出流

输入与输出流迭代器都属于适配器
适配器是用来为已有对象提供新接口的对象
输入与输出流迭代器为流对象提供了迭代器接口
```C++
#include <iterator>
#include <iostream>
#include <algorithm>

double square(double x){
    return x*x;
}

int main(){
    transfrom(istream_iterator<double>(cin), istream_iterator<double>(), ostream_iterator<double>(cout, "\t"), square);
    //第二个参数istream_iterator<double>()用默认方式构造,表示输入流的结束位置
    cout << endl;
    return 0;
}
```
### 前向迭代器
forward iterator
既是输入流迭代器也是输出迭代器，并且可以对序列进行单向的遍历
### 双向迭代器
bidirectional iterator
既是输入流迭代器也是输出迭代器，并且可以对序列进行双向的遍历
### 随机访问迭代器
random access iterator
也是双向迭代器，但能在序列中的任意两个位置之间进行跳转
如指针，使用vector的begin()，end()函数得到的迭代器
### 迭代器的区间
两个迭代器表示一个区间：[p1, p2)
```C++
template <class T, class InputIterator, class OutputIterator>
void mySort(InputIterator first, InputIterator last, OutputIterator result){
    vector<T> s;
    for (; first != last; ++first)
        s.push_back(*first);
    sort(s.begin(), s.end());        //sort函数的参数必须是随机访问迭代器
    copy(s.begin(), s.end(), result);
}

int main(){
    double a[5] = {1.2....};
    mySort<double>(a, a+5, ostream_iterator<double>(cout, " "));
    cout << endl;

    mySort<int>(istream_iterator<int>(cin), ostream_iterator<int>(), ostream_iterator<int>(cout, " "));
    cout << endl;
    return 0;
}
```

### 迭代器的辅助函数
#### advance(p, n)
将 p 执行 n 次自增操作
#### distance(first, last)
计算 first++多少次后 first == last
## 容器
1. 顺序容器：array，vector，deque(双端队列)，forward_list(单向链表)，list(列表)
2. （有序）关联容器：set，multiset(多重集合)，map，multimap(多重映射)
3. 无序关联容器：unordered_set，unordered_multiset，unordered_map，unordered_multimap
容器适配器：stack，queue，priority_queue(优先队列)

容器还可以以另一种分类方式分为
1. 可逆容器：list，set，map......
2. 随机访问容器：vector，deque

容器支持关系运算符
可用begin()，end() 获得容器首尾迭代器
clear()清空，empty()判断是否为空
size()
s1.swap(s2) 将s1和s2两容器内容交换

相关数据类型（S表示容器类型）：
S::iterator：指向容器元素的迭代器类型
S::const_iterator：常迭代器类型
### 对可逆容器的访问
你想迭代器可通过下面的成员函数得到
rbegin()：指向容器尾的逆向迭代器
rend()：指向容器首的逆向迭代器
逆向迭代器类型名表示方式：
S::reverse_iterator
S::const_reverse_iterator
### 对随机访问容器的访问
s[n]
### 顺序容器
array，vector，deque(双端队列)，forward_list(单向链表)，list(列表)
元素线性排列，可以随时在指定位置插入元素和删除元素
必须符合Assignable概念，即具有公有的复制构造函数并可用 = 赋值
特别地：array大小固定，forward_list是单向链表

接口（除array和forward_list）：
赋值函数：assign
插入函数：insert，push_front(只对list，deque)，push_back，emplace，emplace_front
删除函数：erase，clear，pop_front(只对list，deque)，pop_back，emplace_back
首尾元素直接访问：front，back
改变大小resize
```C++
template printContainer(const char* msg, const T& s){
    cout << msg << ":";
    copy(s.begin(), s.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
}

int main(){
    deque<int> s;
    for (int i = 0; i < 10; i++){
        int x;
        cin >> x;
        s.push_front(x);
    }
    printContainer("deque at first", s);
    
    list<int> l(s.rbegin(), s.rend());
    printContainer("list at first", l);
    //将列表中相邻两元素交换次序
    list<int>::iterator iter = l.begin();
    while (iter != l.end()){
        int v = *iter;
        iter = l.erase(iter);
        l.insert(++iter, v);
    }
    printContainer("list at last", l);
    return 0;
}
```
顺序容器主要包括：
#### 向量vector
可扩展的动态数组
随机访问，在尾部插入或删除元素快，在中间或头部插入或删除元素慢
vector是STL容器中唯一可以与C兼容的；`&v[0]`就可以得到v中第一个元素的指针
s.capacity() 返回当前容量
s.reserve(n) 若容量小于n，则扩展到n
#### 双端队列deque
在两端插入或删除快，在中间慢，随机访问较快，但比向量慢
```C++
//先从大到小输出奇数，再从小到大输出偶数
int main(){
    istream_iterator<int> i1(cin), i2;    //i2默认初始化相当于尾
    vector<int> s1(i1, i2);               //通过输入流输入数据
    sort(s1.begin(), s1.end());
    
    deque<int> s2;
    for (vector<int>::iterator iter = s1.begin(); iter != s1.end(); ++iter){
        if (*iter % 2 == 0)
            s2.push_back(*iter);
        else
            s2.push_front(*iter);
    }
    copy(s2.begin(), s2.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
    return 0;
}
```
#### 列表list
在任意位置插入和删除都很快。但是不支持随机访问
特有接合splice操作:s1.splice(p, s2, q1, q2); 将 s2 中 [q1, q2)移动到 s1 中 p 所指向元素之前
#### 单向链表forward_list
没有简单的方法获取一个节点的前驱
只有insert_after, emplace_after, erase_after操作，参数与之前相同，但是操作的是当前之后的结点
不支持size操作
#### 数组array
对内置数组的封装
array对象的大小是固定的，定义时除了需要指定元素类型，还要指定容器大小
不能动态改变容器大小
### 顺序容器的插入迭代器和适配器
用于在容器头部，尾部或中间指定位置插入元素
前插 front_inserter，后插 back_inserter，任意位置插入 inserter
```C++
list<int> s;
back_inserter iter(s);
*(iter++) = 5;    //通过iter将5插入末尾
```
适配器：栈，队列和优先队列（priority_queue，最"大"的元素最先被弹出）
栈模板：`template <class T, class Squence = deque<T>> class stack{};`
队列模板：`template <class T, class FrontInsertionSequence = deque<T>> class queue{};`
栈允许任何一种顺序(sequence)容器作为基础容器，
队列只允许用前插顺序容器（双端队列或列表）

栈和队列共同支持的操作：
关系比较符（字典序比较）
s.size(), s.empty(), s.push(), s.pop()
栈和队列都不支持迭代器，因为它们不允许对任意元素进行访问
特别地：
栈：s.top() 获得栈顶元素的引用
队列：s.front()获得队头元素的引用，s.back()获得队尾元素的引用

优先级队列：
也支持元素的压入和弹出，但弹出的顺序与元素大小有关，每次弹出的总是容器中最大的元素
`template <class T, class Sequence = vector<T>> class priority_queue{};`
优先队列的基础容器必须是支持随机访问的顺序容器
支持 s.size(), s.empty(), s.push(), s.pop(), s.top()
优先队列本身并不支持比较操作
### 关联容器
特点：
每个关联容器都有一个键（可key），可以根据键高效查找元素
用红黑树实现，查找插入删除时间复杂度都是`O(logN)`，都是按键值排好序的。
接口：insert，erase，find，counter，lower_bound，upper_bound，equal_range

单重关联容器（set，map）：键值唯一，一个键值对应一个元素
多重关联容器（multiset，multimap）：键值不唯一，一个键值可对应多个元素

简单关联容器（set，multiset）：容器只有一个类型参数 `set<int>`表示键类型，容器的元素就是键本身
二元关联容器（map，multimap）：容器有两个类型参数 `map<string, int>`分别表示键，附加数据类型，容器的元素是pair<K, V>
#### 无序关联容器（C++11)
unordered_set，unordered_multiset，unordered_map，unordered_multimap
用哈希表实现，查找插入删除时间复杂度都是`O(1)`，最坏情况下`O(N)`
不使用比较运算符来组织元素，而是通过哈希函数和键类型的==运算符
提供了与有序关联容器相同的操作
#### 集合set
集合的元素有序，且存储无重复元素，可以高效查找指定元素，也可以方便得到指定大小范围的元素在容器中所处区间
```C++
set<double> s;
while (true){
    double v;
    cin >> v;
    if (v == 0) break;
    pair<set<double>::iterator, bool> r = s.insert(v);
    if (!r.second)
        cout << v << " is duplicated" << endl;
}

set<double>::iterator iter1 = s.begin();
set<double>::iterator iter2 = s.end();
double medium = (*iter1 + *(--iter2)) / 2;
cout << "<= medium: ";
copy(iter1, s.upper_bond(medium), ostream_iterator<double>(cout, " "));
cout << endl;
cout << ">= medium: ";
copy(s.lower_bond(medium), iter2, ostream_iterator<double>(cout, " "));
cout << endl;
```
#### 映射map
```C++
//选课
map<string, int> courses;
courses.insert(make_pair("C++", 3));
courses.insert(make_pair("data structure", 2));
courses.insert(make_pair("matrix", 4));
int n = 2;        //剩下可选次数
int sum = 0;      //学分总和

while (n > 0){
    string name;
    cin >> name;
    map<string, int>::iterator iter = courses.find(name);
    if (iter == courses.end()){        //没找到
        cout << name << " is not available" << endl;
    }
    else{
        sum += iter -> second;
        courses.erase(iter);
        n--;
    }
}
cout << "Total credit: " << sum << endl;
```
```C++
//统计一句话中每个字母出现的次数
map<string, int> s;
char c;
do{
    cin >> c;
    if (isalpha(c)){
        c = c.tolower(c);
        s[c]++;
    }
}while (c != '.')

for (map<string, int>::iterator iter = s.begin(); iter != s.end(); ++iter)
    cout << iter -> first << " " << iter -> second << " ";
cout << endl;
```
#### 多重集合与多重映射
多重集合：允许有重复元素
多重映射：允许一个键对应多个附加数据
```C++
//上课时间查询
multimap<string, string> courses;
typedef multimap<string, string>::iterator CourseIter;

courses.insert(make_pair("C++", "2-6"));
courses.insert(make_pair("data structure", "3-1"));
courses.insert(make_pair("data structure", "5-2"));
courses.insert(make_pair("matrix", "1-2"));
courses.insert(make_pair("matrix", "4-1"));
courses.insert(make_pair("matrix", "5-5"));
string name;
int count;

do{
    cin >> name;
    count = courses.count(name);
    if (count == 0)
        cout << "Cannot find this course" << endl;
}while (count == 0)

cout << count << " lesson(s) per week" << endl;
//输出多重映射的各个元素
pair<CourseIter, CourseIter> range = courses.equal_range(name);
for (CourseIter iter = range.first; iter != range.second; ++iter)
    cout << iter -> second << " ";
cout << endl;
```
## 函数对象
行为类似函数的对象，可以没有参数，也可以带有若干参数，功能是获取一个值或者改变操作的状态
普通函数和重载了 () 运算符的类的实例都可以作为函数对象
在（）的重载函数中实现想要的功能
函数对象包括产生器Generator（没有参数），一元函数对象Unary Function（一个参数），二元函数对象Binary Function（两个参数）
其中，一元函数对象还包括一元谓词Unary Predicate（返回bool类型）；二元函数对象还包括二元谓词Binary Predicate
连乘实现：`accumulate(a, a + sizeof(a)/sizeof(int), 1, multiplies<int>());`
利用greater实现由大到小排序：`sort(a.begin(), a.end(), greater<int>())`
## 函数适配器
包括以下几种：
### 绑定适配器
bind1st，bind2nd 将n元函数对象的指定参数绑定为一个常数得到n-1元函数对象

比如find_if函数，其原型如下，最后一个参数要求UnaryPredicate一元的
```C++
template<class InputIterator, class UnaryPredicate>
InputIterator find_if(InputIterator first, InputIterator last, UnaryPredicate pred);
```
利用绑定适配器输入一元参数：
```C++
//找到数组中第一个大于40的元素
vector<int>::iterator p = find_if(a.begin(), a.end(), bind2nd(greater<int>, 40));
```
### 组合适配器
not1，not2 将指定谓词的结果取反
### 函数指针适配器
ptr_fun 将一般函数指针转换为函数对象，使之能作为其他函数适配器的输入
在进行参数绑定或其他转化的时候，通常需要函数对象的类型信息，如bind1st，bind2nd要求函数对象必须继承于binary_function类型。但如果传入的是函数指针形式的函数对象则无法获得函数对象的类型信息
```C++
//找到数组中第一个大于40的元素
bool g(int x, int y){
    return x > y;
}
//ptr_fun将函数指针转化为可输入的对象
vector<int>::iterator p = find_if(a.begin(), a.end(), bind2nd(ptr_fun(g), 40));
//找到数组中第一个不大于40的元素
vector<int>::iterator p = find_if(a.begin(), a.end(), not1(bind2nd(ptr_fun(g), 40)));    //not1表示一元谓词
vector<int>::iterator p = find_if(a.begin(), a.end(), bind2nd(not2(ptr_fun(g), 40)));    //not2表示二元谓词
```
### 成员函数适配器
ptr_fun，ptr_fun_ref 将n元成员函数适配为n+1元函数对象，该函数对象的第一个参数是调用该成员函数时的目的对象
即 object -> method(arg) 转化为 method(object, arg)
```C++
struct Car{
    int id;
    Car(int id){this -> id = id;}
    void display() const{cout << id << endl;}
}

vector<Car*> pcars;
vector<Car> cars;
//输出向量中所有成员的信息
for_each(pcars.begin(), pcars.end(), std::mem_fun(&Car::dsiplay));        //mem_fun参数是对象的指针
for_each(cars.begin(), cars.end(), std::mem_fun_ref(&Car::dsiplay));      //mem_fun_ref参数是对象的引用
```
## 算法
STL算法本身是一种函数模板，通过迭代器获得输入，通过函数对象对数据处理，通过迭代器输出
STL算法通用，独立于具体数据类型，容器类型

### 不可变序列算法
不直接修改所操作的容器内容（查找指定元素，比较两序列是否相等，对元素计数等）
```
//查找[first, last)区间内pred(x)为真的首个元素
template<class InputIterator, class UnaryPredicate>
InputIterator find_if(InputIterator first, InputIterator last, UnaryPredicate pred);
```
### 可变序列算法
可以修改所操作的容器内容（对序列进行复制，删除，替换，倒序，旋转，交换，分割，去重，填充，洗牌以及生成一个序列）
```
//将[first, last)所有元素改写为x
template<class ForwardIterator, class UnaryPredicate>
InputIterator find_if(ForwardIterator first, ForwardIterator last, const T& x);
```
### 排序与搜索算法
对序列排序，两有序序列合并，有序序列搜索，有序序列的集合操作，堆算法
```
//以函数对象comp为 "<" 对[first, last)排序
template<class RandomAccessIterator, class UnaryPredicate>
void sort(RandomAccessIterator first, RandomAccessIterator last, UnaryPredicate comp);
```
### 数值算法
求序列元素的和，部分和，相邻元素的差或两序列的内积
```
//以函数对象op为 "+" 对[first, last)求部分和，返回的迭代器输出序列最后一个元素的下一个元素
template<class InputIterator, class OutputIterator, class BinaryFunction>
OutputputIterator partial_sum(InputIterator first, InputIterator last, OutputIterator result, BinaryFunction op);
```
## IO流的概念及流类库结构
标准输入输出文件比如键盘和显示屏，在C++中都是看做文件的
当程序与外界环境进行信息交换时，存在两个对象，一个是程序中的对象，另一个是文件对象（外部设备或硬盘文件）
流是信息流动的现象
流对象与文件操作：
程序建立一个流对象
指定这个流对象与某个文件对象建立连接
程序操作流对象
流对象通过文件系统对所连接的文件对象产生作用

读操作-从流中提取
写操作-从流中插入
## 输出流
最重要的三个输出流：
ostream，ofstream，ostringstream，
预先定义的输出流对象：
cout（标准输出），cerr（标准错误输出，发送给他的立即输出），clog（类似cerr，但是有缓冲，缓冲区满时输出）
```C++
ofstream fout("b.out");    //输出到b.out文件
streambuf* pOld = cout.rdbuf(fout.rdbuf());    //将cout标准输出转移到b.out中
cout.rdbuf(pOld);            //恢复标准输出
```
构造输出流对象
ofstream类支持磁盘文件输出：
```C++
ofstream myFile("b.out");        //直接调用构造函数，构造时直接打开该文件
//等价于：
ofstream myFile;
myFile.open("b.out");
//可以定义打开模式：打开一个用于输出的二进制文件，当有多个关键词限制时，用|限制
ofstream myFile("filename", ios_base::out | ios_base::binary);    //ios_base::out是默认输出参数，可省略
```
### 文件输出流成员函数
open，close
put：写入一个字符
write：将内存中的一块内容（二进制）写到一个文件输出流中
seekp，tellp：操作文件流的内部指针（seekp移动流内部的写指针，tellp返回当前写指针位置）
错误处理函数
### 向文本文件输出
插入运算符：<<
操纵符(manipulator)：与插入运算符一起控制输出格式
大多数定义在ios_base类中，需要#include <iomanip>
控制宽度：setw或调用成员函数width。特殊地：setw和width仅影响随后的一项输出项，其他操纵符则是一直有效，直到发生改变
```C++
double values1[4], values2[4];
for (int i = 0; i < 4; i++){
    cout.width(10);
    cout << values1[i] << endl;
    //cout << setw(10) << values[i] << endl;    //等价
    cout << setiosflags(ios_base::left)     //设置左对齐，ios_base::left是ios_base类的静态常量
         << setw(6) << values1[i]
         << resetiosflags(ios_base::left)     //取消左对齐，回到默认右对齐方式
         << setw(10) << values2[i]
         << endl;
}
```
setiosflags可以操作输出格式，参数可以用 | 进行组合
#### 控制输出精度
浮点数默认值是6：如3466.55
要改变精度，用setprecision
如果不指定fixed（定点）或scientific（科学计数法），精度值表示有效数字位，否则，那俩的精度值表示小数点之后的位数
```C++
double values1[4], values2[4];
cout << setiosflags(ios_base::fixed);        //设置定点数输出
for (int i = 0; i < 4; i++){
    cout << setiosflags(ios_base::left)     //设置左对齐，ios_base::left是ios_base类的静态常量
         << setw(6) << values1[i]
         << resetiosflags(ios_base::left)     //取消左对齐，回到默认右对齐方式
         << setw(10) << setprecision(1) << values2[i]    //小数点后一位小数
         << endl;
}
```
### 向二进制文件输出
两种方法：
使用ofstream构造函数中的模式参量指定二进制输出模式
以通常方式构造一个流，然后用 setmode 成员函数，在文件打开后改变模式
write函数第一个参数是指向字符类型的指针，第二个参数是要写的字节数
```C++
struct Date{            //将一个较大的数据结构的数据对象直接写到磁盘文件中，存储效率更高
    int mon, day, year;
};

Date dt = {6, 2, 1995};
ofstream file("date.dat", ios_base::binary);
file.write(reinterpret_cast<char*>(&dt), sizeof(dt));        //reinterpret_cast临时类型转化为字符类型指针
file.close();
```
### 向字符串输出
头文件是`#include <sstream>`
相当于转化成字符串，因为比如标准输出cout输出到显示设备上也是输出的文本，其实就相当于转化成了字符串
支持ofstream类中除了open，close以外的所有操作
str函数可以返回当前已构造的字符串
```C++
#include <sstream>
template <class> T
inline string toString(const T &v){
    ostringstream os;    //创建字符串输出流
    os << v;            //插入，能被插入运算的都可以直接传化，但如果是自定义的对象，就需要重载 << 操作符了
    return os.str();    //转化为字符串
}

toString(1.2);
```
## 输入流
最重要的三个输入流：
istream，ifstream，istringstream
```C++
ifstream myFile("filename");        //直接调用构造函数，构造时直接打开该文件
//等价于：
ifstream myFile;
myFile.open("filename");
//可以定义打开模式：打开一个用于输入的二进制文件，当有多个关键词限制时，用|限制
ifstream myFile("filename", ios_base::in | ios_base::binary);    //ios_base::out是默认输入参数，可省略
```
### 文件输出流成员函数
open，close
get：与>>相似，但是读入时包括空白字符
getline：整行读入，可指定终止符，读取完成后从读取内容中删除终止符
read：从文件读字节到一个指定内存区域，长度有参数定
seekg，tellg：操作文件流的内部指针（seekg设置流内部的读指针，tellg返回当前读指针位置）
### 向文本文件输出
提取运算符 >>
输入时一般少用格式控制，最多用dec，oct，hex
```C++
char ch;
while (ch = cin.get() != EOF)    //EOF文件结束标
    cout.put(ch);
    
string line;
getline(cin, line, '\n');
```
二进制方式写与读：
```C++
struct Date{            //将一个较大的数据结构的数据对象直接写到磁盘文件中，存储效率更高
    int mon, day, year;
};
//写
Date dt = {6, 2, 1995};
ofstream file("date.dat", ios_base::binary);
file.write(reinterpret_cast<char*>(&dt), sizeof(dt));        //reinterpret_cast临时类型转化为字符类型指针
file.close();
//读
ifstream is("date.dat", ios_base::binary);
if (is){
    Date dd;
    is.read(reinterpret_cast<char*>(&dd), sizeOf(dd));
}
is.close();
```
seekg和tellg
```C++
int values[] = {3, 7, 5, 0, 4};
ofstream os("intergers", ios_base::out | ios_base::binary);
os.write(reinterpret_cast<char*>(values), sizeof(values));
os.close();

//用seekg设置位置指针
ifstream is("intergers", ios_base::in | ios_base::binary);
if (is){
    is.seekg(3 * sizeof(int));    //向后移动3个int，就是第4个数
    int v;
    is.read(reinterpret_cast<char*>(&v), sizeof(int));
    cout << "The 4th interger is " << v << endl;
}
is.close();

//用tellkg返回元素位置
ifstream is("intergers", ios_base::in | ios_base::binary);
if (is){
    while (is){        //读到文件尾is为0
        streampos here = is.tellg();
        int v;
        is.read(reinterpret_cast<char*>(&v), sizeof(int));    //每次读取一个整数
        if (is && v == 0)    //文件没有结束，且v==0
            cout << "Position " << here << " is 0" << endl;
    }
}
is.close();
```
### 从字符串输入
istringstream在构造函数中设置要读取的字符串
支持ifstream类中除了open，close以外的所有操作
典型应用：将字符串转化为数值
```C++
#include <sstream>
template <class> T
inline T fromString(const string &str){
    istringstream os;    //创建字符串输入流
    T v;
    is << v;
    return v;
}

fromString<int>("1.2");
```
### 输入输出流
iostream 既可以是数据的源也可以是数据的目的，有两个逻辑子流分别用于输入和输出
从iostream派生出 fstream（访问文件）stringstream（访问字符串）
## 异常处理
由某个函数产生异常可以立即处理，也可以向上一级抛出
![Screenshot_2020-02-23-23-51-15-842_tv](_v_images/20200224000651132_8606.jpg =540x)
若有异常则通过throw创建一个异常对象并抛掷
将可能抛出异常的程序段嵌在try块中，通过正常顺序执行到try语句，然后执行到try内保护段
若没有引起异常，则catch语句不执行，程序从try块后最后一个catch子句后面的语句继续执行
若没有匹配的catch子句，则自动调用库函数terminate，其默认功能是调用abort终止程序
```C++
//抛出异常程序段
throw 表达式;
//捕获并处理异常
try
    复合语句 保护段(有throw)
catch (异常声明)
    复合语句
catch (异常声明)
    复合语句
```
```C++
int divide(int x, int y){
    if (y == 0)
        throw x;
    return x / y;
}

try{
    cout << "5 / 2 = " << divide(5, 2) << endl;
    cout << "4 / 0 = " << divide(4, 0) << endl;
}catch(int e){        //catch类型要和throw类型约定一致
    cout << e << " is divided by zero" << endl;
}
```
### 异常接口声明
提前显式声明可能抛出的异常，有利于函数调用者为异常处理做好准备
`void fun() throw(A, B, C, D);`其中A，B，C 就是所有的异常类型
若无异常接口声明，说明函数可能抛出任何类型的异常
不抛掷任何类型异常的函数声明：
`void fun() throw();`
### 异常处理中的构造和析构
自动析构：
try块中如果遇到异常直接抛出，后面的不执行。
然后寻找匹配的catch语句，初始化异常参数，将从对应的try块开始，到异常抛出之间构造且尚未被析构的所有自动对象进行析构
最后从最后一个catch之后开始恢复执行
## 标准程序库异常处理
`#include <stdexcept>`
标准异常类的基础：
exception：标准程序库异常类的公共基类
logic_error：表示可以在程序中被预先检测到的异常（可以避免）
runtime_error：表示难以被预先检测的异常
```C++
#include <stdexcept>
#include <iostream>
#include <cmath>

using namespace std;
//三角形面积计算
double area(double a, double b, double c) throw (invalid_argument){
    if (a <= 0 || b <= 0 || c <= 0)
        throw invalid_argument("length should be positive");
    if (a + b <= c || b + c <= a || c + a <= b)
        throw invalid_argument("cannot form a triangle");
    double p = (a + b + c) / 2;
    return sqrt(p * (p - a) * (p - b) * (p - c));
}

int main(){
    double a, b, c;
    cin >> a >> b >> c；
    try{
        double s = area(a, b, c);
        cout << "Area: " << s << endl;
    }catch(exception &e){        //exception是invalid_argument的父类所以这里可以笼统地写exception
        cout << "Error: " << e.what() << endl;        //e.what()可返回错误处字符串
    }
    return 0;
}
```