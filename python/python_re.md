# 正则表达式

## 匹配单个字符

| 字符 | 功能 |
|:--- | ---:|
|. |匹配任意1个字符（除了\n）|
|[ ] |匹配[ ]中列举的字符|
|\d |匹配数字，即0-9|
|\D |匹配⾮数字，即不是数字|
|\s |匹配空⽩，即 空格，\t-tab键 \n-换⾏|
|\S |匹配⾮空⽩|
|\w |匹配单词字符，即a-z、A-Z、0-9、_|
|\W |匹配⾮单词字符|

## 匹配多个字符

| 字符 | 功能 |
|:--- | ---:|
|* |匹配前⼀个字符出现0次或者⽆限次，即可有可⽆|
|+ |匹配前⼀个字符出现1次或者⽆限次，即⾄少有1次|
|? |匹配前⼀个字符出现1次或者0次，即要么有1次，要么没有|
|{m} |匹配前⼀个字符出现m次|
|{m,n} |匹配前⼀个字符出现从m到n次|

## 匹配开头结尾

| 字符 | 功能 |
|:--- | ---:|
|^ |匹配字符串开头，注意^[4-7] 和 [ ^4-7]的区别|
|$ |匹配字符串结尾|

## re模块操作
> re.match(pattern, string, flags=0)

从头匹配⼀个符合规则的字符串，从起始位置开始匹配，匹配成功返回⼀个对象，未匹配成功返回None
- pattern： 正则模型
- string ： 要匹配的字符串
- falgs ： 匹配模式

match() ⽅法⼀旦匹配成功，就是⼀个match object对象，⽽match object对象有以下⽅法：
- group() 返回被 RE 匹配的字符串
- start() 返回匹配开始的位置
- end() 返回匹配结束的位置
- span() 返回⼀个元组包含匹配 (开始,结束) 的位置

```python
# 导⼊re模块
import re
# 使⽤match⽅法进⾏匹配操作
result = re.match("正则表达式","要匹配的字符串")
# 如果上⼀步匹配到数据的话，可以使⽤group⽅法来提取数据
if result:
    result.group()  # 提取出匹配出来的东西
```

## 匹配分组 | 

| 字符 | 功能 |
|:--- | ---:|
|  |匹配左右任意⼀个表达式|
|(ab) |将括号中字符作为⼀个分组|
|\num |引⽤分组num匹配到的字符串|
|(?P<name>) |分组起别名|
|(?P=name) |引⽤别名为name分组匹配到的字符串|

## 匹配分组 （）
```python
import re
re.match("\w{4,20}@163\.com", "test@163.com")
re.match("\w{4,20}@(163|126|qq)\.com", "test@126.com")
re.match("\w{4,20}@(163|126|qq)\.com", "test@qq.com")
# 提取区号和电话号码
re.match("(\d{3,4})-(\d{7,8})", "010-12345678")
```

## 匹配分组之 \
```python
import re
re.match("<[a-zA-Z]*>\w*</[a-zA-Z]*>", "<html>hh</html>")
re.match(r"<([a-zA-Z]*)>\w*</\1>", "<html>hh</html>")
# 因为2对<>中的数据不⼀致，所以没有匹配出来
test_label = "<html>hh</htmlbalabala>"
ret = re.match(r"<([a-zA-Z]*)>\w*</\1>", test_label)
```

## re模块的⾼级⽤法
### search,搜索匹配
re.search函数会在字符串内查找模式匹配,只要找到第⼀个匹配然后返回，如果字符串没有匹配，则返回None。
> re.search(pattern, string, flags=0)

```python
#coding=utf-8
import re
ret = re.search(r"\d+", "阅读次数为 9999")
ret.group()  # ==> '9999'
```
match()和search()的区别：
match（）函数只检测RE是不是在string的开始位置匹配，search()会扫描整个string查找匹配；
也就是说match（）只有在0位置匹配成功的话才有返回，如果不是开始位置匹配成功的话，match()就返回none。
print(re.match(‘super’, ‘superstition’).span()) 会返回(0, 5)
print(re.match(‘super’, ‘insuperable’)) 则返回None
print(re.search(‘super’, ‘superstition’).span())返回(0, 5)
print(re.search(‘super’, ‘insuperable’).span())返回(2, 7)

### findall，查找所有，返回列表
re.findall遍历匹配，可以获取字符串中所有匹配的字符串，返回⼀个列表。
> re.findall(pattern, string, flags=0)

```python
import re
ret = re.findall(r"\d+", "阅读次数:9999次,转发次数:883次,评论次数:3次")
print(ret)  # ==> ['9999', '883', '3']
```

### sub 将匹配到的数据进⾏替换
使⽤re替换string中每⼀个匹配的⼦串后返回替换后的字符串。
> re.sub(pattern, repl, string, count)

```python
import re
ret = re.sub(r"\d+", "10000", "阅读次数:9999次,转发次数:883次,评论次数:3次")
print(ret) # ==> 阅读次数:10000次,转发次数:10000次,评论次数:10000次
```

### split 根据匹配进⾏切割字符串，并返回⼀个列表
按照能够匹配的⼦串将string分割后返回列表。可以使⽤re.split来分割字符串，如：re.split(r'\s+', text)；将字符串按空格分割成⼀个单词列表。
> re.split(pattern, string[, maxsplit])

```python
import re
ret = re.split(r":| ","info:xiaoZhang 33 shandong")
print(ret) # ==> ['info', 'xiaoZhang', '33', 'shandong']
```

## python贪婪和⾮贪婪
Python⾥数量词默认是贪婪的（在少数语⾔⾥也可能是默认⾮贪婪），总是尝试匹配尽可能多的字符；
⾮贪婪则相反，总是尝试匹配尽可能少的字符。
在"*","?","+","{m,n}"后⾯加上？，使贪婪变成⾮贪婪。
```python
import re
result = re.match(r"aaa(\d+)", "aaa123456")
if result:
    print(result.group()) # ==> aaa123456
else:
    print("匹配失败～！")
```
正则表达式模式中使⽤到通配字，那它在从左到右的顺序求值时，会尽量“抓取”满⾜匹配最⻓字符串，
在我们上⾯的例⼦⾥⾯，“\d+”会从字符串的启始处抓取满⾜模式的最⻓数字字符，于是抓取到
了“aaa123456”，⽽不是只抓取“aaa1”，

解决⽅式：⾮贪婪操作符“？”，这个操作符可以⽤在"*","+","?"的后⾯，要求正则匹配的越少越好。
```python
import re
result = re.match(r"aaa(\d+?)", "aaa123456")
if result:
    print(result.group()) # ==> aaa1
else:
    print("匹配失败～！")
```

## r的作⽤
Python中在正则字符串前⾯加上 ‘r‘ 表示，让正则中的 '\' 不再具有转义功能(默认为转义)，就是表示原⽣字含义⼀个斜杠 \

```python
import re
re.match("<([a-zA-Z0-9]*)>.*</\\1>", "<html>helloworld</html>")
re.match(r"<([a-zA-Z0-9]*)>.*</\1>", "<html>helloworld</html>")
```
**说明**
与⼤多数编程语⾔相同， 正则表达式⾥使⽤"\"作为转义字符 ，这就可能造成反斜杠困扰。
假如你需要匹配⽂本中的字符"\"，那么使⽤编程语⾔表示的正则表达式⾥将需要4个反斜杠"\\"：
前两个和后两个分别⽤于在编程语⾔⾥转义成反斜杠，转换成两个反斜杠后再在正则表达式⾥转义成⼀个反斜杠。
Python⾥的原⽣字符串很好地解决了这个问题，有了原⽣字符串，你再也不⽤担⼼是不是漏写了反斜杠，写出来的表达式也更直观。