## Python

##### 生成式（推导式）的用法

```python
prices = {
    'AAPL': 191.88,
    'GOOG': 1186.96,
    'IBM': 149.24,
    'ORCL': 48.44,
    'ACN': 166.89,
    'FB': 208.09,
    'SYMC': 21.29
}
# 用股票价格大于100元的股票构造一个新的字典
prices2 = {key: value for key, value in prices.items() if value > 100}
print(prices2)
```

##### 后台执行

```shell
nohup python3 main.py >/dev/null 2>&1 &
nohup python3 main.py >./log/runtime_`date +%Y-%m-%d-%H-%M-%S`.out 2>&1 &
```

##### 命名规范

![python命名](C:\Users\Administrator\Desktop\文档\Python.assets\python命名.png)

**文件名**

全小写,可使用下划线

**包**

应该是简短的、小写的名字。如果下划线可以改善可读性可以加入。如mypackage。

**模块**

与包的规范同。如mymodule。

**类**

总是使用首字母大写单词串。如MyClass。内部类可以使用额外的前导下划线。

**函数&方法**

函数名应该为小写，可以用下划线风格单词以增加可读性。如：myfunction，my_example_function。

*注意*：混合大小写仅被允许用于这种风格已经占据优势的时候，以便保持向后兼容。

函数和方法的参数

总使用“self”作为实例方法的第一个参数。总使用“cls”作为类方法的第一个参数。

如果一个函数的参数名称和保留的关键字冲突，通常使用一个后缀下划线好于使用缩写或奇怪的拼写。

**全局变量**

对于from M import *导入语句，如果想阻止导入模块内的全局变量可以使用旧有的规范，在全局变量上加一个前导的下划线。

*注意*:应避免使用全局变量

**变量**

变量名全部小写，由下划线连接各个单词。如color = WHITE，this_is_a_variable = 1

*注意*：

1.不论是类成员变量还是全局变量，均不使用 m 或 g 前缀。

2.私有类成员使用单一下划线前缀标识，多定义公开成员，少定义私有成员。

3.变量名不应带有类型信息，因为Python是动态类型语言。如 iValue、names_list、dict_obj 等都是不好的命名。

**常量**

常量名所有字母大写，由下划线连接各个单词如MAX_OVERFLOW，TOTAL。

**异常**

以“Error”作为后缀。

**缩写**

命名应当尽量使用全拼写的单词，缩写的情况有如下两种：

1.常用的缩写，如XML、ID等，在命名时也应只大写首字母，如XmlParser。

2.命名中含有长单词，对某个单词进行缩写。这时应使用约定成俗的缩写方式。

例如：

function 缩写为 fn

text 缩写为 txt

object 缩写为 obj

count 缩写为 cnt

number 缩写为 num，等。

**前导后缀下划线**

一个前导下划线：表示非公有。

一个后缀下划线：避免关键字冲突。

两个前导下划线：当命名一个类属性引起名称冲突时使用。

两个前导和后缀下划线：“魔”（有特殊用图）对象或者属性，例如__init__或者__file__。绝对不要创造这样的名字，而只是使用它们。

*注意*：关于下划线的使用存在一些争议。

**特定命名方式**

主要是指 __xxx__ 形式的系统保留字命名法。

项目中也可以使用这种命名，它的意义在于这种形式的变量是只读的，这种形式的类成员函数尽量不要重载。如

class Base(object):

def __init__(self, id, parent = None):

self.__id__ = id

self.__parent__ = parent

def __message__(self, msgid):

其中 __id__、__parent__ 和 __message__ 都采用了系统保留字命名法。

附:Google Python命名规范

module_name, package_name, ClassName, method_name, ExceptionName, function_name, GLOBAL_VAR_NAME, instance_var_name, function_parameter_name, local_var_name.

![image-20220429142232028](C:\Users\Administrator\Desktop\文档\Python.assets\image-20220429142232028.png)

**字典的合并**  

info_1 = {"apple": 13, "orange": 22}
info_2 = {"爆款写作": 48, "跃迁": 49}
##### 一行代码搞定合并两个字典  
new_info = {**info_1, **info_2}
print(new_info)
输出：
{'apple': 13, 'orange': 22, '爆款写作': 48, '跃迁': 49}

##### Numpy

###### **1.1. 使用np.array创建数组**

```python
a = np.array([1,2,3,4])
#打印数组
print(a)
#查看类型
print(type(a))
```

###### **1.2. 使用np.arange创建数组**

```python
#创建0-10步数为2的数组 结果为[0,2,4,6,8]
#2. 使用np.arange创建数组
b = np.arange(0,10,2)
```

###### **1.3. np.random.random创建数组**

```python
#3. np.random.random创建一个N行N列的数组
# 其中里面的值是0-1之间的随机数
# 创建2行2列的数组
c = np.random.random((2,2))
```

###### **1.4. np.random.randint创建数组**

```python
#4. np.random.randint创建一个N行N列的数组
# 其中值的范围可以通过前面2个参数来指定
# 创建值的范围为[0,9)的4行4列数组
d = np.random.randint(0,9,size=(4,4))
```

###### **1.5. 特殊函数**

```python
#5. 特殊函数
#5.1 zeros
## N行N列的全零数组
### 例如：3行3列全零数组
array_zeros = np.zeros((3,3))
#5.2 ones
## N行N列的全一数组
### 例如：4行4列全一数组
array_ones = np.ones((4,4))
#5.3 full
## 全部为指定值的N行N列数组
### 例如：值为9的2行3列数组
array_full = np.full((2,3),9)
#5.4 eye
## 生成一个在斜方形上元素为1，其他元素都为0的N行N列矩阵
### 例如：4行4列矩阵
array_eye = np.eye(4)
```

###### **2.1 数据类型**

| 数据类型   | 描述                                                        | 唯一标识符 |
| :--------- | :---------------------------------------------------------- | :--------- |
| bool       | 用一个字节存储的布尔类型（True或False）                     | b          |
| int8       | 一个字节大小，-128 至 127                                   | i1         |
| int16      | 整数，16 位整数(-32768 ~ 32767)                             | i2         |
| int32      | 整数，32 位整数(-2147483648 ~ 2147483647)                   | i4         |
| int64      | 整数，64 位整数(-9223372036854775808 ~ 9223372036854775807) | i8         |
| uint8      | 无符号整数，0 至 255                                        | u1         |
| uint16     | 无符号整数，0 至 65535                                      | u2         |
| uint32     | 无符号整数，0 至 2 ** 32 - 1                                | u4         |
| uint64     | 无符号整数，0 至 2 ** 64 - 1                                | u8         |
| float16    | 半精度浮点数：16位，正负号1位，指数5位，精度10位            | f2         |
| float32    | 单精度浮点数：32位，正负号1位，指数8位，精度23位            | f4         |
| float64    | 单精度浮点数：64位，正负号1位，指数11位，精度52位           | f8         |
| complex64  | 复数，分别用两个32位浮点数表示实部和虚部                    | c8         |
| complex128 | 复数，分别用两个64位浮点数表示实部和虚部                    | c16        |
| object_    | python对象                                                  | O          |
| string_    | 字符串                                                      | S          |
| unicode_   | unicode类型                                                 | U          |

###### **2.2 创建数组指定数据类型**

```python
import numpy as np
a = np.array([1,2,3,4,5],dtype='i1')
a = np.array([1,2,3,4,5],dtype=int32)
```

###### **2.3 查询数据类型**

```python
class Person:
    def __init__(self,name,age):
        self.name = name
        self.age = age
d = np.array([Person('test1',18),Person('test2',20)])
print(d)
print(d.dtype)
```

###### **2.4 修改数据类型**

```python
f = a.astype('f2')
```

###### **3.1 数组维度查询**

```python
import numpy as np
# 数组维度
## 维度为1
a1 = np.array([1,2,3])
print(a1.ndim)
## 维度为2
a2 = np.array([[1,2,3],[4,5,6]])
print(a2.ndim)
## 维度为3
a3 = np.array([
    [
        [1,2,3],
        [4,5,6]
    ],
    [
        [7,8,9],
        [10,11,12]
    ]
])
print(a3.ndim)
```

###### **3.2 数组形状查询**

```python
a1 = np.array([1,2,3])
# 结果为(3,)
print(a1.shape)
a2 = np.array([[1,2,3],[4,5,6]])
# 结果为(2,3)
print(a2.shape)
a3 = np.array([
    [
        [1,2,3],
        [4,5,6]
    ],
    [
        [7,8,9],
        [10,11,12]
    ]
])
# 结果为(2,2,3)
print(a3.shape)
```

###### **3.3 修改数组形状**

```python
a1 = np.array([
    [
        [1,2,3],
        [4,5,6]
    ],
    [
        [7,8,9],
        [10,11,12]
    ]
])
a2 = a1.reshape((2,6))
print(a2)
#结果为(2, 6)
print(a2.shape)
# 扁平化 （多维数组转化为一维数组）
a3 = a2.flatten()
print(a3)
print(a3.ndim)
```

###### **3.4 数组元素个数与所占内存**

```python
a1 = np.array([
    [
        [1,2,3],
        [4,5,6]
    ],
    [
        [7,8,9],
        [10,11,12]
    ]
])
#数组的元素个数
count = a1.size
print(count)
#各元素所占内存
print(a1.itemsize)
#各元素数据类型
print(a1.dtype)
#数组所占内存
print(a1.itemsize * a1.size)
```

###### **3.5 总结**

```python
（1）一般情况下，数组维度最大到三维，一般会把三维以上的数组转化为二维数组来计算
（2）ndarray.ndmin查询数组的维度
（3）ndarray.shape可以看到数组的形状（几行几列），shape是一个元组，里面有几个元素代表是几维数组
（4）ndarray.reshape可以修改数组的形状。条件只有一个，就是修改后的形状的元素个数必须和原来的个数一致。比如原来是（2,6），那么修改完成后可以变成（3,4），但是不能变成（1,4）。reshape不会修改原来数组的形状，只会将修改后的结果返回。
（5）ndarray.size查询数组元素个数
（6）ndarray.itemsize可以看到数组中每个元素所占内存的大小，单位是字节。（1个字节=8位）
```

###### **4.2 二维数组**

```python
# 2. 多维数组
# 通过中括号来索引和切片，在中括号中使用逗号进行分割
#逗号前面的是行，逗号后面的是列，如果多维数组中只有一个值，那么这个值就是行
a2 = np.random.randint(0,10,size=(4,6))
print(a2)
#获取第0行数据
print(a2[0])
#获取第1,2行数据
print(a2[1:3])
#获取多行数据 例0,2,3行数据
print(a2[[0,2,3]])
#获取第二行第一列数据
print(a2[2,1])
#获取多个数据 例:第一行第四列、第二行第五列数据
print(a2[[1,2],[4,5]])
#获取多个数据 例:第一、二行的第四、五列的数据
print(a2[1:3,4:6])
#获取某一列数据 例:第一列的全部数据
print(a2[:,1])
#获取多列数据 例:第一、三列的全部数据
print(a2[:,[1,3]])
```

###### 5.类方法（`classmethod`）和静态方法（`staticmethod`）

类方法（Class Method）
定义
类方法使用 @classmethod 装饰器来定义。它的第一个参数是类本身（通常命名为 cls）。

特点
绑定到类：类方法绑定到类，而不是类的实例。
访问类属性和方法：类方法可以访问和修改类的状态（类属性和类方法）。
支持继承：类方法可以在子类中重载，实现多态行为。
用法
类方法通常用于需要在类级别进行操作的情况，如访问或修改类属性，或创建类的实例（工厂方法）。

```pytho
class MyClass:
    class_variable = 'Hello, World!'

    @classmethod
    def get_class_variable(cls):
        return cls.class_variable

# 调用类方法
print(MyClass.get_class_variable())  # 输出：Hello, World!

# 通过实例调用
instance = MyClass()
print(instance.get_class_variable())  # 输出：Hello, World!

```

```python
from abc import ABC, abstractmethod

# 基类
class Product(ABC):
    @abstractmethod
    def operation(self):
        pass

# 具体产品A
class ConcreteProductA(Product):
    def operation(self):
        return "Result of ConcreteProductA"

# 具体产品B
class ConcreteProductB(Product):
    def operation(self):
        return "Result of ConcreteProductB"

# 工厂类
class ProductFactory:
    @classmethod
    def create_product(cls, product_type: str) -> Product:
        if product_type == 'A':
            return ConcreteProductA()
        elif product_type == 'B':
            return ConcreteProductB()
        else:
            raise ValueError(f"Unknown product type: {product_type}")

# 使用工厂方法创建产品实例
if __name__ == "__main__":
    product_a = ProductFactory.create_product('A')
    product_b = ProductFactory.create_product('B')

    print(product_a.operation())  # Output: Result of ConcreteProductA
    print(product_b.operation())  # Output: Result of ConcreteProductB

```



静态方法（Static Method）

静态方法使用 @staticmethod 装饰器来定义。它不接收特殊的第一个参数（既不接收 self 也不接收 cls）。

特点
绑定到类：静态方法绑定到类，而不是类的实例。
不访问类或实例的状态：静态方法不能访问或修改类或实例的状态。
独立性强：静态方法独立于类和实例，通常用于逻辑上属于类但不依赖于类或实例状态的方法。
用法:
**静态方法通常用于工具函数或逻辑上属于类但不依赖于类或实例状态的方法。**

```
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b

# 调用静态方法
result = MathUtils.add(5, 7)
print(result)  # 输出：12

```

| 特点       | 类方法（Class Method）                 | 静态方法（Static Method）                        |
| ---------- | -------------------------------------- | ------------------------------------------------ |
| 装饰器     | @classmethod                           | @staticmethod                                    |
| 第一个参数 | 类本身（`cls`）                        | 无特殊参数                                       |
| 访问权限   | 可以访问和修改类属性和方法             | 不能访问类或实例属性和方法                       |
| 绑定       | 绑定到类                               | 绑定到类                                         |
| 用途       | 需要在类级别进行操作的场景，如工厂方法 | 工具函数或逻辑上属于类但不依赖类或实例状态的方法 |

用法上的不同点
访问和修改类状态：

类方法可以访问和修改类属性和方法。
静态方法不能访问类或实例属性和方法，只能访问其自身的参数和局部变量。
实例化：

类方法常用作工厂方法来创建类实例。
静态方法一般用于逻辑上属于类的工具函数，不涉及类的实例化。
继承：

类方法支持继承，并可以在子类中重载，实现多态。
静态方法不支持继承特性，通常用于不需要依赖于类的继承结构的功能。
通过上述解释和示例，希望能帮助你理解类方法和静态方法的区别及其用法。

##### **httpx**

httpx是一个异步协程http请求

##### **dataclass**

Python 3.7 中的`dataclasses`模块是一种更有效的方法来存储将在程序的不同部分之间传递的数据

```python
from dataclasses import dataclass

@dataclass
class Employee:
    id_: int
    name: str
    salary: float

    def __repr__(self):
       return f"{self.name} earns ${self.salary}."

e1 = Employee(id_=1, name='Tusk', salary=69999.99)
print(e1) # Tusk earns $69999.99.
```

#####  asyncio和aiohttp

`asyncio` 协程主要使用 `async` 和 `await` 关键字进行定义和调用：

- `async def`：定义一个异步函数（协程）。
- `await`：在协程中等待一个可等待对象（如另一个协程、Future、Task 等）的完成。

**技巧和注意事项：**

- **事件循环（Event Loop）：** 协程需要运行在事件循环中，`asyncio.run()` 会自动创建一个事件循环并运行。
- **任务（Task）：** 可以使用 `asyncio.create_task()` 将协程封装成任务，以便更灵活地控制协程的执行。
- **Future：** Future 对象表示一个异步操作的最终结果。
- **异步 I/O：** 协程主要用于处理 I/O 密集型任务，如网络请求、文件读写等。使用异步 I/O 可以避免阻塞，提高程序的并发性能。
- **避免阻塞操作：** 在协程中应避免执行阻塞操作（如 time.sleep()），否则会阻塞整个事件循环。应使用 `asyncio.sleep()` 或其他异步 I/O 操作。
- **上下文管理：** 可以使用 `async with` 语句进行异步上下文管理，例如异步文件操作、异步锁等。

**协程比多线程快的原因：**

协程的“快”主要体现在以下几个方面：

1. **切换开销小：** 协程的切换由程序自身控制，仅涉及少量寄存器和栈信息的保存和恢复，开销远小于线程切换（需要操作系统内核介入）。
2. **避免了锁竞争：** 由于协程是单线程的，不存在多个线程同时访问共享资源的情况，因此避免了锁的竞争和死锁等问题，简化了并发编程的复杂性。
3. **更高效的 I/O 操作：** 协程结合异步 I/O，可以在 I/O 等待期间切换到其他任务执行，最大限度地利用 CPU 资源，提高了 I/O 密集型任务的效率。

###### 1.在windows上使用时，有时会报错遇到`RuntimeError: Event loop is closed`的问题

```
将asyncio.run(main())改为：
loop = asyncio.get_event_loop()
loop.run_until_complete(main())
原因是： 
asyncio 对 Windows 的支持本来就不好。Python3.8 后默认 Windows 系统上的事件循环采用 ProactorEventLoop （仅用于 Windows ）这篇文档描述了其在 Windows 下的缺陷：https://docs.python.org/zh-cn/3/library/asyncio-platforms.html#windows

参考：https://www.cnblogs.com/james-wangx/p/16111485.html
```

###### 2.性能测试

```python
# -*- encoding: utf-8 -*-
"""
@file    :   asyn_aiohttp_demo.py  
@contact :   wuwenliang@wxchina.com
@license :   (c)copyright 2018-2023
 
@modify time      @author    @version    @description
------------      -------    --------    -----------
2024/12/17 15:46   wwl        1.0         asyn_aiohttp_demo
"""
import asyncio
import time

import aiohttp
import requests


# 同步请求函数
def fetch2(url):
    response = requests.get(url)
    return response.text


# 主函数，执行同步请求
def main2():
    urls = ["https://jsonplaceholder.typicode.com/posts"] * 100  # 请求的URL

    start_time = time.time()  # 记录开始时间
    results = [fetch2(url) for url in urls]  # 创建任务列表并同步执行
    end_time = time.time()  # 记录结束时间

    print(f"同步请求的总时间: {end_time - start_time}秒")
    return results


async def fetch(url, semaphore):

    async with semaphore:

        async with aiohttp.ClientSession() as session:

            async with session.get(url) as response:

                return await response.text()


async def main():

    urls = ["https://jsonplaceholder.typicode.com/posts"] * 100

    semaphore = asyncio.Semaphore(100)

    tasks = [fetch(url, semaphore) for url in urls]

    start_time = time.time()  # 记录开始时间
    results = await asyncio.gather(*tasks)
    end_time = time.time()  # 记录结束时间

    print(f"100个并发请求的总时间: {end_time - start_time}秒")

    # print(results)


if __name__ == "__main__":

    # 协程测试
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main())

    # 同步测试
    # main2()
```

结果：

测试请求100条

使用协程，Semaphore=100，耗时：1.899681568145752秒-2秒左右

使用协程，Semaphore=10，耗时：5.349802017211914秒

使用协程，Semaphore=1，耗时：48.74130725860596秒

使用多线程，workers=100，耗时 1.5306212902069092秒

使用多线程，workers=10，耗时 3.461984157562256秒

使用多线程，workers=1，耗时 32.65285873413086秒

测试请求10000条

使用协程，Semaphore=100，耗时：99.30082130432129秒左右，大概100/S，和Semaphore一致

使用多线程，workers=100，报错

###### 3.asyncio.run()和lock

当代码中使用了`lock = asyncio.Lock()`创建锁，并且使用`asyncio.run()`运行代码，有可能会出现`Task <Task pending ...> got Future <Future pending> attached to a different loop Task was destroyed but it is pending!`的报错。

可能的原因是：`syncio.Lock`与`asyncio.run`之间的事件循环可能不匹配，通常会在某些环境中（如特定的 IDE 或脚本运行环境）出现问题。原因在于`asyncio.run` 创建并管理一个新的事件循环，而锁 (`asyncio.Lock`) 可能会被不同的事件循环使用，从而导致不一致。为避免这种情况，可以显式创建并使用一个事件循环:

```python
loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```

###### 4.asyncio.to_thread()

`asyncio.to_thread()` 是 Python 3.9 引入的一个函数，它的主要作用是在单独的线程中运行一个函数，并将其结果作为一个协程返回。这使得我们可以在 `asyncio` 的事件循环中安全地调用阻塞的同步函数，而不会阻塞事件循环。

**为什么需要 `asyncio.to_thread()`？**

在异步编程中，我们应该尽量避免执行阻塞操作，因为它们会阻塞事件循环，导致整个程序停顿。然而，在实际开发中，我们可能会遇到一些无法避免的阻塞操作，例如：

- 调用一些没有提供异步接口的第三方库。
- 执行一些 CPU 密集型计算。
- 进行文件 I/O 操作（虽然 `aiofiles` 等库提供了异步文件操作，但在某些情况下可能不适用）。

在这种情况下，`asyncio.to_thread()` 就派上了用场。它可以将这些阻塞操作放到单独的线程中执行，从而避免阻塞事件循环。

**`asyncio.to_thread()` 的用法**

`asyncio.to_thread()` 的基本用法如下：

```python
import asyncio

async def main():
    def blocking_io():
        print(f"start blocking_io at {asyncio.get_running_loop()}")
        # 模拟阻塞操作
        import time
        time.sleep(1)
        print(f"blocking_io complete at {asyncio.get_running_loop()}")
        return "Blocking result"

    print(f"start main at {asyncio.get_running_loop()}")
    result = await asyncio.to_thread(blocking_io)
    print(f"end main with result: {result} at {asyncio.get_running_loop()}")

asyncio.run(main())
```

**注意事项**

- **GIL 的影响：** 由于 Python 的全局解释器锁（GIL）的存在，`asyncio.to_thread()` 对于 CPU 密集型任务的性能提升有限。对于 CPU 密集型任务，应该使用 `concurrent.futures.ProcessPoolExecutor()` 来利用多核 CPU。但是对于 I/O 密集型任务，`asyncio.to_thread()` 仍然非常有效。
- **线程安全：** 传递给 `asyncio.to_thread()` 的函数应该是线程安全的。如果函数访问了共享的可变数据，需要使用适当的同步机制（如锁）来保护数据。
- **异常处理：** 如果在线程中执行的函数抛出异常，`asyncio.to_thread()` 返回的协程也会抛出相同的异常。可以使用 `try...except` 语句来捕获异常。

**与 `loop.run_in_executor()` 的比较**

在 Python 3.9 之前，通常使用 `loop.run_in_executor()` 来在单独的线程或进程中运行函数。`asyncio.to_thread()` 可以看作是 `loop.run_in_executor()` 的一个简化版本，它默认使用线程池执行器，使用起来更加方便。

##### aiolimite

`AsyncLimiter` 是一个实现了 **漏桶算法（Leaky Bucket Algorithm）** 的速率限制器，允许我们控制并发请求的速率。在异步编程中，`AsyncLimiter` 可以用作异步上下文管理器（`async with`），用于限制在一定时间内进行的操作次数。例如，限制每分钟最多执行 10 次任务，或者每秒最多执行一定数量的请求。

````
GTP源码介绍：
https://chatgpt.com/share/676532aa-9794-8004-b724-358e921655e6

顺便一提：
源码中的__aenter__：
在 Python 中，__aenter__ 和 __aexit__ 是异步上下文管理器的一部分，它们用于定义一个对象的异步上下文管理协议。这些方法通常在 async with 语句中使用。
__aenter__ 方法在进入 async with 语句的上下文管理器时调用。这通常用于设置资源，例如打开文件，或者建立网络连接。
__aexit__ 方法在离开 async with 语句的上下文管理器时调用。这通常用于清理资源，例如关闭文件，或者断开网络连接。
````

使用`aiolimite`的原因是替换`asyncio`中的`semaphore`，原因是使用了`semaphore`后，发现调用百度API的QPS远超90，后面才发现是混淆了概念，semaphore是表示协程的并发数，和QPS的概率不同，并不是一秒内的并行数量，使用`aiolimite`可以设置周期内的并发数，适合用于调用有QPS限制的API，使用方式也很简单，定义`limiter = AsyncLimiter(90, 1)`,对需要显示的代码使用with管理，`async with self.limiter`



### conda环境搭建

```
# 下载
wget https://repo.anaconda.com/archive/Anaconda3-2023.03-1-Linux-x86_64.sh

1、输入命令：bash Anaconda-3-5.3.1-Linux-x86_64.sh
2、回车
3、输入：yes
4、选择安装路径，可以修改安装路径
5、输入：yes
# 提示“Thank you for installing Anaconda3!”视为安装成功

# 文件配置

1、打开配置文件：
    vim /etc/profile
2、 在文件的最后加上如下配置
    export ANACONDA_HOME=/home/data/xw_bigdata/anaconda3           # 步骤2.4 中的安装路径
    export PATH=$ANACONDA_HOME/bin:$PATH
    export PYSPARK_PYTHON=$ANACONDA_HOME/bin/python            # 可不添加
3、source /etc/profile     # 使文件修改生效

#添加数据源：例如, 添加清华anaconda镜像：
conda config --add channels https://mirrors.aliyun.com/conda/pkgs/free/
conda config --add channels https://mirrors.aliyun.com/conda/pkgs/main/
conda config --add channels https://mirrors.aliyun.com/conda/pkgs/r/

#移除
conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/

# 也可以在/root/.condarc修改
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  deepmodeling: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/
  
# 其他的可以参考：
https://zhuanlan.zhihu.com/p/584580420
```

##### 命令

```
1. conda --version #查看conda版本，验证是否安装
2. conda update conda #更新至最新版本，也会更新其它相关包
conda update -n base conda #update最新版本的conda
3. conda update --all #更新所有包
4. conda update package_name #更新指定的包
5. conda create -n env_name package_name #创建名为env_name的新环境，并在该环境下安装名为package_name 的包，可以指定新环境的版本号，
例如：conda create -n test_env python=3.10 numpy pandas，创建了python2环境，python版本为2.7，同时还安装了numpy pandas包
6. conda activate env_name #切换至env_name环境
7. conda deactivate #退出环境
8. conda info -e #显示所有已经创建的环境
或 conda env list
或 conda info --envs
9. conda create --name new_env_name --clone old_env_name #复制old_env_name为new_env_name
10. conda remove --name env_name –all #删除环境
11. conda list # 查看所有已经安装的包
12. conda install package_name #在当前环境中安装包
13. conda install --name env_name package_name #在指定环境中安装包
14. conda remove -- name env_name package #删除指定环境中的包
15. conda remove package #删除当前环境中的包
16. conda env remove -n env_name #采用第10条的方法删除环境失败时，可采用这种方法


17. 以下两个命令必须在 base 环境下进行操作，否则会报错
conda create --name newname --clone oldname      # 克隆oldname 环境为newname 环境
conda remove --name oldname --all       # 彻底删除旧环境
```

**conda 自动开启/关闭激活**

```
conda config --set auto_activate_base false  #关闭自动激活状态
conda config --set auto_activate_base true  #关闭自动激活状态
```

**conda瘦身**

```
 conda clean 可以轻松搞定
conda clean -p        # 删除没有用的包
conda clean -r        # 删除 tar 包
conda clean -y --all    # 删除所有的安装包及cache
```

**如何使用回服务器的python**

```
vi /etc/profile
把export PATH=$ANACONDA_HOME/bin:$PATH
修改为：
export PATH=$PATH:$ANACONDA_HOME/bin
source /etc/profile

vi ~/.bashrc
修改PATH为export PATH="$PATH:/home/data/anaconda3/bin"
source ~/.bashrc

查看：
echo $PATH
WHICH python3
```


**重新部署**

```

1.conda create -n store-server python=3.11
报错：
Retrieving notices: ...working... done
Collecting package metadata (current_repodata.json): failed

UnavailableInvalidChannel: HTTP 404 NOT FOUND for channel pkgs/r <https://mirrors.aliyun.com/conda/pkgs/r>

The channel is not accessible or is invalid.

You will need to adjust your conda configuration to proceed.
Use `conda config --show channels` to view your configuration's current state,
and use `conda config --show-sources` to view config file locations.

conda config --show-sources
==> /root/.condarc <==
auto_activate_base: False
channels:
  - https://mirrors.aliyun.com/conda/pkgs/r/
  - https://mirrors.aliyun.com/conda/pkgs/main/
  - https://mirrors.aliyun.com/conda/pkgs/free/
  - conda-forge
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - defaults

# 移除

conda config --remove channels https://mirrors.aliyun.com/conda/pkgs/r/
conda config --remove channels https://mirrors.aliyun.com/conda/pkgs/main/
conda config --remove channels https://mirrors.aliyun.com/conda/pkgs/free/

# 添加
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/pro/

# 重新
conda create -n store-server python=3.11
conda activate store-server
pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple/


```

## pydantic2.7

Pydantic 是一个用于数据验证和解析的 Python 库，它通过声明式的方式定义数据模型，并提供了自动生成文档、验证数据等功能。

#### 基础用法

```python
from pydantic import BaseModel


class User(BaseModel):
    id: int
    name: str = 'Jane Doe'
    
user = User(id='123')


assert user.model_dump() == {'id': 123, 'name': 'Jane Doe'}
assert user.model_fields_set == {'id'}
assert user.name == 'Jane Doe'
```

#### orm模型

使用model_validate方法将sqlalchemy-orm转为pydantic模型

model_validate(): 它接受一个dict或一个对象而不是关键字参数。如果传递的对象不能被验证，或者它不是一个字典或模型的实例，那么将引发一个 ValidationError 。

model_validate_json():接受一个str或bytes并将其解析为json，然后将结果传递给 `model_validate()` 。

```python
from typing import List

from sqlalchemy import Column, Integer, String
from sqlalchemy.dialects.postgresql import ARRAY
from sqlalchemy.orm import declarative_base
from typing_extensions import Annotated

from pydantic import BaseModel, ConfigDict, StringConstraints

Base = declarative_base()


class CompanyOrm(Base):
    __tablename__ = 'companies'

    id = Column(Integer, primary_key=True, nullable=False)
    public_key = Column(String(20), index=True, nullable=False, unique=True)
    name = Column(String(63), unique=True)
    domains = Column(ARRAY(String(255)))


class CompanyModel(BaseModel):
    model_config = ConfigDict(from_attributes=True)

    id: int
    public_key: Annotated[str, StringConstraints(max_length=20)]
    name: Annotated[str, StringConstraints(max_length=63)]
    domains: List[Annotated[str, StringConstraints(max_length=255)]]


co_orm = CompanyOrm(
    id=123,
    public_key='foobar',
    name='Testing',
    domains=['example.com', 'foobar.com'],
)
print(co_orm)
#> <__main__.CompanyOrm object at 0x0123456789ab>
co_model = CompanyModel.model_validate(co_orm)
print(co_model)
"""
id=123 public_key='foobar' name='Testing' domains=['example.com', 'foobar.com']
"""

# model_validate_json()
class User(BaseModel):
    id: int
    name: str = 'John Doe'
    signup_ts: Optional[datetime] = None


m = User.model_validate({'id': 123, 'name': 'James'})
print(m)
#> id=123 name='James' signup_ts=None

try:
    User.model_validate(['not', 'a', 'dict'])
except ValidationError as e:
    print(e)
    """
    1 validation error for User
      Input should be a valid dictionary or instance of User [type=model_type, input_value=['not', 'a', 'dict'], input_type=list]
    """

m = User.model_validate_json('{"id": 123, "name": "James"}')
print(m)
```



```python
from pydantic import BaseModel, ConfigDict


class User(BaseModel):
    model_config = ConfigDict(extra='ignore')  

    name: str


user = User(name='John Doe', age=20)  
print(user)
#> name='John Doe'
```

## Transformers

Python的`transformers`库是由Hugging Face开发的开源工具，旨在简化自然语言处理（NLP）任务的实现。

### **1. 简介与背景**

- **背景**：Hugging Face的`transformers`库集成了多种预训练模型（如BERT、GPT、T5、RoBERTa等），支持PyTorch、TensorFlow和JAX框架，广泛应用于文本分类、生成、翻译等任务。
- **优势**：提供统一的API，用户无需从头训练模型，可直接使用或微调预训练模型，节省资源。

### **2. 安装与基本用法**

- **安装**：

  ```bash
  pip install transformers
  ```

- **快速示例（文本分类）**：

  ```python
  from transformers import pipeline
  
  # 使用预构建的pipeline
  classifier = pipeline("sentiment-analysis")
  result = classifier("I love using transformers!")
  print(result)  # 输出：[{'label': 'POSITIVE', 'score': 0.9998}]
  ```

### **3. 核心组件**

- **分词器（Tokenizer）**：

  - 将文本转换为模型所需的输入格式（如token ID、attention mask）。

  - 示例：

    ```python
    from transformers import AutoTokenizer
    
    tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")
    inputs = tokenizer("Hello, my dog is cute", return_tensors="pt")  # 返回PyTorch张量
    ```

- **模型（Model）**：

  - 加载预训练模型，支持多种架构（如`BertModel`, `GPT2LMHeadModel`）。

  - 示例：

    ```python
    from transformers import AutoModel
    
    model = AutoModel.from_pretrained("bert-base-uncased")
    outputs = model(**inputs)  # 前向传播
    ```

- **配置（Config）**：

  - 控制模型结构参数（如层数、注意力头数）。

  - 示例：

    ```python
    from transformers import BertConfig
    
    config = BertConfig.from_pretrained("bert-base-uncased")
    model = BertModel(config)  # 根据配置初始化模型
    ```

### **4. 使用示例**

#### **文本生成（GPT-2）**

```python
from transformers import GPT2LMHeadModel, GPT2Tokenizer

tokenizer = GPT2Tokenizer.from_pretrained("gpt2")
model = GPT2LMHeadModel.from_pretrained("gpt2")

inputs = tokenizer("Once upon a time,", return_tensors="pt")
outputs = model.generate(**inputs, max_length=50)
print(tokenizer.decode(outputs[0]))
```

#### **微调模型（以文本分类为例）**

```python
from transformers import Trainer, TrainingArguments, AutoModelForSequenceClassification
from datasets import load_dataset

dataset = load_dataset("imdb")  # 加载数据集
model = AutoModelForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=2)

# 定义训练参数
training_args = TrainingArguments(
    output_dir="./results",
    num_train_epochs=3,
    per_device_train_batch_size=16,
)

# 使用Trainer简化训练
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=dataset["train"],
    eval_dataset=dataset["test"],
)
trainer.train()
```

### **5. 常见任务与应用场景**

- **文本分类**：情感分析、垃圾邮件检测。
- **文本生成**：故事创作、代码补全（如CodeGen）。
- **问答系统**：基于BERT的抽取式问答。
- **翻译与摘要**：使用T5或BART模型。
- **命名实体识别（NER）**：标记文本中的实体。

### **6. 高级功能与技巧**

- **模型压缩**：使用`bitsandbytes`进行8位量化，减少内存占用。

- **分布式训练**：利用`accelerate`库实现多GPU/TPU训练。

- 自定义模型

  ：添加任务特定层（如分类头）并微调。

  ```python
  from transformers import BertForSequenceClassification
  
  model = BertForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=5)
  ```

### **7. 资源与社区**

- **Hugging Face Hub**：托管数千个预训练模型（[链接](https://huggingface.co/models)）。
- **文档与教程**：详细指南和示例（[文档](https://huggingface.co/docs/transformers/)）。
- **社区支持**：活跃的论坛和GitHub仓库，便于问题解答。

------

### **8. 注意事项**

- **输入长度限制**：如BERT最多处理512个token，超长文本需截断或分段。
- **硬件要求**：大型模型（如GPT-3）需高显存，可选用小规模变体（如`distilbert`）。
- **分词器匹配**：确保分词器与模型匹配（如BERT分词器不可用于GPT-2）。