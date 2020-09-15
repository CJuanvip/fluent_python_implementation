# Python编码规范

### 7.什么是 PEP8?
> [PEP8官网](https://www.python.org/dev/peps/pep-0008/)

PEP【Python Enhancement Proposal】，即Python增强建议书，是一种代码规范，旨在增加代码可读性。

### 8.了解 Python 之禅么？
python的设计哲学：简洁、优雅。  `import this`:
```
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```
>优美胜于丑陋；  
明了胜于晦涩；  
简洁胜于复杂；  
复杂胜于凌乱；  
扁平胜于嵌套；  
间隔胜于紧凑；  
可读性很重要；  
为了特例的实用性也不可打破这些规则；  
除非确实需要这样做，否则不要忽视错误；  
当出现多种可能不要尝试去猜测，而是尽量取唯一一种明显的方法，可能这种方法一开始不那么明显但谁让你不是Python之父呢；  
做好过不做，但不假思索地就去做也许还不如不做；  
如果你无法向人描述你的方案，那肯定不是一个好方案；如果可以，那也许是个好方案；  
命名空间是种绝妙的理念，让我们多多使用它吧！  


### 9.了解 docstring 么？
docstring：文档字符串。用于解释程序，是模块、类、方法中的第一个声明性描述说明。由一对三重双引号引起来，如果其中有斜杠，引号前面加r；如果使用统一编码，引号前面加u；如果有多行，引号分别自成一行。

### 10.了解类型注解么？
Python是动态语言，变量和函数参数不区分类型，代码编写速度快。但阅读别人的代码或者回头看自己以前的代码可能需要一定时间理解这些变量返回值到底是什么类型。因此python3中添加了“类型注解”特性，可以给函数参数、返回值和变量加上注解，提高代码可读性。

### 11.例举你知道 Python 对象的命名规范，例如方法或者类等。
	• 变量：小写加下划线。
		○ 单下划线开头是私有变量，程序员之间约定不要在外部访问它，但实际上还是可以访问到的；
		○ 双下划线开头的变量是私有类型（private）的变量，只允许类内部访问，子类也不能访问；
		○ 双下划线开头和结尾的是内置变量，可以直接访问，如```__init__```,```__file__```,```__import__```等；
		○ 单下划线结尾的变量是为了和关键字区分，如```class_```。
	• 全局变量和常量：全大写加下划线。
	• 函数参数：小写加下划线。避免滥用```*args```和```*kwargs```，可能破坏函数健壮性。
	• 函数，方法：小写加下划线。
		○ 私有方法：单下划线开头；
		○ 特殊方法（如运算符重载等）：双下划线开头；
	• 类：驼峰命名。基类前可加‘Base’或‘Abstract’。异常也是类，异常名为驼峰命名后加一个‘Error’。
	• 模块和包：模块尽量使用简短全小写，为了提高可读性也可用下划线；包使用全小写，不推荐加下划线。

	
### 12.Python 中的注释有几种？
单行注释：使用#开始。  
多行注释：使用三重单引号或双引号包含，注释中若需使用引号应该与外部不同的引号。

### 13.如何优雅的给一个函数加注释？
使用docstring。

### 14.如何给变量加注释？
在代码后至少两个空格，使用单行注释#后一个空格再跟上注释语句；可以将注释对其但应注意一行最好不超过79个字符（PEP8）。

### 15.Python 代码缩进中是否支持 Tab 键和空格混用。
不可以混用！  
不同解释器对tab的解释是不同的，有的4字符宽有的8字符宽，如果混用的话就可能出现缩进错误。虽然使用空格会麻烦一丢丢，但是使用4个空格替代tab键会更好，有的编译器可以设置tab自动转空格可以试试。

### 16.是否可以在一句 import 中导入多个库?
	• 可以，但不推荐。每行import中只导入一个库，更易于阅读、编辑、搜索、维护。
	• 导入顺序：
		○ 标准库
		○ 第三方库
		○ 自定义模块
	• 不建议from matplotlib import *：
		○ 占用更多内存空间，不必要的方法会被初始化；
		○ 代码可读性差，模块内突然会冒出来一个没有归属的方法，不知道从哪个库引入的。

### 17.在给 Py 文件命名的时候需要注意什么?
	• 不与关键字重名；
	• 不与标准库或第三方库重名，除非想重写；
	• 不用中文命名任何路径和可执行文件；
	• 不用字母数字下划线以外的字符，不用数字开头。
	
### 18.例举几个规范 Python 代码风格的工具
	• pylint
	• black
	• autopep8
	• flake8
