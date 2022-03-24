## 基础语法
1.数据类型：
`
int long float complex bool Str  List[] Tuple() Dictionary{}
`

- 获取数据类型： 用type()得到变量数据类型
- 转换数据类型：int(3.14)，int('3')
- 还原数据类型（去掉引号）:eval()
- 键盘输入函数：s = input('输入：') 得到的内容是字符串

```python
name = "chen"
age = 10
hight = 180
print(f'我得名字是{name}我得年龄是{age}')
print('我得名字是%s我得年龄是%d身高是%.2f'% (name, age, hight))
```

> %c 字符， %s 字符串， %d 有符号的十进制, %u 无符号十进制, %o 八进制, %x 十六进制, %f 浮点数

2.算术运算符:
```
+ - * /
// 整除(求商) 9//2=4
% 取余数
** 指数 幂运算2**3=8
() 可以改变优先级
```

3.赋值运算符:
=，+=， c+=a  ==> c = c + a

4.比较运算符：
==，!=，>，<，>=，<=

5.逻辑运算符：
and，or，not

6.PEP8注释规范：
- 单行注释#开头，后面加一个空格# 123      
- 行内注释需要两个空格#  123
- 多行注释：'''或者"""


## 判断结构：
1.if else结构
```
if: 条件1
  代码
elif: 条件2
  代码
else:
  代码
```

2.三目运算

判断成立即表达式1，判断不成立表达式2
```python
# a = 表达式1 if 判断条件 else 表达式2 
a = 1
b = 2
c = a - b if a > b else b - a
```

## 随机random
产生 [a, b] 之间的随机整数,包含 a 和 b
```python
import random
num = random.randint(1, 10) 
```

##循环语句
1.while语句：
条件成立会一直执行
```python
while True: 
    pass
```
2.for语句:
```python
for i in '213123':  # 字符串里写出了i的顺序,也就是in后面跟着的是数组
    pass
for i in range(5): # 0-4
    pass
for i in range(3, 7): # 3-6
    pass
for i in range(3, 19, 3): #3-18 步长3  这边是in后面跟着的是条件i的范围和步长
    pass
for i in range(10):
    for j in range(i + 1):
        print("*", end=' ')
    print() # 单独打印print，会自动换行，想要取消换行就加end，end后面输入的表示前面输入后面跟着的东西
# for else 结构 # 如果循环里面存在，即执行循环里面代码，否则执行else代码
```

## break，continue
break，continue是关键字，用法跟java一样
```python
for i in '1213451234':
    if i == '5':
        print("存在5")
        break
else:
    print("不存在5")
```

## 字符串

**注意**
- 带引号的内容，python中字符串可以乘上一个整数 "213" * 3 ==> "213213213"
- 单引号和双引号没区别，即'123' 和 “123”和 '''123'''完全一致
- 如果字符串里有包含单引号，则用双引号定义，或者统一用三引号定义

1.字符串长度

```python
len('213') # 3
```

2.字符串切片：

```python
a = 'heloab' 
c = a[1:4:2] # 开始下标，结束下标，步长
c = a[1:4] # 默认步长为1 eloa
c = a[1:] # 从1-最后 eloab
c = a[:2] # 从0-2 hel
c = a[:] # 相当于复制 heloab
c = a[::-1] # 逆置 baoleh
c = a[::2] # 从0-最后步长为2 hla
```

3.字符串查找方法：

>  find(), rfind()

```python
"hello chen liang".find("chen") = 6 # 查找到返回下标
"hello chen liang".find("chen", 3, 11) = 6 # 将原来字符串切割出3 - 11， 返回下标，还是原来字符串下标
"hello chen liang".rfind("liang") = 11 # 从右向左查找，返回下标
```

4.字符串替换方法： 

> replace(old, new, count) 

count是替换的次数，默认是全部替换

```python
"hello".replace("l", "t") = "hetto"
"hello".replace("l", "t", 1) = "hetlo"
```

5.字符串分隔方法：

> split(sub_str, count) 

count是分割几次，默认是全部分割，返回是列表[]

```
"helloalaq".split("l") # ==> ['he', '', 'oa', 'aq']
```

6.字符串拼接方法： 

> join(字符串列表)

```python
"_".join(['a', '2', 'b']) # ==> a_2_b
```
字符串可以通过+号直接拼接，跟Java一样

## 列表[]
是python得一种数据类型，可以存放多个数据，任意数据类型

1.创建列表
```python
a = list()
a = [] # 空列表
a = [0, 1, 2, 3, 4]
a[-1] # ==> 4
a[1:3] # ==> [1, 2]
```

2.列表遍历:

两种遍历方式
```python
a = []
for i in a:
    pass

i = 0 
while i < len(a): 
    pass
    i+=1
```

3.添加数据
```python
a = []
a.append(1) # 向尾部添加数据，是单个元素 ==> [0, 1, 2, 3, 4, 1]
a.extend('12') # 向尾部添加可迭代数据 ==> [0, 1, 2, 3, 4, '1', '2']
a.insert(1, '12') # 向下标1地方插入数据 ==> [0, '12', 1, 2, 3, 4]
```

4.查询数据
```python
a = []
a.index(2) # ==> 2 # 查询下标
a.count(1) # ==> 1 # 统计出现的次数
b = 3.14 in a # ==> False # 判断是否存在
b = 3.14 not in a # ==> True # 判断是否存在
```

5.删除操作
```python
a = []
a.remove(2) #删除数据项2 数据不存在会报错==> [0, 1, 3, 4]
a.pop() # 删除最后一个数 ==> [0, 1, 2, 3]
a.pop(2) # 删除下标为2的数据 下标不存在会报错 ==> [0, 1, 3, 4]
del a[3] # 删除下标为3的数, 删除不存在下标会报错 ==> [0, 1, 2, 4]
```

6.排序
```python
a = [0, 3, 4, 5, 2]
a.sort() # ==> [0, 2, 3, 4, 5]
a.sort(reverse=True) # ==> [5, 4, 3, 2, 0]
b = sorted(a)/sorted(a, reverse=True) # 该方法不会影响原本的a
```

7.逆置
```python
a = [0, 3, 4, 5, 2]
b = a[::-1] # 不影响原来的a
a.reverse() # 将a逆置
```

8.列表嵌套
```python
a = [[],[],[]]
```

## 元组 tuple
元组和列表非常相似，可以存放不同数据，存放不同的数据类型

列表[], 元组()
```python
a = (0, 3, 4, 5, 2)
a = (0,)
```
**注意**
- 列表数据可以修改，元组数据不能被修改，只能查询
- 定义一个元素的数据，必须加,  

##字典 
dict定义使用{},是键值对组成{key-value}

```python
a = {'name': 'chenliang', 'age': '28', 'like': ['打球', '电影'], 1: 2}
```
1个key-value是一个元素

1.空字典：
```python
a = {}
a = dict()
```
2.查询
```python
a = {'name': 'chenliang', 'age': '28', 'like': ['打球', '电影'], 1: 2}
a['like'][1] # ==> '打球' # 如果key值不存在，则会报错
a.get('gender') # ==> None # key值不存在，不会报错，返回None
len(a) # ==> 4 # len()字典长度
```
3.字典添加数据：

```python
a = {'name': 'chenliang', 'age': '28', 'like': ['打球', '电影'], 1: 2}
# a[key] = value  # 如果key存在，就是修改数据，如果key不存在就是添加数据
```

4.字典删除数据
```python
a = {'name': 'chenliang', 'age': '28', 'like': ['打球', '电影'], 1: 2}
a.pop('name') # 删除，返回删除的value
del a['name'] # 根据key得值删除数据
del a  # 删除字典，没法再用了
a.clear() # 清空字典，删除所有键值对
```

5.字典中遍历数据
```python
a = {'name': 'chenliang', 'age': '28', 'like': ['打球', '电影'], 1: 2}
for key in a:
  print(key, a[key])
b = a.keys() # 获取到的类型是dict_keys，可以用list()进行类型转换，也可以直接进行遍历
b = a.values() # 获取到的类型是dict_values，可以用list()进行类型转换，也可以直接进行遍历
b = a.items()  # 获取到的是键值对，类型是dict_items，也可以用list()进行类型转换，也可以直接进行遍历
```

6.enumerate函数：
将可迭代序列中元素所在下标和具体元素数据组合在一块，变成元组

```python
a = ['a', 's', 'e', 'q']
b = enumerate(a)
print(b) # ==> <enumerate object at 0x000001B8C2E82D80>
for i in b:
    print(i)  # ==> (0, 'a')  (1, 's')  (2, 'e')  (3, 'q')
```

## 公共方法：
- `+`  支持 字符串、列表、元组进行操作， 得到一个新的容器
- `* 整数` 复制， 支持 字符串、列表、元组进行操作， 得到一个新的容器
- `in/not in`  判断存在或者是不存在，支持 字符串、列表、元组、字典进行操作， 注意： ==如果是字典的话，判断的是 key 值是否存在或不存在==
- `max/min` 对于字典来说，比较的字典的 key值的大小

## 函数
定义函数用关键字：def，函数名遵循命名规则

1.定义函数
> def 函数名():

```python
def a(b):
    """
    这是循环
    :param b: 
    :return: 
    """
    # 注释必须写在函数名得下方
    for i in b:
        print(i)
s = [1, 2, 3, 4]
a(s)
help(a)  # 查看文档注释可以使用help(函数名)
```

2.全局变量和局部变量

```python
a = 30
def fun():
    a = 40 # 函数中定义的是局部变量，跟外部的全局变量没有关系
fun()
print(a) # ==>30

a = 30
def fun():
    global a  # 在函数中想要使用全局变量，必须使用global关键字
    a = 40
fun()
print(a) # ==>40

a = 30
b = 20
def fun(n, m):
    c = n + m
    return c   # 返回函数值，执行到此结束，return多个函数值就用逗号隔开，默认是组成元组进行返回的
print(fun(a, b))
```

3.return
- return 关键字后边可以不写数据值， 默认返回 None
```
def func():
    xxx
    return   # 返回 None，终止函数的运行的
```
   
- 函数可以不写 return，返回值默认是 None
```
def func():
    xxx
    pass
```
   
4.传参两种形式
```python
def fun(a,b,c):
    pass
fun(1,2,3)  # ==> 位置传参，和形参的位置顺序保持一致
fun(a=1,b=2,c=3)  # ==> 参数传参，指定了参数的值
```
注意:可以使用混合传参，但是一定要是先写位置传参，再写参数传参

形参方式
```python
def fun(a,b,c=10):
    pass
fun(1,2)  # ==>  没有写c，则参数默认用10
fun(1,2,3)  # ==> 写了c，则参数用传递的数据3
```
也就是有默认值的参数，可以不用传

- 在形参前面加上一个*，该形参变为不定长元组形参，可以接受所有位置的实参，类型是元组()
- 在形参前面加上两个*，该形参变为不定长字典形参，可以接受所有关键字实参，类型是字典{}

```python
def func(*args, **kwargs):
    print(args)
    print(kwargs)
func(1, 2, 3, 4, 5)  # ==>(1, 2, 3, 4, 5)  {}
func(a=1, b=2, c=3, d=4)  # ==>()  {'a': 1, 'b': 2, 'c': 3, 'd': 4}
func(1, 2, 3, a=4, b=5)  # ==>(1, 2, 3)  {'a': 4, 'b': 5}
def func(a, *args, b=1, **kwargs): # 普通形参，不定长元组形参，缺省形参，不定长字典形参
    pass
```
5.拆包和组包

组包：将多个数据值，组成元组，给到一个变量

```python
a=1,2,3
def fun(): return 1,2
# 拆包：将容易的数据分别给到多个变量，注意，数据的个数要和变量的个数保持一致
b,c,d = a
e,f = fun()
list1 = [1,2]
a,b = list1  # ==>1 2
dict1 = {'name':'chen','age':18}
a,b = dict1  # ==>name age
a, b = b, a  # 先进行组包，再进行拆包，相当于更换两个数据的值
```

6.引用

使用id()查看变量的引用，id值认为是内存地址的值，python数据值转递是引用，赋值运算符可以改变变量的引用

- 可变类型：在不改变变量引用的前提下，改变引用中数据。即地址不变，数据变。==> list dict
- 不可变类型：改变数据，引用也跟着变。即数据变，地址变。 ==> int float bool str tuple

```python
tuple1 = (1,2,[3,4])
print(tuple1, id(tuple1)) # ==> (1, 2, [3, 4]) 2187341722816
tuple1[2][1] = 10
print(tuple1, id(tuple1))  # ==> (1, 2, [3, 10]) 2187341722816
```
注意：可变类型在函数中不需要global修饰，因为引用值不发生变化。修改的还是全局变量。
```python
list1 = [1,2,3]
def fun1():
    list1.append(4)
fun1() # ==> [1, 2, 3, 4]
a=1
def fun1(a):
    a = a + 1
fun1(a)
print(a) # ==> 1 # 这里打印的还是全局变量a，调用函数没有有，里面的a值改变，引用也改变了。

```

7.匿名函数

> 使用关键字lambda定义函数即匿名函数

```
1.无参数，无返回值
def 函数名():
    函数代码
lambda: 函数代码
2.无参数有返回值
def 函数名():
    return 1+2
lambda: 1+2
3.有参数无返回值
def 函数名(a,b)
    print(a, b)
lambda a,b: print(a, b)
4.有参数有返回值
def 函数名(a,b)
    return a+b
lambda a,b: a+b
```

匿名函数使用场景 - 作为函数参数

```python
def fun1(a, b, fun2):
    print(fun2(a, b))
fun1(1, 2, lambda a, b: a + b)
```

```python
list1 = [{'name': 'd', 'age': 19}, {'name':'b', 'age': 16}, {'name': 'a', 'age': 16}, {'name': 'c', 'age': 20}]
list1.sort() # ==>会直接报错，因为字典无法直接排序，所以需要给sort方法指定排序规则
list1.sort(key=lambda x: x['name'])
```
查看sort方法sort(cls,key,reverse)

取了里面每个元素的name标签，然后进行排序
```python
list2 = ['dasd', 'wedas', 'ds', 'dfs']
list2.sort(key=lambda x: len(x))
```
根据列表中字符串长度进行排序 ,里面的x相当于取了每个元素，通过方法，返回

## 列表推导式
为了更快速生成一个列表
```python
#  相当于循环输出i
list2 = [i for i in range(5)]  
# [0, 1, 2, 3, 4]  

#  相当于循环输出i
list2 = [i + i for i in range(5)]  
# [0, 2, 4, 6, 8]  

#  相当于循环输出hello
list2 = ['hello' for i in range(5)]  
# ['hello', 'hello', 'hello', 'hello', 'hello']

list2 = [f'num{i}' for i in range(5)] 
# ['num0', 'num1', 'num2', 'num3', 'num4']

list2 = [(i, j) for i in range(2) for j in range(3)] 
# [(0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2)]
```

## 集合set{}

```python
set1 = {1, 1.0, True, 'chen', (1, 2)}  
# ==> {1, (1, 2), 'chen'} <class 'set'>

set1 = {1, 3.14, True, 'chen', (1, 2)} 
# ==> {3.14, 1, (1, 2), 'chen'} <class 'set'>
```
**注意**
- 集合set中类型必须是不可变类型，不能放入列表[] 和字典{}
- 集合set是可变类型
- True和1.0本质上和1同等，所以不输出，集合是无序的（不支持下标操作），数据不重复（去重）
- 集合，列表，元组 三者之间可以相互转换

1.删除: 
默认删掉排序完的第一个，因此你也不知道删掉的是哪一个
```python
set1 = {1, 1.0, True, 'chen', (1, 2)}  
set1.remove(3.14)
set1.pop()
```

2.添加：
```python
set1 = {1, 1.0, True, 'chen', (1, 2)}  
set1.add(100)
```

3.修改数据：

即先删除数据，再添加数据

4.清空：
```python
set1 = {1, 1.0, True, 'chen', (1, 2)}  
set1.clear()
```

## 文件操作

1.打开文件，将文件从硬盘加载到内存中
文件名 打开方式{r(read),w(write),a(append)} encoding文件编码{gbk,utf-8}
```
f = open(file, mode='r', encoding) 
buf = f.read(n) # n表示一次读取多少字节的内容，可以不加读取全部内容, read()一次读取全部内容，读到末尾会返回空
buf = f.readlines() # 按行读取，一次读取所有行，返回值是列表，每个元素是个字符串，是一行内容
f.write(内容) # 写入文件
f.close() #关闭文件
```

2.读取操作
```python
f = open('1.txt', 'r')
buf = f.read()
print(buf)
f.close()
```

3.写入操作
```python
f = open('1.txt', 'w', encoding='utf-8')  # w的方式打开文件，不存在会创建文件，存在会覆盖
f.write('chen')
f.close()
```

4.追加操作
```python
f = open('1.txt', 'a', encoding='utf-8')  # a的方式打开文件，追加内容，文件不存在会创建文件
f.write(' liang')
f.close()
```

5.二进制文件：mp3, mp4,avi,png 基本上视频和图片

> 二进制文件打开方式：rb wb ab

| 访问模式 | 说明                                                         |
| :------- | -----------------------------------------------------------: |
| r        | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。 |
| w        | 打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| a        | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| rb       | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。 |
| wb       | 以二进制格式打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| ab       | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| r+       | 打开一个文件用于读写。文件指针将会放在文件的开头。           |
| w+       | 打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| a+       | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| rb+      | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。 |
| wb+      | 以二进制格式打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| ab+      | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |


6.备份代码：
```python
file_name = input("请输入要备份得文件名：")
f = open(file_name, 'rb')
buf = f.read()
f.close()
index = file_name.rfind('.') # 找到.可以切割文件名和后缀
new_file_name = file_name[:index] + '[备份]' + file_name[index:]  # 生产新的备份文件名
print(new_file_name)
f_w = open(new_file_name, 'wb')# 写入文件
f_w.write(buf)
f_w.close()
```

7.文件和文件夹操作
```python
import os
os.rename('a.txt','b.txt') # 文件重命名，源文件路径，新文件路径
os.remove('a.txt') # 删除文件
os.mkdir('test/aa') # 创建目录
os.rmdir('test/aa') # 删除空目录
os.getcwd() # 获取当前所在的目录
os.chdir('test') # 修改当前目录 change dir 相当于进入一个文件夹。os.chdir('../') 放回上一级
os.listdir('test') # 获取指定目录下内容，返回值是列表，每一项是文件名
```

读取文件
```python
import os
if os.path.exists('a.txt'):
    f = open('a.txt','r',encoding='utf-8')
buf=f.read()
if buf:
    list1 = eval(buf)
f.close()
```

## 面向对象
1.构造对象
```
class 类名(object):
    def 函数名(cls):
        pass
```
**注意**
- self就是一个形参得名字，可以写其他形参名，一般不修改默认self，
- 通过调用方法得时候，不需要手动传递实参，
- 是python解释器自动将调用该方法得对象传递给self，所以self代表对象

```python
class Dog(object):
    def eat(cls):
        print("狗在吃东西")
dog = Dog() # 创建对象 此时函数的self就代表了此对象，两者地址相同
dog.eat() # ==> 狗在吃东西
dog.name = '大黄'  # 添加属性，如果存在就是修改
dog.age = 3
```

2.魔法方法:
> 该方法__开头__结尾，满足特定条件下会自动调用，该方法称为魔法方法

__init()__：在对象创建后会立即调用
作用：1.用来给对象添加属性，初始值。2.代码业务需求，将执行代码写进去
如果__init()__方法中，有self以外的形参，那么在创建对象的时候，需要给额外的形参船体实参

```python
class Dog(object):
    def __init__(cls):
        print("这是init")
        cls.name = '小狗'
dog = Dog() # ==> 这是init
print(dog.name)  # ==> 小狗
dog1 = Dog() # ==> 这是init
print(dog1.name)  # ==> 小狗

class Dog(object):
    def __init__(cls, name):
        print("这是init")
        cls.name = name
dog = Dog('大黄') # ==> 这是init
print(dog.name)  # ==> 大黄
dog1 = Dog('小黑') # ==> 这是init
print(dog1.name)  # ==> 小黑
```

__str__:1.print(对象)会自动调用该方法，打印结果是该方法返回值。2.str(对象) 类型转换字符串会自动调用。
应用：1.打印对象时候需要输出属性信息。2.需要将对象转换成字符串类型时候。
注意：方法返回必须是一个字符串，因此只有self一个参数

```python
class Dog(object):
    def __init__(cls, name):
        print("这是init")
        cls.name = name
    def __str__(cls):  # 相当于把对象做成了string返回出去，类似java的toString
        print("这是str")
        return cls.name  # 这里必须返回一个字符串
dog = Dog('大黄') # ==> 这是init
s = str(dog) # ==> 这是这是str
print(s)   # ==> 大黄
```

__del__:折构函数
1.对象在内存中销毁的时候（引用计数为0）会自动调用__del__方法
2.使用del变量，将该对象引用计数变为0，会自动调用__del__方法
应用：对象要被删除时候，要书写代码放在里面，一般很少用
del 对象  ==>引用计数-1，当变为0时候，会调用方法

Python 中的 __str__ 和 __repr__ ⽅法都是⽤来显示
的，即描述对象信息的。
1. __str__ 的⽬标是可读性，或者说， __str__ 的结
果是让⼈看的。主要⽤来打印，即 print 操作，
2. __repr__ 的⽬标是准确性，或者说， __repr__ 的
结果是让解释器⽤的。 __repr__ ⽤于交互模式下提
示回应，
3. 如果没有重写 __str__ ⽅法，但重写了 __repr__ ⽅
法时，所有调⽤ __str__ 的时机都会调⽤__repr__
⽅法。

3.继承：
```
class 类B(类A)
    pass
B继承A
```

单继承：一个类只有一个父类
子类可以重写父类的同名方法

子类调用父类的方法：
必须在子类的方法里进行调用
```python
class Dog(object):
    def eat(cls):
        print('狗吃骨头')
class AA(Dog):
    def see(cls):
        Dog.eat(cls)  # 父类名.方法名调用父类方法，自动将对象作为实参传递给self
        super(AA, cls).eat() # super关键字，调用当前类的父类
        super().eat() # 相当于上一个方法的简写
dog = AA()
dog.see()
```

如果子类重写了父类的__init__方法，在子类需要调用父类的__init__方法给对象添加从父类继承的属性
```python
super().__init__(参数)
class Dog(object):
    def __init__(cls, name):
        cls.name = name
class AA(Dog):
    def __init__(cls, name, color):
        super().__init__(name) # 子类方法的形参，一般先写父类的形参，再添加自己独有的形参
        cls.color = color
```

多继承
class AA(God, Dog) # 多个父类逗号隔开。可以调用两个父类的属性和方法

4.私有权限
> 在方法和属性前面加上两个下划线，即私有

***
1.不能被类外部直接访问和使用，只能内部访问和使用
2.不能被子类继承
```python
class People(object):
    def __init__(cls):
        cls.__money = 0
    def get_money(cls): # 定义get接口。获取余额
        return cls.__money
    def set_money(cls, money):
        cls.__money = money
```

5.类属性
```python
class People(object):
    class_name = '人类'
    def __init__(cls, name):
        cls.name = name
p = People('chen')
print(p.__dict__) # ==> {'name': 'chen'}  打印dog对象具有的属性，字典方式展现
print(p.class_name) # ==> 人类  访问类属性
print(People.class_name)  # ==> 人类
```

- 实例属性：就是每个对象都存在一份，但是值不一样。相当于成员变量
- 类属性：在类内部，公用一个。相当于共有静态变量
- 如果存实例属性和类属性的重名，使用实例访问的一定是实例属性，不是类属性

6.类方法

> 用@classmethod修饰

```python
class People(object):
    class_name = '人类'
    def __init__(self, name):
        self.name = name
    @classmethod
    def get_class_name(cls):  # cls 是类方法的默认形参，在调用的时候，不需要手动传递，python会自动传递 
        return cls.class_name  # cls 代表类对象自己，访问的是类方法和类变量，意思就是共用的东西
p = People('chen')
print(p.get_class_name())  # ==> 人类

    @classmethod
    def get_class_name(cls, a):
        a = a + 1
        print(a)
        return cls.class_name # 输出类属性
print(p.get_class_name(1))  # 静态方法如有参数，就必须传递参数值
```

7.静态方法
用@staticmethod修饰

8.多态

- 实例化父类的时候，调用的是父类的方法
- 实例化子类的时候，调用的是子类的重写方法

## 异常

```
try:
    可能发生异常的代码
except 异常类型：
    发生异常执行的代码

捕获多个异常
try:
    可能发生异常的代码
except （异常类型1， 异常类型2）：
    发生异常执行的代码

try:
    可能发生异常的代码
except 异常类型1：
    发生异常1执行的代码
except 异常类型2：
    发生异常1执行的代码
except 异常类型3：
    发生异常3执行的代码
```

1.打印异常信息

```python
try:
    pass
except Exception as e:
    print('内容', e)
```

捕获所有异常：Exception

- Exception 是常见异常类的父类,
- ZeroDivisionError --> ArithmeticError --> Exception --> BaseException  ---> object
- ValueError --> Exception --> BaseException  ---> object

异常完整结构
```
try:
    可能发生异常的代码
except Exception as e:
    发生异常执行的代码
    print(e)
else:
    代码没有发生异常,会执行
finally:
    不管有没有发生异常,都会执行
```

2.抛出异常：

> raise 异常对象  # 当程序代码遇到 raise时候，程序就报错了

- 自定义异常类，基础Exception或者baseException
- 选择书写，定义__init__方法和__str__方法
- 合适的时机抛出异常

```python
class DefineError(Exception):
    pass
def get_num():
    raise DefineError('有错')
try:
    get_num()
except DefineError as e:
    print(e)
finally:
    print("最终执行")
```


## 模块
注意：如果导入的事自己书写的模块，使用的模块和代码文件需要在一个目录中

- 方法一：import 模块名， 使用：模块名.方法名
- 方法二：from 模块名 import 方法1， 方法2， 使用：方法名
- 方法三：from 模块名 import * 将方法全部导入

```
import my_module1 as mm1  
# 起别名就不能再用原来的名字 使用： 
# mm1.func()
```

2.模块中的__all__变量

可以在每个代码文件中定义，类型是元组，列表
作用：影响from 模块名 import *的行为，如果定义了all变量，则只能导入all变量定义的内容
__all__ = ['name', 'func']  # 可以写变量名和函数名

3.模块中的__name__变量

*每个模块都有，是系统自己定义的*

- 直接运行当前代码，值为__main__
- 把文件作为模块导入时，运行，结果是 模块名 相当于每个模块中加上if __name__ == '__main__': 则等同于测试案例

**注意：**
- 自己定义的模块名字，不要和系统中你要使用的模块名字相同
- 模块的搜索顺序，当前目录--系统目录--程序报错
- 包：功能相近或者相似的模块放在一个目录中，并且在目录中定义一个 __init__.py文件，这个目录就是包
- from 包名 import *  # 导入的是__init__.py中的内容
```
from my_package import *
```



