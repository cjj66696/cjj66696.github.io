# PEP 8 -- Style Guide for Python Code

PEP8 -- Python代码样式指南（中文版）翻译自 Python官网

---

Contents

[TOC]

---

## 1. Introduction

本文档给出了包含主要Python发行版中的标准库的Python代码的编码约定。请参阅Python[1]的C实现中C代码的伴随信息PEP描述样式指南。

本文和PEP 257 (Docstring约定)是从Guido的原始Python风格指南文章改编而来的，还有一些来自Barry的风格指南[2]。

随着时间的推移，随着更多的约定被识别，过去的约定被语言本身的变化所淘汰，这种风格指南也在不断发展。

---

## 2. Code Lay-out

### 2.1 Indentation

每个缩进需要使用4个空格。

延续行应该使用Python的隐式行在括号、括号和大括号内进行垂直对齐，或者使用悬挂缩进[7]。使用悬挂缩进时，应考虑以下因素：第一行不应该有任何参数，进一步的缩进应该被用来清楚地区分自己作为一个延续行。

Yes:

```python
# Aligned with opening delimiter.
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# More indentation included to distinguish this from the rest.

def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

# Hanging indents should add a level.
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```

No:
```python
# Arguments on first line forbidden when not using vertical alignment.
foo = long_function_name(var_one, var_two,
    var_three, var_four)

# Further indentation required as indentation is not distinguishable.
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print(var_one)
```
对于延续行，4个空格的规则是可选使用的。
```python
Optional:

# Hanging indents *may* be indented to other than 4 spaces.
foo = long_function_name(
  var_one, var_two,
  var_three, var_four
```
当条件一个if语句的一部分足够长要求编写跨多个行,值得注意的是,两个字符的组合关键字(即如果),加上一个空格,加上开括号创建一个自然4空间缩进的后续行多行条件。这可能会与嵌套在if-语句中的缩进代码集产生视觉冲突，后者也会自然缩进到4个空格中。对于如何(或是否)进一步在视觉上区分这些条件行和ne没有明确的立场。
```python
# No extra indentation.
if (this_is_one_thing and
    that_is_another_thing):
    do_something()

# Add a comment, which will provide some distinction in editors
# supporting syntax highlighting.
if (this_is_one_thing and
    that_is_another_thing):
    # Since both conditions are true, we can frobnicate.
    do_something()

# Add some extra indentation on the conditional continuation line.
if (this_is_one_thing
        and that_is_another_thing):
    do_something()
```
(也请参阅下面关于在二进制运算符之前或之后中断的讨论。)

多行结构上的右括号/括号/括号可以排列在列表最后一行的第一个非空格字符下，如：
```python
my_list = [
    1, 2, 3,
    4, 5, 6,
    ]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
    )
```
或者，它可能排在开始多行构造的第一行的第一个字符下面，如：
```python
my_list = [
    1, 2, 3,
    4, 5, 6,
]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
)
```
### 2.2 Tabs or Spaces?

空格是首选的缩进方法。

Tabs应该仅用于与已经用tabs缩进的代码保持一致。

Python 3不允许混合使用制表符和空格来缩进。

python 2使用tabs和空格的组合缩进方式应该转换为专门使用空格缩进。

### 2.3 Maximum Line Length

将所有行限制为最多79个字符。

对于具有较少结构限制的长文本块(文档字符串或注释)，行长度应该限制为72个字符。

限制所需的编辑器窗口宽度可以让多个文件并排打开，并且在使用在相邻列中显示两个版本的代码审查工具时工作得很好。

大多数工具中的默认包装破坏了代码的视觉结构，使其更难理解。选择这些限制是为了避免在窗口宽度设置为80的编辑器中换行，即使工具在换行时在最后一列中放置了标记符号。一些基于web的工具可能根本不提供动态换行。

有些队强烈喜欢较长的线路长度。对于专门或主要由一个团队维护的、能够就这个问题达成一致的代码，可以将标称行长度从80个字符增加到100个字符(有效地将最大长度增加到99个字符)，只要注释和文档字符串仍然包装在72个字符。

Python标准库是保守的，要求限制行为79个字符(文档字符串/注释为72个)。

最好的换行方式是在括号、括号和大括号中使用Python的隐含的行延续。通过在括号中包装表达式，可以在多行中打断长行。与使用反斜杠进行行延续相比，应该优先使用它们。

反斜杠有时仍然是合适的。例如，长而多的with-statement不能使用隐式延续，因此可以接受反斜杠

```python
with open('/path/to/some/file/you/want/to/read') as file_1, \
     open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())
```

(请参阅前面关于多行if-statements的讨论，以进一步了解此类多行if-statements的缩进。)
另一种情况是断言语句。

确保适当缩进连续行。

### 2.4 Should a Line Break Before or After a Binary Operator?
几十年来，推荐的样式是在二进制运算符之后换行。但这可能在两方面破坏代码的可读性:操作符往往分散在屏幕上的不同列上，每个操作符都在其操作数的上一行。因此，眼睛必须分辨哪些操作数被加，哪些被减：

```python
# No: operators sit far away from their operands
income = (gross_wages +
          taxable_interest +
          (dividends - qualified_dividends) -
          ira_deduction -
          student_loan_interest)
```

为了解决这个可读性问题，数学家和他们的出版商遵循了相反的惯例。Donald Knuth解释了他的计算机和排版系列中的传统规则:“虽然一个段落中的公式总是在二进制操作和关系之后换行，但是显示的公式总是在二进制操作之前进行换行”[3]

遵循数学的传统，通常会产生更易读的代码：

```python
# Yes: easy to match operators with operands
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

在Python代码中，允许在二进制操作符之前或之后换行，只要约定在本地是一致的。

### 2.5 Blank Lines

用两个空行包围顶级函数和类定义。

类中定义的方法（函数）通过一个空行隔开。

可以(少量地)使用额外的空行来隔开相关函数组。在一组相关的一行程序(例如一组虚拟实现)之间可以省略空行。

在函数中尽量使用空行来指明逻辑部分。

Python接受control-L(例如^ L)换页空白字符;许多工具将这些字符视为页面分隔符，因此您可以使用它们来分隔文件中相关部分的页面。注意，一些编辑器和基于web的代码查看器可能不会将control-L识别为表单提要，而会在其位置显示另一个符号。

### 2.6 Source File Encoding

核心Python发行版中的代码应该始终使用UTF-8(或Python 2中的ASCII)格式。

使用ASCII(在Python 2中)或UTF-8(在Python 3中)的文件不应该有编码声明。

在标准库中，非默认编码只能用于测试目的，或者当注释或文档字符串需要提到包含非ascii字符的作者名时。否则，使用\x、\u、\u或\N转义是在字符串文本中包含非ascii数据的首选方法。

对于Python 3.0及以上版本，标准库规定了以下策略(请参阅PEP 3131)： Python标准库中的所有标识符必须仅使用ascii标识符，并且应该在可行的情况下使用英语单词(在许多情况下，使用的是非英语的缩写词和技术术语)。此外，字符串文字和注释也必须是ASCII格式的。唯一的例外是：(a) 测试用例测试非ascii特性，(b) 作者的名字。不以拉丁字母(Latin alphabet -1, ISO/IEC 8859-1字符集)为基础的作者必须在这个字符集中提供他们名字的音译。

鼓励具有全球受众的开源项目采用类似的策略。

### 2.7 Imports

 导入操作通常按行分开，例：

```python
Yes: import os
     import sys

No:  import sys, os
```

不过也可以这样操作：

from subprocess import Popen, PIPE
import 操作总是放在文件的顶部，在任何模块注释文档之后，在模块全局和常量之前进行。

import 操作应按以下顺序进行:

1. import 标准库
2. import 相关第三方库
3. import 本地自建库

且不同类型的库以空行分割。

建议绝对导入，因为它们通常更易于阅读，并且在导入路径错误时，表现的更友好(至少能给出更友好的错误消息)：

```python
import mypkg.sibling
from mypkg import sibling
from mypkg.sibling import example
```

但是，相对于绝对导入，显式的相对导入也是一种可接受的替代方法，特别是在处理复杂的包布局时，使用绝对导入会不不可避免的冗长，例如：

```python
from . import sibling
from .sibling import example
```

一个标准的库代码应该避免复杂的包布局，并始终使用绝对导入。

在python3中，隐式的相对导入操作不能被使用并且已被删除。

从类包模块中import 一个类时，通常可以这样操作：

```python
from myclass import MyClass
from foo.bar.yourclass import YourClass
```

如果这个 import 方法与本地名有冲突，那么就用下面显示的 import 方法：

```python
import myclass
import foo.bar.yourclass
```

并使用 "myclass.MyClass" 和 "foo.bar.yourclass.YourClass" 操作需要的类。

应该避免通配符导入操作(from \<module> import *)，因为它们容易混淆哪些变量名是导入的，哪些是命名空间中出现的。 

有一个站得住脚的通配符导入用例,这是重新发布的公共API的一个内部接口(例如，使用可选的accelerator模块中的定义重写接口的纯Python实现，以及哪些定义将被重写，这些都是事先不知道的)。当以这种方式重新发布名称时，关于公共和内部接口的准则仍然适用。

### 2.8 Module level dunder names

模块级"dunders"(即名称前后各有两个下划线)，如__all__、__author__、__version__等，应在模块注释之后且任何导模块语句之前写入，除了 from  __future__ 。

Python要求__future__ 的 import 操作必须优先于除了模块注释之外的其他任何操作。例如：

```python
"""This is the example module.

This module does stuff.
"""

from __future__ import barry_as_FLUFL

__all__ = ['a', 'b', 'c']
__version__ = '0.1'
__author__ = 'Cardinal Biggles'

import os
import sys
```

---

## 3. String Quotes

在Python中，单引号字符串和双引号字符串是相同的。PEP8不会对此提出建议。选择一个规则并坚持它。然而，当字符串里包含单引号或双引号字符时，使用另一个字符来避免字符串中的反斜杠。它提高了可读性（不太理解）。

对于三引号字符串，始终使用双引号字符以符合PEP 257中的docstring约定。

---

## 4. Whitespace in Expressions and Statements

### 4.1 Pet Peeves

在以下情况下避免使用多余的空格：

在括号，括号或大括号内

```python
Yes: spam(ham[1], {eggs: 2})
No:  spam( ham[ 1 ], { eggs: 2 } )
```

在括号内的逗号后面和逗号后面的右括号之间

```python
Yes: foo = (0,)
No:  bar = (0, )
```

紧跟在逗号、分号或冒号之前：

```python
Yes: if x == 4: print x, y; x, y = y, x
No:  if x == 4 : print x , y ; x , y = y , x
```
然而，在一个切片中，冒号就像一个二进制操作符，并且两边的数量应该相等(把它当作优先级最低的操作符对待)。在扩展切片中，两个冒号必须具有相同的间距。例外:当切片参数被省略时，空格空间被省略。

```python
Yes:

ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
ham[lower:upper], ham[lower:upper:], ham[lower::step]
ham[lower+offset : upper+offset]
ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
ham[lower + offset : upper + offset]
No:

ham[lower + offset:upper + offset]
ham[1: 9], ham[1 :9], ham[1:9 :3]
ham[lower : : upper]
ham[ : upper]
```

在启动函数调用的参数列表的左括号之前：

```python
Yes: spam(1)
No:  spam (1)
```

在开始索引或切片的开括号之前:

```python
Yes: dct['key'] = lst[index]
No:  dct ['key'] = lst [index]
```

一个赋值(或其他)操作符周围的多个空格，以便与另一个操作符对齐。

```python
Yes:

x = 1
y = 2
long_variable = 3

No:

x             = 1
y             = 2
long_variable = 3
```

### 4.2 Other Recommendations
避免尾随空格。因为它通常是不可见的，所以它可能会令人困惑：例如，反斜杠后跟空格和换行符不算作行延续标记。有些编辑器不保存它，许多项目(比如CPython本身)都有拒绝它的pre-commit hooks 

这些二进制操作符两边通常都会用一个空格：赋值(=)，增广赋值(+=，-=等)，比较(==，<，>，!=，`<>`，<=，>=，in, not in, is, not)，布尔值(and, or, not)。

如果使用不同优先级的操作符，请考虑在优先级最低的操作符周围添加空格。但是，永远不要使用超过一个空格，并且总是在二进制运算符的两边有相同数量的空格。

```python
Yes:

i = i + 1
submitted += 1
x = x*2 - 1
hypot2 = x*x + y*y
c = (a+b) * (a-b)

No:

i=i+1
submitted +=1
x = x * 2 - 1
hypot2 = x * x + y * y
c = (a + b) * (a - b)
```

不要在 = 符号周围使用空格来表示关键字参数或默认参数值。

```python
Yes:

def complex(real, imag=0.0):
    return magic(r=real, i=imag)
No:

def complex(real, imag = 0.0):
    return magic(r = real, i = imag)
```

函数注释应该使用冒号的常规规则，如果存在的话，在->箭头周围总是有空格。(有关函数注释的更多信息，请参阅下面的函数注释。)

```python
Yes:

def munge(input: AnyStr): ...
def munge() -> AnyStr: ...

No:

def munge(input:AnyStr): ...
def munge()->PosInt: ...
```

将参数注释与默认值组合在一起时，在=符号周围使用空格(但仅限于那些同时具有注释和默认值的参数)。

```python
Yes:

def munge(sep: AnyStr = None): ...
def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
No:

def munge(input: AnyStr=None): ...
def munge(input: AnyStr, limit = 1000): ...
```

复合语句(同一行的多个语句)通常不鼓励使用。

```python
Yes:

if foo == 'blah':
    do_blah_thing()
do_one()
do_two()
do_three()
Rather not:

if foo == 'blah': do_blah_thing()
do_one(); do_two(); do_three()
```

虽然有时候在一行中加上if /for / While，但是对于多子句的语句永远不要这样做。也要避免折叠如此长的行：

```python
Rather not:

if foo == 'blah': do_blah_thing()
for x in lst: total += x
while t < 10: t = delay()
Definitely not:

if foo == 'blah': do_blah_thing()
else: do_non_blah_thing()

try: something()
finally: cleanup()

do_one(); do_two(); do_three(long, argument,
                             list, like, this)

if foo == 'blah': one(); two(); three()
```

---

## 5. When to Use Trailing Commas

结尾的逗号通常是可选的，除了在构成一个元素的元组时是强制性需要的(在Python 2中，它们对print语句有语义)。为了清晰起见，建议将后者用括号括起来(在技术上是多余的)。

```python
Yes:

FILES = ('setup.cfg',)
OK, but confusing:

FILES = 'setup.cfg',
```

当末尾逗号是冗余时，当使用版本控制系统时，当一列值、参数或导入项预期会随着时间的推移而增加时，逗号通常是有用的。模式是将每个值(等等)单独放在一行上，总是添加一个逗号，并在下一行添加右括号/括号/括号。然而，在同一行中使用逗号作为结束分隔符是没有意义的(除了在上面的单例元组中)。

```python
Yes:

FILES = [
    'setup.cfg',
    'tox.ini',
    ]
initialize(FILES,
           error=True,
           )
No:

FILES = ['setup.cfg', 'tox.ini',]
initialize(FILES, error=True,)
```

---

## 6. Comments

与代码功能相矛盾的注释比没有注释更糟糕。当代码发生变化时，一定要优先保证注释的更新!

注释应该是完整的句子。第一个单词应该大写，除非它是以小写字母开头的标识符(千万不要改变标识符的大小写!)

块注释通常由完整句子组成的一个或多个段落组成，每个句子以句号结尾。

在多句注释中，除了最后一句之外，在一个句子结束后应该使用两个空格。

来自非英语国家的Python程序员：请用英语写你的注释，除非你有120%的把握知道代码不会被不懂你语言的人读到。

### 6.1 Block Comments 

块注释通常适用于跟随它们的一些(或所有)代码，并且缩进到与代码相同的级别。块注释的每一行都以#和单个空格开头(除非它是注释内部缩进的文本)。

块注释中的段落由包含单个#的行分隔。

### 6.2 Inline Comments

有节制的使用内联注释。

内联注释是与语句同一行的注释。内联注释应该与语句至少分隔两个空格。它们应该从一个#和一个空格开始。

如果代码陈述了明显的内容，那么内联注释是不必要的，实际上内联注释还会分散注意力。不要这样做：

```python
x = x + 1                 # Increment x

# But sometimes, this is useful:

x = x + 1                 # Compensate for border
```

### 6.3 Documentation Strings

对已经编写好的文档字符串的约定（a.k.a. “docstring”）在PEP 257中哟永久保留。

所有公共模块、函数、类和方法需要编写文档字符串。非公共方法不需要编写文档字符串，但是应该有一个注释来描述该方法的功能。这个注释应该出现在def行之后。

PEP 257描述了良好的文档字符串约定。最重要的是，结束多行文档字符串的""" 应该单独在一行上:。

```python
"""Return a foobang
Optional plotz says to frobnicate the bizbaz first.
"""
```

对于一个行文档字符串，请保持结束注释时的 """ 在同一行。 

---

## 7. Naming Conventions

Python库的命名约定有点混乱，因此我们永远不会得到完全一致的命名约定——不过，下面是当前推荐的命名标准。应该根据这些标准编写新的模块和包(包括第三方框架)，但如果现有库具有不同的风格，则首选内部一致性。

### 7.1 Overriding Principle

作为API的公共部分，对用户可见的库命名应该遵循反映使用情况而不是实现的约定。

### 7.2 Descriptive: Naming Styles

有很多不同的命名方式。它有助于识别正在使用的命名风格，独立于它们的用途。

以下命名风格通常是可以区分的：

b (单个小写字母)

B (单个大写字母)

lowercase

lower_case_with_underscores

UPPERCASE

UPPER_CASE_WITH_UNDERSCORES

CapitalizedWords (或 CapWords, 或 CamelCase -- 之所以这样命名是因为它的字母[4]看起来很凹凸不平)。这有时也被称为StudlyCaps

注意：当在CapWords中使用首字母缩略词时，需要将首字母缩略词大写。因此HTTPServerError比HttpServerError好。

mixedCase (与CapitalizedWords不同的是首字母小写!)

Capitalized_Words_With_Underscores (丑陋的!)

还有一种风格是使用一个简短的唯一前缀将相关名称组合在一起。这在Python中并不常用，但为了完整起见，本文提到了这一点。例如，os.stat()函数返回一个元组，其项通常具有st_mode、st_size、st_mtime等名称。(这样做是为了强调与POSIX系统调用struct的字段的对应关系，这有助于程序员熟悉这些字段。)

X11库的所有公共功能都使用一个前导X。在Python中，这种样式通常被认为是不必要的，因为属性和方法名的前缀是对象，函数名的前缀是模块名。（不理解）

此外，我们还了解到以下使用前导或后导下划线的特殊形式(这些通常可以与任何例子约定结合使用):

_single_leading_underscore: weak "internal use" indicator. E.g. from M import *  不会import 任何以下划线开头的对象

single_trailing_underscore_: 用于避免与Python关键字冲突的约定, e.g.

```python
Tkinter.Toplevel(master, class_='ClassName')
```

__double_leading_underscore：在命名类属性时，调用名称管理(inside class FooBar, __boo becomes _FooBar__boo; see below).

__double_leading_and_trailing_underscore__: "magic" 对象或来自于用户控制的命名空间中的属性. E.g. __init__, __import__ or __file__. 从来没有发明这样的名字;只在文档中使用它们。

---

## 8. Prescriptive: Naming Conventions

### 8.1 Names to Avoid

千万不要把“l”(小写字母el)、“O”(大写字母oh)或“I”(大写字母eye)作为单个字符变量名。
在某些字体中，这些字符与数字1和0没有区别。当你想用“l”的时候，用“L”代替。

### 8.2 ASCII Compatibility

标准库中使用的标识符必须与ASCII兼容，如PEP 3131的策略部分所述。

### 8.3 Package and Module Names

模块应该有简短的、全小写的名称。如果能提高可读性，可以在模块名中使用下划线。Python包也应该有简短的、全小写的名称，尽管不鼓励使用下划线。

当用C或c++编写的扩展模块有一个附带的Python模块，该模块提供更高级别的接口(例如，更面向对象的接口)时，C/ c++模块有一个前导下划线(例如_socket)。

### 8.4 Class Names

类名通常应该使用CapWords约定。

The naming convention for functions may be used instead in cases where the interface is documented and used primarily as a callable.

注意，内建名称有一个单独的约定:大多数内建名称是单个单词(或两个单词一起运行)，而CapWords约定仅用于异常名称和内建常量。

### 8.5 Type Variable Names

在PEP 484中引入的类型变量的名称通常更倾向于使用简短名称的标题:T, AnyStr, Num.建议在用于声明协变或逆变行为的变量中添加后缀_co或_contra：

```python
from typing import TypeVar

VT_co = TypeVar('VT_co', covariant=True)
KT_contra = TypeVar('KT_contra', contravariant=True)
```

### 8.6 Exception Names

因为异常应该是类，所以这里使用类命名约定。但是，您应该在异常名称上使用后缀“Error”(如果异常实际上是一个错误)。

### 8.7 Global Variable Names

(我们希望这些变量只在一个模块中使用。)这些约定与函数的约定大致相同。

具有 from M import * 设计的模块应该使用__all__机制来防止导出全局变量，或者使用旧的将这些全局变量加上下划线作为前缀的惯例(您可能希望这样做以表明这些全局变量是“模块非公共的”)。

### 8.8 Function and Variable Names

函数名应该是小写的，需要用下划，线分隔单词以提高可读性。

变量名与函数名遵循相同的约定。

mixedCase只能在已经流行的样式(例如thread .py)的上下文中使用，以保持向后兼容性。

### 8.9 Function and Method Arguments

使用self作为第一个参数来实例化方法。

使用cls作为类方法的第一个参数

如果函数参数的名称与保留关键字冲突，通常最好是附加一个单后置下划线，而不是使用缩写或拼写错误。因此class_比clss好。(最好是使用同义词来避免这种冲突。)

### 8.10 Method Names and Instance Variables

使用函数命名规则：必须用下划线分隔单词的小写字母来提高可读性。

仅对非公共方法和实例变量使用一个前导下划线。

为了避免与子类的名称冲突，使用两个前导下划线来调用Python的名称管理规则。

Python用类名来处理这些名称:如果类Foo有一个名为`__a`的属性，`Foo.__a`就不能访问它。(坚持不懈的用户仍然可以通过调用`Foo._Foo__a`获得访问权限。)通常，双前导下划线应该仅用于避免名称与设计为子类的类中的属性冲突。

注意:关于使用`__name`存在一些争议(见下文)。

### 8.11 Constants

常量通常在模块级别上定义，用所有大写字母和下划线分隔单词。示例包括MAX_OVERFLOW和TOTAL。

### 8.12 Designing for Inheritance

决定类的方法和实例变量(统称为“属性”)应该是公共的还是非公共的。如有疑问，选择非公开;晚些时候将其公开比将公共属性非公开要容易得多。

公共属性是您期望类中不相关的客户端使用的属性，并承诺避免向后不兼容的更改。非公共属性是指不打算由第三方使用的属性;您不能保证非公共属性不会更改，甚至不会被删除。

这里我们不使用术语“private”，因为Python中没有真正的私有属性(没有通常不必要的工作量)。

另一类属性属于“子类API”(在其他语言中通常称为“受保护”)。有些类的设计是为了继承，或者扩展或者修改类的行为。在设计这样的类时，要明确地决定哪些属性是公共的，哪些属性是子类API的一部分，哪些属性仅供基类使用。

考虑到这一点，下面是python的指导方法：

公共属性应该没有前导下划线。

如果公共属性名与保留关键字冲突，则在属性名后面附加一个下划线。这比缩写或错误的拼写更可取。(然而，尽管有这个规则，“cls”是任何已知为类的变量或参数的首选拼写，特别是类方法的第一个参数)。

注意1：关于类方法，请参阅上面的参数名建议。

对于简单的公共数据属性，最好只公开属性名，而不使用复杂的访问器/变量方法。请记住，如果您发现一个简单的数据属性需要增长功能行为，Python提供了一条通向未来增强的简单路径。在这种情况下，使用属性将函数实现隐藏在简单的数据属性访问语法后面。

注意1：属性只适用于新样式的类。

注意2：尽量避免函数行为的副作用，尽管缓存之类的副作用一般都很好。

注意3：避免将属性用于昂贵的计算操作;属性符号使调用者认为访问(相对)便宜。

如果您的类是要被子类化的，并且您有不希望子类使用的属性，请考虑使用双前导下划线和无尾随下划线来命名它们。这将调用Python的名称管理算法，其中类的名称将被划分为属性名。这有助于避免属性名冲突，因为子类可能无意中包含同名的属性。

注意1：请注意，在混乱的名称中只使用简单的类名，因此如果子类同时选择相同的类名和属性名，您仍然可以得到名称冲突。

注意2：名称管理可能会使某些用途(如调试和__getattr__())变得不方便。但是，名称管理算法有很好的文档记录，并且很容易手工执行。

注意3：不是每个人都喜欢拼名字。尽量在避免意外名称冲突与高级调用程序潜在使用之间保持平衡。

### 8.13 Public and Internal Interfaces

任何向后兼容保证只适用于公共接口。因此，重要的是用户能够清楚地区分公共接口和内部接口。

文档化的接口被认为是公共的，除非文档显式地声明它们是临时的或内部接口，而不受通常向后兼容保证的约束。所有未文档化的接口都应该假定为内部接口。

为了更好地支持内省，模块应该使用__all__属性显式地声明其公共API中的名称。将__all__设置为空列表表示该模块没有公共API。

即使正确设置了__all__，内部接口(包、模块、类、函数、属性或其他名称)仍然应该以一个前导下划线作为前缀。

如果任何包含名称空间(包、模块或类)被认为是内部的，那么接口也被认为是内部的。

导入的名称应该始终被视为实现细节。其他模块不能依赖对这些导入名称的间接访问，除非它们是包含模块API(如os)的显式文档部分。路径或包的__init__模块，该模块公开子模块的功能。

---

## 9. Programming Recommendations

代码的编写方式应该不影响Python的其他实现(PyPy、Jython、IronPython、Cython、Psyco等等)。

例如,不依赖CPython的高效实现就地字符串连接的语句形式+ = b或a = a + b。这种优化即使在CPython中也是脆弱的(它只适用于某些类型)，并且在不使用refcount的实现中根本不存在。在库的性能敏感部分，应该使用" .join()表单。这将确保连接在各个变量之间的线性时间内发生。

像None这样的单例比较操作应该使用is或not来做，永远不要用等式运算符。

另，要注意写if x ，当你真正的意思 if x is not None -- e.g测试一个默认为None的变量或参数是否被设置为其他值时。另一个值可能有一个类型(比如容器)，在布尔上下文中可能为false !

使用is not 操作，而不是 not ... is是多少。虽然这两个表达式在功能上是相同的，但前者更易于阅读，更受青睐。

```python
Yes:

if foo is not None:
No:

if not foo is None:
```

在实现具有丰富比较的排序操作时，最好实现所有的六个操作(__eq__、__ne__、__lt__、__le__、__gt__、__ge__)，而不是依赖其他代码来执行特定的比较操作。
为了尽量减少工作，functools. total_ordered()装饰器提供了一个工具来生成缺少的比较方法。

PEP 207表示Python假定了自反性规则。因此,解释器可能交换  y > x 及x < y, y >= x 及 x <= y, 可能交换参数x == y和! = y。sort ()和min ()操作保证使用<操作符和max()函数使用>操作符。但是，最好实现所有六个操作，这样在其他上下文中就不会出现混淆。

始终使用def语句，而不是将lambda表达式直接绑定到标识符的赋值语句

```python
Yes:

def f(x): return 2*x
No:

f = lambda x: 2*x
```

第一种形式表示生成的函数对象的名称是“f”，而不是通用的 “`<lambda>`” 。这对于一般的回溯和字符串表示更有用。赋值语句的使用消除了lambda表达式相对于显式def语句所能提供的唯一好处(即它可以嵌入到更大的表达式中)

从异常而不是基异常中派生异常。来自BaseException的直接继承保留给异常，在这些异常中，捕获它们几乎总是错误的。

根据捕获异常的代码可能需要的区别来设计异常层次结构，而不是异常产生的位置。以编程的方式回答“哪里出错了?”，而不是仅仅说明“发生了问题”(请参阅PEP 3151，以获得关于内置异常层次结构的这一课的示例)

类命名约定适用于此，但如果异常是错误的，则应该将后缀“Error”添加到异常类中。用于非本地流控制或其他信令形式的非错误异常不需要特殊后缀。

适当地使用异常链接。在python3中，“raise X from Y”应该被用来表示显式的替换而不丢失原始回溯。

当故意更换内部异常(使用Python 2中“raise X”或在Python 3.3 +中使用“raise X from None“),确保相关信息转移到新异常(如保护属性名称转换KeyError AttributeError时,或嵌入的文本原始异常在新的异常消息)。

在Python 2中引发异常时，使用raise ValueError('message')而不是较老的表单raise ValueError('message')。

后一种形式不是合法的Python 3语法。

使用参数的形式还意味着，当异常参数很长或包含字符串格式时，由于包含括号，您不需要使用行延续字符。

当捕捉异常时，尽可能提及特定的异常，而不是使用一个简单的except:子句:

```python
try:
    import platform_specific_module
except ImportError:
    platform_specific_module = None
```

一个简单的except：子句将捕获SystemExit和KeyboardInterrupt异常，使得用Control-C中断一个程序变得更加困难，并且可以掩盖其他问题。如果你想捕获所有的异常，那就使用except Exception:：(除了等于 except BaseException:)。

一个好的经验法则是将“except”子句的使用限制在两种情况下:

如果异常处理程序将打印或记录回溯;至少用户会知道发生了错误。

如果代码需要做一些清理工作，请使用raise. try...finally 向上传播抛出异常。

当将捕获的异常绑定到名称时，最好使用Python 2.6中添加的显式名称绑定语法:

```python
try:
    process_data()
except Exception as exc:
    raise DataProcessingFailedError(str(exc))
```

这是Python 3中唯一支持的语法，并避免了与旧的基于逗号的语法相关的歧义问题。

在捕获操作系统错误时，优先选择Python 3.3中引入的显式异常层次结构，而不是errno值的自省。

此外，对于所有try/except子句，将try子句限制为所需代码的绝对最小数量。同样，这可以避免掩盖bug。

```python
Yes:

try:
    value = collection[key]
except KeyError:
    return key_not_found(key)
else:
    return handle_value(value)
No:

try:
    # Too broad!
    return handle_value(collection[key])
except KeyError:
    # Will also catch KeyError raised by handle_value()
    return key_not_found(key)
```

当资源是特定代码段的本地资源时，使用with语句确保在使用后能够迅速、可靠地清理资源。try/finally语句也是可以接受的。

上下文管理器应该通过单独的函数或方法调用，只要它们做的不是获取和释放资源：

```python
Yes:

with conn.begin_transaction():
    do_stuff_in_transaction(conn)
No:

with conn:
    do_stuff_in_transaction(conn)
```

后一个示例没有提供任何信息来表明__enter__和__exit__方法在执行事务后关闭连接之外的其他操作。在这种情况下，明确是很重要的。

在返回语句中保持一致。函数中的所有返回语句都应该返回表达式，或者它们都不应该返回表达式。如果任何return语句返回一个表达式，任何没有返回值的return语句都应该显式地将其声明为return None，并且在函数末尾应该显示显式的return语句(如果可达的话)。

```python
Yes:

def foo(x):
    if x >= 0:
        return math.sqrt(x)
    else:
        return None

def bar(x):
    if x < 0:
        return None
    return math.sqrt(x)
No:

def foo(x):
    if x >= 0:
        return math.sqrt(x)

def bar(x):
    if x < 0:
        return
    return math.sqrt(x)
```

使用string方法而不是string模块。

字符串方法总是快得多，并且与unicode字符串共享相同的API。如果需要向后兼容大于2.0的python，则重写此规则。

使用" .startswith()和" .endswith()代替字符串切片检查前缀或后缀。

startswith()和endswith()更干净，更不容易出错：

```python
Yes: if foo.startswith('bar'):
No:  if foo[:3] == 'bar':
```

对象类型比较应该始终使用isinstance()，而不是直接比较类型。

```python
Yes: if isinstance(obj, int):

No:  if type(obj) is type(1):
```

当检查对象是否为字符串时，请记住它可能也是unicode字符串!在Python 2中，str和unicode有一个公共基类basestring，因此您可以这样做:

```python
if isinstance(obj, basestring):
```

注意，在Python 3中，unicode和basestring不再存在(只有str)， bytes对象不再是某种字符串(而是整数序列)。

对于序列(字符串、列表、元组)，使用空序列为假的事实。

```python
Yes: if not seq:
     if seq:

No:  if len(seq):
     if not len(seq):
```

不要写依赖于重要的尾随空格的字符串文字。这样的结尾空格在视觉上难以区分，一些编辑器(或者最近的reindent.py)会对它们进行修剪。

不要使用==将布尔值与真值或假值进行比较。

```python
Yes:   if greeting:
No:    if greeting == True:
Worse: if greeting is True:
```

### 9.1 Function Annotations

随着PEP 484的验收，函数注释的样式规则正在发生变化。

为了向前兼容，Python 3代码中的函数注释最好使用PEP 484语法。(在前一节中有一些注释格式建议。)

以前在这个PEP中推荐的注释样式的实验不再被鼓励。

然而，在stdlib之外，现在鼓励使用PEP 484规则进行实验。例如，使用PEP 484样式类型注释标记大型第三方库或应用程序，检查添加这些注释有多么容易，并观察它们的存在是否增加了代码的可理解性。

Python标准库在采用这种注释时应该是保守的，但是对于新代码和大型重构，允许使用这些注释。

对于想要使用不同功能注释的代码，建议使用以下形式的注释:

```python
# type: ignore
```

接近文件顶部；这告诉类型检查器忽略所有注释。(在PEP 484中可以找到更细粒度的方法来禁用类型检查程序的投诉。)

与linters一样，类型检查器也是可选的、独立的工具。Python解释器默认情况下不应该因为类型检查而发出任何消息，也不应该基于注释改变它们的行为。

不想使用类型检查器的用户可以随意忽略它们。但是，第三方库包的用户可能希望在这些包上运行类型检查程序。为此，PEP 484建议使用存根文件:.pyi文件，类型检查器优先读取相应的.py文件。存根文件可以通过一个库进行分发，也可以通过类型化的repo[5]单独(在库作者的允许下)进行分发。

对于需要向后兼容的代码，可以以注释的形式添加类型注释。请参阅PEP 484[6]的相关部分。

### 9.2 Variable Annotations

PEP 526引入了变量注释。它们的的书写风格建议参考上面描述的函数注释:

模块级变量、类和实例变量以及局部变量的注释应该在冒号后面有一个空格。

冒号前面应该没有空格。

如果一个任务有有右边，那么等号两边应该只有一个空格。

```python
Yes:

code: int

class Point:
    coords: Tuple[int, int]
    label: str = '<unknown>'
No:

code:int  # No space after colon
code : int  # Space before colon

class Test:
    result: int=0  # No spaces around equality sign
```

虽然Python 3.6接受PEP 526约束，但是对于所有Python版本的存根文件，变量注释语法是首选语法(详细信息请参阅PEP 484)。

附注：

挂缩进是一种类型设置样式，其中除了第一行以外，段落中的所有行都缩进。在Python的上下文中，这个术语用于描述这样一种样式:圆括号语句的开头括号是行的最后一个非空格字符，后面的行缩进直到结束括号。

---

[原文](https://blog.csdn.net/wk585858/article/details/82254907)
[其他译文](https://blog.csdn.net/ratsniper/article/details/78954852)
[官方原文](https://www.python.org/dev/peps/pep-0008/)
