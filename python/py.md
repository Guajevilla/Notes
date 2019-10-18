# py

引用全局变量,不需要golbal声明.修改全局变量,需要使用global声明
特别地.列表.字典等如果只是修改其中元素的值.可以直接使用全局变量.不需要global声明.

## 切片
当s=''时，s[0]和s[-1]会报IndexError: string index out of range，但是s[:1]）和s[-1:]不会。
L=[0, 1, 2, 3, ..., 99]，L[::5]=[0, 5, 10, 15, 20,······, 90, 95]
## 迭代
isinstance('abc', Iterable) # str是否可迭代  
for i, value in enumerate(['A', 'B']): //输出0 A 1 B
## 生成器
1. 把一个列表生成式的[]改成()，就创建了一个generator
```
g = (x * x for x in range(10))
for n in g:
     print(n)
```
    
2. 把函数的return改为yield，但yield比较特殊，函数执行到yield后即返回，并中断，下一次进入时从该中断处继续
```
def fib(max): #斐波那契数列生成器
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```
```
def triangles(): #杨辉三角
    L = [1]
    while 1:
        yield L
        L = [1] + [L[i] + L[i+1] for i in range(len(L)-1)] + [1]
```
## 模块
形如`if name == 'main':`放在.py文件最后，用于测试，判断主函数入口（当前函数为主函数，其他调用函数均为模块名，所以这部分不会调用）
## callable
表示对象是否可以调用。对于类对象，只有当类中含有`def __call__(self):`时，其**实例**才可以调用
## return
~~~python
def func():  
    try:  
        print 98  
        return 'ok'             #函数得到了一个返回值  
    finally:                     #finally语句块中的语句依然会执行  
        print (98)
print (fun())
~~~
输出：98 98 ok

函数作为返回值返回:

```python
def lazy_sum(*args):
    def sum():
        x=0
        for n in args:
            x=x+n
        return x
    return sum
lazy_sum(1,2,3,4,5,6,7,8,9) #这时候lazy_sum 并没有执行，而是返回一个指向求和的函数的函数名sum 的内存地址。
f=lazy_sum(1,2,3,4,5,6,7,8,9)
print(type(f))
print(f())  # 调用f()函数，才真正调用了 sum 函数进行求和，
```
输出：<class 'function'>   45
## argparse模块
```python
# python test.py 4 5 6  返回6
# python test.py 4 5 6 --sum 返回15
parser = argparse.ArgumentParser(description='Process some integers.')
parser.add_argument('integers', metavar='N', type=int, nargs='+',help='an integer for the accumulator')
parser.add_argument('-s','--sum', dest='accumulate', action='store_const',const=sum, default=max,
                      help='sum the integers (default: find the max)')
```                      
argparse模块的作用是用于解析命令行参数，注意命令行运行方向是从右往左
使用步骤：
1. import argparse
2. parser = argparse.ArgumentParser()    # 创建一个 ArgumentParser对象

```python
argparse.ArgumentParser(prog=None, 
                        usage=None, 
                        epilog=None, 
                        parents=[], 
                        formatter_class=argparse.HelpFormatter, 
                        prefix_chars='-',                            
                        fromfile_prefix_chars=None,              
                        argument_default=None,
                        conflict_handler='error', 
                        add_help=True)
# eg:
parser = argparse.ArgumentParser(
                    description='SSD Training With Pytorch')
```
3. parser.add_argument()
向对象中添加你要关注的命令行参数和选项

```python
parser.add_argument(name or flags...[, action][, nargs]
                            [, const][, default][, type][, choices]
                            [, required][, help][, metavar][, dest])
```
参数：
* name or flags - 选项字符串的名字或者列表，例如 foo 或者 -f, –foo。
* action - 命令行中该参数存在时的操作，默认值是 store
    * store_ture，表示赋值为true；
    * append，将遇到的值存储成列表，也就是如果参数重复则会保存多个值;
    * append_const，将参数规范中定义的一个值保存到一个列表；
    * count，存储遇到的次数；
* default - 默认值；
* nargs - 应该读取的命令行参数个数，可以是具体的数字，或者是?号，当不指定值时对于 Positional argument 使用 default，对于 Optional argument 使用 const；或者是 * 号，表示 0 或多个参数；或者是 + 号表示 1 或多个参数。
* required - 可选参数是否可以省略 (仅针对可选参数),默认为False；
* const - action 和 nargs 所需要的常量值。
* metavar - 对于必选参数默认就是参数名称，对于可选参数默认是必须存在参数后；
* dest - 解析后的参数名称，默认情况下，对于可选参数选取最长的名称，中划线转换为下划线。

```python
parser.add_argument('-s', '--sum')# 指定一个可选参数，以-为前缀是可选参数
# -短参数，--长参数，同时写表示同一个参数
parser.add_argument('bar')        # 指定一个位置参数，调用脚本时必须输入值
parser.add_argument('--lr', '--learning-rate', 
                        default=1e-3, type=float,
                        help='initial learning rate')
parser.add_argument('--dataset', default='VOC', 
                        choices=['VOC', 'COCO'],
                        type=str, help='VOC or COCO')
```

4. args = parser.parse_args()
每一个add_argument方法对应一个你要关注的参数或选项；最后调用parse_args()方法进行解析，解析成功之后即可使用。

另外，还有互斥参数，互斥参数在调用时不能同时调用两个参数：

```python
group = parser.add_mutually_exclusive_group()
group.add_argument("--square", 
                help="display a square of a given number", type=int)
group.add_argument("--cubic", 
                help="display a cubic of a given number", type=int)
```
## join
```python
str = "-";
seq = ("a", "b", "c") # 字符串序列
print str.join( seq ) # 输出为a-b-c
```