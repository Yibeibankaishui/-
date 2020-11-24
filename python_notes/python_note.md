#python学习笔记

---

[toc]

---

###装饰器

[对装饰器的理解](https://foofish.net/python-decorator.html)

>装饰器本质上是一个 Python 函数或类，它可以让其他函数或类在不需要做任何代码修改的前提下增加额外功能，装饰器的返回值也是一个函数/类对象。它经常用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景，装饰器是解决这类问题的绝佳设计。有了装饰器，我们就可以抽离出大量与函数功能本身无关的雷同代码到装饰器中并继续重用。概括的讲，装饰器的作用就是为已经存在的对象添加额外的功能。

#####简单装饰器
```python
def use_logging(func):

    def wrapper():
        logging.warn("%s is running" % func.__name__)
        return func()   # 把 foo 当做参数传递进来时，执行func()就相当于执行foo()
    return wrapper

def foo():
    print('i am foo')

foo = use_logging(foo)  # 因为装饰器 use_logging(foo) 返回的时函数对象 wrapper，这条语句相当于  foo = wrapper
foo()                   # 执行foo()就相当于执行 wrapper()
```
#####@语法糖
```python
def use_logging(func):

    def wrapper():
        logging.warn("%s is running" % func.__name__)
        return func()
    return wrapper

@use_logging
def foo():
    print("i am foo")

foo()
```

---

###@staticmethod,@classmethod

[@staticmethod和@classmethod](https://zhuanlan.zhihu.com/p/28010894)
![@staticmethod和@classmethod的区别](.\1.png)

---

###pickle模块

[pickle模块(python-cookbook)](https://python3-cookbook.readthedocs.io/zh_CN/latest/c05/p21_serializing_python_objects.html)

对于序列化最普遍的做法就是使用 pickle 模块。为了将一个对象保存到一个文件中，可以这样做：

```python
import pickle

data = ... # Some Python object
f = open('somefile', 'wb')
pickle.dump(data, f)
```

为了将一个对象转储为一个字符串，可以使用 pickle.dumps() ：

```python
s = pickle.dumps(data)
```

为了从字节流中恢复一个对象，使用 pickle.load() 或 pickle.loads() 函数。比如：

```python
# Restore from a file
f = open('somefile', 'rb')
data = pickle.load(f)

# Restore from a string
data = pickle.loads(s)
```

---

###gzip模块

[gzip（官方文档）](https://docs.python.org/zh-cn/3/library/gzip.html)

此模块提供的简单接口帮助用户压缩和解压缩文件，功能类似于 GNU 应用程序 gzip 和 gunzip。

#####主要功能
```python
gzip.open(filename, mode='rb', compresslevel=9, encoding=None, errors=None, newline=None)
```
*以二进制方式或者文本方式打开一个 gzip 格式的压缩文件，返回一个 file object。*

#####用法示例

读取压缩文件示例：
```python
import gzip
with gzip.open('/home/joe/file.txt.gz', 'rb') as f:
    file_content = f.read()
```

---

###pathlib模块

[pathlib---面向对象的文件系统路径（官方文档）](https://docs.python.org/zh-cn/3/library/pathlib.html)
[pathlib介绍---比os.path更好的路径处理方式（知乎）](https://zhuanlan.zhihu.com/p/33524938)

---

###struct模块

[struct---将字节串解读为打包的二进制数据（官方文档）](https://docs.python.org/zh-cn/3/library/struct.html)
[Python中对字节流的操作:struct模块简易使用教程](https://www.jianshu.com/p/5a985f29fa81)

>此模块可以执行 Python 值和以 Python bytes 对象表示的 C 结构之间的转换。 这可以被用来处理存储在文件中或是从网络连接等其他来源获取的二进制数据。 它使用 格式字符串 作为 C 结构布局的精简描述以及与 Python 值的双向转换。


#####定义函数

```python
struct.pack(format, v1, v2, ...)
```
*返回一个 bytes 对象，其中包含根据格式字符串 format 打包的值 v1, v2, ... 参数个数必须与格式字符串所要求的值完全匹配*

```python
struct.unpack(format, buffer)
```
*根据格式字符串 format 从缓冲区 buffer 解包（假定是由 pack(format, ...) 打包）。 结果为一个元组，即使其只包含一个条目。 缓冲区的字节大小必须匹配格式所要求的大小，如 calcsize() 所示。*

```python
struct.unpack_from(format, /, buffer, offset=0)
```
*对 buffer 从位置 offset 开始根据格式字符串 format 进行解包。 结果为一个元组，即使其中只包含一个条目。 缓冲区的字节大小从位置 offset 开始必须至少为 calcsize() 显示的格式所要求的大小。*

```python
struct.calcsize(format)
```
*返回与格式字符串 format 相对应的结构的大小（亦即 pack(format, ...) 所产生的字节串对象的大小）。*


#####格式字符串

>**格式字符串**是用来在打包和解包数据时指定预期布局的机制。 它们使用指定被打包/解包数据类型的格式字符进行构建。 此外，还有一些特殊字符用来控制**字节顺序，大小和对齐方式**。

######格式字符
|格式|C类型|Python类型|标准大小|
|-|-|-|-|
|x|填充字节|无| |
|c|	char|	string of length|1|
|b|	signed char|integer|1|
|B|	unsigned char|integer|1|
|?|	_Bool|bool|1|
|h|	short|integer|2|
|H|	unsigned short|integer|2|
|i|	int|integer|4|
|I|	unsigned int|integer or long|4|
|l|	long|integer|4|
|L|	unsigned long|long|4|
|q|	long long|long|8|
|Q|	unsigned long long|long|8|
|f|	float|float|4|
|d|	double|float|8|
|s|	char[]|string|1|
|p|	char[]|string|1|
|P|	void *|long| |

>注1：q和Q只在机器支持64位操作时有意思
**注2：每个格式前可以有一个数字，表示个数**
注3：s格式表示一定长度的字符串，4s表示长度为4的字符串，但是p表示的是pascal字符串
注4：P用来转换一个指针，其长度和机器字长相关
注5：最后一个可以用来表示指针类型的，占4个字节


######字节顺序，大小和对齐方式
|字符|字节顺序|大小|对齐方式|
|-|-|-|-|
|@|按原字节|按原字节|按原字节|
|=|按原字节|标准|无|
|<|小端|标准|无|
|>|大端|标准|无|
|!|网络（=大端）|标准|无|


#####代码解析
```python
def decode_idx3_ubyte(idx3_ubyte_file):
    """
    解析idx3文件的通用函数
    :param idx3_ubyte_file: idx3文件路径
    :return: 数据集
    """
    # 读取二进制数据
    bin_data = open(idx3_ubyte_file, 'rb').read()

    # 解析文件头信息，依次为魔数、图片数量、每张图片高、每张图片宽
    offset = 0
    fmt_header = '>iiii'

    #大端对齐，头信息一共4个int

    magic_number, num_images, num_rows, num_cols = struct.unpack_from(fmt_header, bin_data, offset)
    print '魔数:%d, 图片数量: %d张, 图片大小: %d*%d' % (magic_number, num_images, num_rows, num_cols)

    # 解析数据集
    image_size = num_rows * num_cols
    offset += struct.calcsize(fmt_header)

    fmt_image = '>' + str(image_size) + 'B'

    #格式字符为 > [图片大小] B

    images = np.empty((num_images, num_rows, num_cols))
    for i in range(num_images):
        if (i + 1) % 10000 == 0:
            print '已解析 %d' % (i + 1) + '张'
        images[i] = np.array(struct.unpack_from(fmt_image, bin_data, offset)).reshape((num_rows, num_cols))
        offset += struct.calcsize(fmt_image)
    return images
```
