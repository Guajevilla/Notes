# Protobuf
protocol buffers 是一种语言无关、平台无关、可扩展的序列化结构数据的方法，它可用于（数据）通信协议、数据存储等。

目前有2.0和3.0两个大版本，项目上是基于2.0

通过在 .proto 文件中定义 protocol buffer message 类型，来指定你想如何对序列化信息进行结构化。每一个 protocol buffer message 是一个信息的小逻辑记录，包含了一系列的 name-value 对。
一旦定义了 messages，就可以在 .proto 文件上运行 protocol buffer 编译器来生成指定语言的数据访问类。这些类为每个字段提供了简单的访问器（如 name()和 set_name()），以及将整个结构序列化为原始字节和解析原始字节的方法

Example:
```C++
syntax = "proto3"; // proto3 必须加此注解

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
  enum Corpus {
    UNIVERSAL = 0;
    WEB = 1;
    IMAGES = 2;
    LOCAL = 3;
    NEWS = 4;
    PRODUCTS = 5;
    VIDEO = 6;
  }
  Corpus corpus = 4;
}
```
上面便是定义好的一个 message，里面包含：

1. String 类型的 query，编号是 1 （字段必须有编号且编号不允许重复）
2. int 类型的 page_number，编号是 2
3. 枚举类型的 corpus （枚举内部的编号也不允许重复，并且第一个编号必须为0）

# gflags
`#include <gflags/gflags.h>`
gflags是google的一个开源的处理命令行参数的库
使用gflags定义一个flag只需要使用相应类型对应的宏定义就可以,相应的宏包括(复杂的变量需要用专门的解析工具来进行解析，不放在flag中进行定义和解析)：
```C++
DEFINE_bool: boolean
DEFINE_int32: 32-bit integer
DEFINE_int64: 64-bit integer
DEFINE_uint64: unsigned 64-bit integer
DEFINE_double: double
DEFINE_string: C++ string
//命名、默认值、提示性说明
DEFINE_bool(big_menu, true, "Include 'advanced' options in the menu listing");
DEFINE_string(languages, "english,french,german",
              "comma-separated list of languages to offer in the 'lang' menu");
```
在gflags中flag可以分散的定义在各个文件之中，而不用定义在一起，这就意味着我们可以在一个单独的文件中只定义这个文件所需要用到的一些flag，链接了该文件应用都可以使用该文件中的flag，这样就能非常方便的实现代码的复用，如果不同的文件定义了相同的flag，则会产生错误，所以需要明确规范gflags的使用规范。

宏定义的flag都可以像普通的变量一样进行调用，定义的变量以FLAGS_为前缀!如之前的例子中可以 `FLAGS_languages += ",klingon";`
在一个文件中如果想要使用另外一个文件中的flag，只需要在.h的头部进行如下声明 `DECLARE_bool(big_menu); `

使用：在main函数中加入：（一般是放在main函数的头几行）
`google::ParseCommandLineFlags(&argc, &argv, true);`
前两个参数通常户会在main()函数中给出，最后一个参数是remove_flags，如果为true的话，ParseCommandLineFlags会移除相应的flag和对应的参数并且修改相应的argc，然后argv只会保留命令行参数；反之如果remove_flags为false的话，会保持argc不变，但是会调整argv中存储内容的顺序，并且把flag放在命令行参数的前面

## 完整性检验
在定义一个flag之后，可以选择定义一个验证器来对该flag进行校验，每当利用 SetCommandLineOption()函数更改flag的值得时候，都会利用该验证器对flag的值进行合理性验证，当值是合理的时候，验证器会返回true；当值是不合理的时候，验证器会返回false并报错。如：
```C++
static bool ValidatePort(const char* flagname, int32 value) {
   if (value > 0 && value < 32768)   // value is ok
     return true;
   printf("Invalid value for --%s: %d\n", flagname, (int)value);
   return false;
}
DEFINE_int32(port, 0, "What port to listen on");
DEFINE_validator(port, &ValidatePort);
```
# glog
`#include <glog/logging.h>`
Glog是一个应用程序的日志库。它提供基于C++风格的流的日志API，以及各种辅助的宏。打印日志只需以流的形式传给 LOG
```C++
int main(int argc, char* argv[]) {
  // Initialize Google's logging library.
  google::InitGoogleLogging(argv[0]);

  // ...
  LOG(INFO) << "Found " << num_cookies << " cookies";
  // 只在满足一定条件的时候打印日志：
  LOG_IF(INFO, num_cookies > 10) << "Got lots of cookies";
  // 代码被执行多次，只想对其中某几次打印日志。如下每10次打印一次
  LOG_EVERY_N(INFO, 10) << "Got the " << google::COUNTER << "th cookie";
  // 只打印前20次
  LOG_FIRST_N(INFO, 20) << "Got the " << google::COUNTER << "th cookie";
  // 结合起来
  LOG_IF_EVERY_N(INFO, (size > 1024), 10) << "Got the " << google::COUNTER
                                        << "th big cookie";
}
```
可以指定下面这些级别（按严重性递增排序）： INFO, WARNING, ERROR, and FATAL 。打印 FATAL 消息会在打印完成后终止程序。
如果不指定的话，Glog输出到文件 /tmp/<program name>.<hostname>.<user name>.log.<severity level>.<date>-<time>.<pid> （比如 /tmp/hello_world.example.com.hamaji.log.INFO.20080709-222411.10474）。默认情况下，Glog对于 ERROR 和 FATAL 级别的日志会同时输出到stderr。

## CHECK 宏
CHECK 宏和标准库中的 assert 宏类似，可以在给定的条件不满足时终止程序。CHECK 和 assert 不同的是，它不由 NDEBUG 控制，所以一直有效。
用参数和操作符op决定了要么不符合条件返回NULL，要么返回一个string*，如果返回NULL，则不再执行下面的输出，否则则输出日志信息。
```C++
CHECK_EQ(some_ptr, static_cast<SomeType*>(NULL)); //为避免空指针报错转化类型比较
//CHECK_NOTNULL检查空指针等,该宏会返回传入的指针，因此在构造函数的初始化列表中非常有用，如下：
struct S {
  S(Something* ptr) : ptr_(CHECK_NOTNULL(ptr)) {}
  Something* ptr_;
};
```
CHECK_DOUBLE_EQ , CHECK_NOTNULL , CHECK_NE(not equal) , CHECK_GT(greater than) , CHECK_GE(greater equal)

VLOG 宏，使你可以自定义一些日志级别。通过 --v 可以控制输出的细节日志:(级别越低的 VLOG 越会打印)
```C++
VLOG(1) << "I'm printed when you run the program with --v=1 or higher";
VLOG(2) << "I'm printed when you run the program with --v=2 or higher";
```
# gtest
1. 测试应该是独立的、可重复的。一个测试的结果不应该作为另一个测试的前提。GTest中每个测试运行在独立的对象中。如果某个测试失败了，可以单独地调试它。
2. 测试应该是有清晰的结构的。GTest的测试有很好的组织结构，易于维护。
3. 测试应该是可移植和可复用的。有很多代码是不依赖平台的，因此它们的测试也需要不依赖于平台。GTest可以在多种操作系统、多种编译器下工作，有很好的可移植性。
4. 测试失败时，应该给出尽可能详尽的信息。GTest在遇到失败时并不停止接下来的测试，而且还可以选择使用非终止的失败来继续执行当前的测试。这样一次可以测试尽可能多的问题。
5. 测试框架应该避免让开发者维护测试框架相关的东西。GTest可以自动识别定义的全部测试，你不需要一一列举它们。
6. 测试应该够快。GTest在满足测试独立的前提下，允许你复用共享数据，它们只需创建一次。

使用GTest，可以从写断言开始。断言的结果有三种：成功、非终止的失败、终止的失败。终止的失败会终止当前的测试函数。
断言有两种。 ASSERT_* 的版本会终止当前函数， EXPECT_* 的版本则不会终止。一般来说 EXPECT_* 更常用，因为它可以输出测试的多个失败。
```C++
EXPECT_NE(val1, val2);  // val1 != val2
EXPECT_STRNE(str1, str2);   // 两个C字符串内容不同
EXPECT_STRCASEEQ(expected_str, actual_str);   // 忽略大小写，两个C字符串内容相同
```

1. 使用 TEST() 宏定义和命名一个测试函数。它们就是普通的C++的无返回值函数。
2. 函数中可以使用任何C++表达式，以及GTest中的断言。
3. 如果任一断言失败了（终止或非终止的），或者如果测试崩溃了，该测试失败；反之成功。
```C++
// Tests factorial of positive numbers.
TEST(FactorialTest, HandlesPositiveInput) {
  EXPECT_EQ(1, Factorial(1));
  EXPECT_EQ(2, Factorial(2));
  EXPECT_EQ(6, Factorial(3));
  EXPECT_EQ(40320, Factorial(8));
}
```
TEST() 的第一个参数是测试用例名，第二个参数是测试用例中的测试名。必须是有效的C++标识符，并且不能包含下划线。测试的全名由这两个名字组成。不同测试用例的测试的名字可以相同。
## 捆绑测试：给多个测试使用相同的数据配置
如果有多个测试使用类似的数据，可以使用捆绑测试（text fixture）。它允许几个不同的测试复用相同的配置。
创建一个捆绑：
1. 从 ::testing::Test 派生一个类。使用 protected: 或 public: ，因为我们需要能从子类访问捆绑的成员。
2. 在类中声明任何你想用的对象。
3. 如果有必要，实现默认构造函数或者 SetUp() 函数来为测试准备数据。
4. 如果有必要，实现一个析构函数或者 TearDown() 函数来释放在 SetUp() 中分配的资源。
5. 如果需要，定义用于共享的子例程。
