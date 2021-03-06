 **本规范糅合了本人多年的Python开发经验，不喜勿喷，谢谢！** 



# Python代码规范（适用于Python2.7和Python3.X）  

**1. 代码优雅简洁**  
**2. 注释明确优美**  
**3. 测试案例尽可能完整**


## 代码风格
### 基本的代码布局
#### 缩进
Python 严格采用4个空格的缩进, 任何 Python 代码都必须遵守此规定。


#### 最大行长度
按 PEP8 规范, Python 一般限制最大79个字符,
但是有时候写代码有些命名比较长， URL 等通常比较长,
而且21世纪都是宽屏了, 所以我们限制最大120字符（pycharm编辑器最大120字符）


#### 长语句缩进
编写长语句时, 可以使用换行符"\"换行。在这种情况下, 下一行应该与上一行的最后一个"."句点或"="对齐, 或者是缩进4个空格符。
```python
    this_is_a_very_long(function_call, 'with many parameters') \
        .that_returns_an_object_with_an_attribute

    MyModel.query.filter(MyModel.scalar > 120) \
                 .order_by(MyModel.name.desc()) \
                 .limit(10)
```
如果你使用括号"()"或花括号"{}"为长语句换行, 那么下一行应与括号或花括号对齐：
```python
    this_is_a_very_long(function_call, 'with many parameters',
                        23, 42, 'and even more')
```

对于元素众多的列表或元组, 在第一个"["或"("之后马上换行：
```python
    items = [
        'this is the first', 'set of items', 'with more items',
        'to come in this line', 'like this'
    ]

 # _Style Guidance: http://www.pocoo.org/internal/styleguide/
```

#### 空行  
函数与类之间空两行，函数和函数之间空两行，类和类之间空两行, 此外都只空一行。不要在代码中使用太多的空行来区分不同的逻辑模块。
```python
    def hello(name):
        print 'Hello %s!' % name


    def goodbye(name):
        print 'See you %s.' % name


    class MyClass(object):
        """This is a simple docstring."""

        def __init__(self, name):
            self.name = name

        def get_annoying_name(self):
            return self.name.upper() + '!!!!111'
```


### 语句和表达式

#### 一般空格规则
1. 单目运算符与运算对象之间不空格(例如, -, ~等), 即使单目运算符位于括号内部也一样。
2. 双目运算符与运算对象之间要空格。
```python
    exp = -1.05
    value = (item_value / item_count) * offset / exp
    value = my_list[index]
    value = my_dict['key']
```

#### 比较
1. 任意类型之间的比较, 使用"=="和"!="。
2. 与单例(singletons)进行比较时, 使用 is 和 is not。
3. 永远不要与True或False进行比较(例如, 不要这样写：`foo ==
   False`, 而应该这样写：`not foo`)。


#### 否定成员关系检查
使用 `foo not in bar`, 而不是 `not foo in bar`。



### 命名约定
1. 类/异常名称：采用骆驼拼写法(CamelCase), 首字母缩略词保持大写不变(HTTPWriter, 而不是 HttpWriter)，保护类加单下划线，` _class_name
`
2. 变量名：小写，必须至少要有三个字符，单词之间用下划线分隔，例如：quote_identifier，保护变量加单下划线，` _variable_name
`
3. 方法与函数名：小写，单词之间用下划线分隔，例如：quote_identifier，保护方法/函数加单下划线，` _function_name
`
4. 常量：全大写，单词之间用下划线分隔，例如：`RAW_STRING`，保护常量加单下划线，`_RAW_STRING`
5. 预编译的正则表达式：`name_re`。
6. 受保护的元素以一个下划线为前缀。
7. 不要使用Python关键字(keywords)作为类名称、变量名、方法名、函数名
8. 命名要有寓意, 不使用拼音, 不使用无意义简单字母命名 (循环中计数例外 `for i in`)
9. 命名缩写要谨慎, 尽量是大家认可的缩写


### 函数和方法的参数：
1. 类方法：cls 为第一个参数，其他参数小写，单词之间用下划线分隔，例如：quote_identifier
2. 实例方法：self 为第一个参数，其他参数小写，单词之间用下划线分隔，例如：quote_identifier
3. property函数中使用匿名函数(lambdas)时, 匿名函数的第一个参数可以用 x 替代,
   例如：`display_name = property(lambda x: x.real_name or x.username)`。




## 文档注释(Docstring, 即各方法, 类的说明注释，请用中文优雅注释)
注释如果只有一行, 代表字符串结束的三个引号与代表字符串开始的三个引号在同一行。  
注释如果为多行, 文档字符串中的文本紧接着代表字符串开始的三个引号编写, 代表字符串结束的三个引号则自己独立成一行，多行注释应该包括  
- 一行摘要，合适的话，请描述使用场景
- 参数
- 返回数据类型和语义信息，除非返回 None    
```python  
    def foo():
        """这是一个简单的 docstring."""
```
```
    def bar():
        """Train a model to classify Foos and Bars.

           Usage::

           >>> import klassify
           >>> data = [("green", "foo"), ("orange", "bar")]
           >>> classifier = klassify.train(data)

        :param train_data: A list of tuples of the form ``(color, label)``.
        :rtype: A :class:`Classifier <Classifier>`
        """  
```
文档字符串应分成简短摘要(尽量一行)和详细介绍。如果必要的话, 摘要与详细介绍之间空一行
在类的文档字符串中一般是给`__init__`方法编写文档字符串



### 模块头部
模块文件的头部包含有 utf-8 编码声明(如果模块中使用了非 ASCII 编码的字符, 一定进行声明), 使用标准的文档字符串Docstring。
```python
    # -*- coding: utf-8 -*-
    """
        package.module

        A brief description goes here.

        copyright: (c) YEAR by AUTHOR.
    """
```


### 注释(Comment)
如果使用注释来编写类属性的文档, 请在#符号后添加一个冒号":"。
(请用中文优雅注释)
```python
    class User(object):
        pass
        #: the name of the user as unicode string
        name = Column(String)
        #: the sha1 hash of the password + inline salt
        pw_hash = Column(String)
```  
  
    
### 格式化字符串
 所有的字符串格式化都需要使用.format()函数来格式化，不要使用print函数
 在格式花括号中使用索引或标识符
 ```python
  data = 'some text'
  more = '{0} and then some'.format(data)
 ```
 
  
## 模块导入  
### 导入模块胜于导入函数
使用模块导入而不是函数导入，使用函数导入容易暴露安全问题
当然有些情况一定要导入函数的，实际情况实际判断，
例如：如果第三方代码的文档中明确说明要单个引用
理由：避免循环引用
```python
# 这是推荐的
import os

def minion_path():
    path = os.path.join(self.opts['cachedir'], 'minions')
    return path
```
```python
# 这是不推荐的
from os.path import join

def minion_path():
    path = join(self.opts['cachedir'], 'minions')
    return path
```  
  
### import导入顺序  
遵循先内部后外部  
```  
import Python内置模块
...
# 空行
import 第三方库的模块
...
# 空行
import 你自己写的模块
...
```

  


## 字符编码解码 
### Python2和Python3都要三码合一，（源码文件编码，外部文件编码，字符串变量编码都要统一）  
**下面规范为了对系统的改动最少，达到最佳效果**

  
#### Python2.X  
源代码文件
```python
# 文件头一定要加上字符编码声明，特别写django项目时候，新建文件往往没有了字符编码声明
# -*- coding:utf-8 -*-
```

	
字符串变量  
- 要在源码文件头导入`from __future__ import unicode_literals`  
- `sys.getdefaultencoding()`，str类型默认是ASCII编码的字节类型  
- 统一使用unicode类型字符串变量  
- 编码时候统一使用utf-8编码和解码  
```Python  
xx.encode('utf-8')
xx.decode('utf-8')
```

外部文件  
统一使用codecs模块打开文件，并且指定编码utf-8  
```Python
import codecs
with codecs.open(r'D:\1.txt', 'r', encoding='utf-8') as fd:  
```


  
#### Python3.X    
源代码文件
```python
# 文件头一定要加上字符编码声明，特别写django项目时候，新建文件往往没有了字符编码声明
# -*- coding:utf-8 -*-
```


字符串变量  
- sys.getdefaultencoding()，str类型默认是utf-8编码的字符串类型，getdefaultencoding/setdefaultencoding在py3貌似都已经废弃
- 编码时候统一使用utf-8编码和解码 
 
```Python  
xx.encode('utf-8')

xx.decode('utf-8')  
#或
str(xx, encoding='utf-8')
```

外部文件  
open函数增加了encoding等参数，指定编码utf-8

```Python
with open(r'D:\1.txt', 'r', encoding='utf-8') as fd:  
```
  
对于Windows平台比较麻烦，Windows平台下的默认编码格式是gbk，用utf-8解码会UnicodeDecodeError  
另外，在命令行下，Windows平台的默认编码是gbk，Linux平台的默认编码是utf-8
  
  
参考：  
https://blog.csdn.net/weixin_33881140/article/details/91470873  
https://blog.csdn.net/aidanmo/article/details/86513977  
https://www.dongwm.com/post/109/
  

## Python3兼容代码
在使用Python2编写代码的时候尽量使用兼容写法，这样有利于以后将代码迁移到Python3
### 导入模块
在Python3里有名称变化的模块，导入前做判断  
configparser库
```python
try:
    # Python3
    from configparser import ConfigParser
except ImportError:
    # Python2
    from ConfigParser import ConfigParser
 
config = ConfigParser()
```  
urllib库
```python
try:
    # Python2
    from urllib import urlencode  
    from urllib import quote
    from urlparse import urlparse
    import urllib2 as request
except ImportError:
    # Python3
    from urllib.parse import urlencode  
    from urllib.parse import quote
    from urllib.parse import urlparse
    import urllib.request as request
 
# do something
```  
Queue库
```python
try:
    # Python2
    import Queue 
except ImportError:
    # Python3
    import queue
 
# do something
```  

### 使用__future__导入   
如果用Python2.7编写代码请使用 `__future__`库  
python3的print函数  
python3的int除以int得float  
python3的字符串字面量的类型为文本（python2中的unicode，python3中的str），而不是字节（python2中的str，python3中的bytes）
```python
# 导入下面四个函数
from __future__ import print_function
from __future__ import division
from __future__ import unicode_literals
from __future__ import absolute_import
```  






**代码格式化利器- Black和isort**  
不管是写单个Python文件或者写Python项目，尽量使用Black和isort格式化你的代码   
请点击下面链接查看使用详情  
[代码格式化工具Black](https://github.com/psf/black "超流行的Python代码格式化工具！")  
[import格式化工具isort](https://github.com/timothycrosley/isort)

  






  