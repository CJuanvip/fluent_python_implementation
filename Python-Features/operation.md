# Python操作类题目

### 49.Python 交换两个变量的值
```python
a, b = b, a
```
### 50.在读文件操作的时候会使用 read、readline 或者 readlines，简述它们各自的作用
* read: 读取整个文件，将文件内容放到一个字符串变量中。如果文件过大，一次读取全部内容到内存容易造成内存不足，更推荐大家使用逐行读取文件的方式。
* readline: 读取文件中的一行，包含最后的换行符“\n”，返回的是一个字符串对象，保持当前行的内存。
* readlines: 每次按行读取整个文件内容，包含最后的换行符“\n”，将读取到的内容放到一个字符串列表中，返回list类型。

```python
with open('path.txt') as file:
	file_content = file.read()
	line_content = file.readline([size]) 	# size参数可选，指定一次最多读取的字符数
	lines_content = file.readlines()
```

### 51.json 序列化时，可以处理的数据类型有哪些？如何定制支持 datetime 类型？
* 序列化：把python的对象编码转换为json格式的字符串，反序列化：把json格式字符串解码为python数据对象。
* 序列化时可以处理的数据类型：列表、元组字典、字符、数值、布尔值、None。(没有set和datetime)
* 定制datetime类型：
```python
from datetime import datetime
import json
from json import JSONEncoder

class DatetimeEncoder(JSONEncoder):
	'''扩展JSONEcoder中的default方法

	判断传入类型是否为datetime类型：
	若是则转为str字符，否则返回父类的值
	'''
	def default(self, o):
		if isinstance(o, datetime):
			return o.strftime('%Y-%m-%d %H:%M:%S')
		else:
			return super(DatetimeEncoder, self).default(o)

if __name__ == '__main__':
	dict_demo = {'name':'Renee', 'data':datetime.now()}
	print(json.dumps(dict_demo,cls=DatetimeEncoder))
```
* isinstance(object, classinfo)认为子类是一种父类类型，考虑继承关系；type(object)不会认为子类是一种父类类型，不考虑继承关系。
* super(type[, object-or-type])：调用父类的方法；
* json.dumps()：将 Python 对象编码成 JSON 字符串。


### 52.json 序列化时，默认遇到中文会转换成 unicode，如果想要保留中文怎么办？
将dumps的默认参数 ensure_ascii 设置为False。

### 53.有两个磁盘文件 A 和 B，各存放一行字母，要求把这两个文件中的信息合并(按字母顺序排列)，输出到一个新文件 C 中。
分为三步：读文件、信息合并、写文件。
```python
def read_file(file_path):
	'''读文件

	读取文件并返回文件内容
	:param file_path: 文件路径
	:return: 文件内容
	'''
	with open(file_path, 'r') as file:
		return file.read()


def write_file(file_path, file_data):
	'''写文件
	
	将数据写入指定文件中
	:param file_path: 要写入的文件路径
	:param file_data: 要写入的数据
	:return: 
	'''
	with open(file_path, 'w') as file:
		file.write(file_data)


def letter_sort(letter_str, reverse_flag=False):
	'''按字典排序

	:param letter_str: 需要排序的字母字符串
	:param reverse_flag: 排序顺序，False为正序，True为反序
	:return: 排序后的新字符串
	'''
	return ''.join(sorted(letter_str, reverse=reverse_flag))


if __name__ == '__main__':
	data1 = read_file('test1.txt')
	data2 = read_file('test2.txt')
	new_data = letter_str(data1 + data2)
	write_file('new.txt', new_data)
```

### 54.如果当前的日期为 20190530，要求写一个函数输出 N 天后的日期，(比如 N 为 2，则输出 20190601)。
```python
from datetime import datetime
from datetime import timedelta


def date_calculation(now_date, offset):
    '''获取日期

    获取几天前或几天后的日期
    :param now_date: 当前日期
    :param offset: 日期偏移量，负数为向前偏移
    :return: 偏移后的日期
    '''
    now_date = datetime.strptime(now_date, '%Y-%m-%d').date()
    offset_date = timedelta(days=offset)
    return (now_date + offset_date).strftime('%Y-%m-%d')


if __name__ == '__main__':
    print(date_calculation('2019-12-13', 7))
```

### 55.写一个函数，接收整数参数 n，返回一个函数，函数的功能是把函数的参数和 n 相乘并把结果返回。
闭包：
* 将内部函数的引用作为返回值；
* 保存运行环境，在闭包内的变量不能被轻易修改；
* 提高代码复用性。
```python
def out_func(n):
    '''闭包函数

    :param n: 整数参数n
    :return: 内层函数
    '''
    def in_func(num):
        return num * n

    return in_func


if __name__ == '__main__':
    demo = out_func(3)
    print(demo(4))
```
此例中n=3, num=4, 最后输出12.


### 56.下面代码会存在什么问题，如何改进？
```python
def strappend(num):        # 函数作用、参数意义不明，需要加注释
	str = 'frist'          # 不能使用关键字"str"作为变量名
	for i in range(num):   # 遍历得到的元素"i"意义不明，无注释
		str += str(i)      # 变量名和关键字在这个时候重名，必定报错，没有了str()方法
return str
```

修改后的代码：
```python
def str_append(num):
    '''字符串添加

    :param num: 添加数字【0到num-1】到字符串末尾
    :return: 修改后的字符串
    '''
    s = 'frist'
    # 获取数字0~num-1，添加到字符串末尾
    for i in range(num):
        s += str(i)
    return s


if __name__ == '__main__':
    print(strappend(5))
```


### 57.一行代码输出 1-100 之间的所有偶数。
```python
print([i for i in range(101) if i % 2 == 0])
```

### 58.with 语句的作用，写一段代码？
with语句：上下文管理器，用于资源访问的场合，自动在使用后进行资源的释放和异常处理。  
线程锁：
```python
import threading

num = 0			# 全局变量，多个线程可以读写，用于数据chuandi
thread_lock = threading.Lock()  # 创建一个锁


class MyThread(threading.Thread):
    """线程锁

    实现一个线程锁
    :param threading.Thread:当前线程
    :return:
    """

    def run(self):
        global num
        with thread_lock:			# 当前上下文，自动获取和释放线程资源
            for i in range(1000):  # 锁定期间，其他线程不能运行
                num += 1
        print(num)
```


### 59.python 字典和 json 字符串相互转化方法
```python
import json

# 序列化：

dict_demo = {'a': 1, 'b': 2}
# 1. 使用json.dumps()将python数据类型转化为json字符串
json_demo = json.dumps(dict_demo)
# 2. 使用json.dump()将python数据序列化后保存到指定文件
with open('demo.json') as file:
    json.dump(dict_demo, file)


# 反序列化：

# 1. 使用json.loads()将json字符类型转化为python类型
dict_demo = json.loads(json_demo)
print(dict_demo)
# 2. 使用json.load()将json字符类型从指定文件中提取出来并转化为python类型
with open('demo.json', 'r') as file:
	file_data = json.load(file)
	print(file_data)
```


### 60.请写一个 Python 逻辑，计算一个文件中的大写字母数量
```python
import re
def capital_count(file_path):
	'''计算文件中大写字母的个数
	
	读取文件，计算文件中大写字母的个数
	:param file_path:文件路径
	:return: 文件中大写字母的数量
	'''
	with open (file_path, 'r') as file:
		data = file.read()

	# 将非字母删除
	data = re.sub('^[a-zA-Z]', '', data)

	# 逐个判断是否为大写字母并计数
	cnt = 0
	for c in data:
		if c.isupper():
			cnt += 1
	return cnt


if __name__ == '__main__':
	print(capital_count('test.txt'))
```


### 61. 请写一段 Python连接 Mongo 数据库，然后的查询代码。
```python
import pymongo

# 连接本地数据库
db_client = pymongo.MongoClient('mongodb://localhost:27017/')

# 切换到testdb数据库
test_db = db_client['testdb']

# 切换到’sites‘文档
sites_obj = test_db['sites']

# find_one() 方法查询集合中的一条数据
first_data = sites_obj.find_one()
print(first_data)
```


### 62.说一说 Redis 的基本类型。
* 字符串： string
* 列表： list
* 哈希： hash
* 集合： set
* 有序集合sorted set： zset


### 63. 请写一段 Python连接 Redis 数据库的代码。
```python
import redis

# 创建连接对象
conn_obj = redis.Redis(host='localhost', port=6379, db=0)

# 设置一个键值
conn_obj.set('test', '1')

# 读取一个键值
conn_obj.get('test')
```


### 64. 请写一段 Python 连接 MySQL 数据库的代码。
游标：一条SQL语句对应一个资源集合，而游标是访问其中一行数据的接口，相当于资源集合的下标。
```python
import pymysql

# 连接数据库
db = pymysql.connect('localhost', 'testuser', 'test123', 'TESTDB', charset='utf8')

# 使用cursor()方法操作游标
cursor = db.cursor()

# 使用execute()方法执行SQL语句
cursor.execute('SELECT VERSION()')

# 使用fetclone()方法获取一条数据
data = cursor.fetclone()

# 关闭数据库连接
db.close()
```


### 65.了解 Redis 的事务么？
1. 事务的概念：事务提供了“将多个命令打包一次性提交，并按顺序执行”的能力，事务在执行过程中不会被打断，只有事务全部执行完毕才会继续执行其他命令。
2. redis事务实现：redis使用`multi，exec，discard，watch`实现事务：
* multi：开始事务；
* exec：提交并执行事务
* discard：取消事务
* watch：事务开始之前监视任意数量的键
* unwatch：取消watch命令的监视
3. redis的ACID：
* 单独的隔离操作：事务中的命令在提交后按顺序执行，不会被其他命令打断；
* 没有隔离级别的概念：队列中的命令在实际提交之前不会执行；
* 不保证原子性：事务中若有一个任务执行失败，仍会继续执行后面的任务，不会回滚。


### 66.了解数据库的三范式么？
范式是为了解决“冗余数据，插入异常，删除异常，修改异常”，低级别范式依赖于高级别范式，1NF是最低级别的范式。
* 第一范式：属性不可分
* 第二范式：每个非主属性完全依赖于键码
* 第三范式：非主属性不传递依赖于键码


### 67.了解分布式锁么？
当使用多进程或多线程的时候，多个进程或线程对同一资源进行抢占，对其读写操作可能引发数据不一致。因此，为了保证数据安全，引入了进程锁和线程锁对分布式系统共享资源的访问进行控制，通过互斥来保证一致性。  
常见的分布式锁的实现有MySQL，zookeeper，redis等。

### 68.用 Python 实现一个 Reids 的分布式锁的功能。
```python
略。
```

### 69.写一段 Python 使用 Mongo 数据库创建索引的代码。
```python
import pymongo
from pymongo import ASCENDING, DECENDING

# 连接数据库，创建连接对象
my_client = pymongo.MongoClient(mongodburl)

# 切换数据库
my_db = my_client[dbName]

# 创建索引，create_index()创建索引，可以有多个约束条件，值1为升序，-1为降序
my_db.creat_index([('date', DECENDING), ('author', ASCENDING)])
```
