---
title: Python代码风格指南
prev: /docs/codingStyles
---

## 引入

本指南档提供了Aaron212制定的的Python代码风格指南。主要源自
[Python PEP 8](https://peps.python.org/pep-0008/)
以及一些个人细节/习惯上的调整。

## 一致性为王

Guido本人的其中一个极其重要的见解是读代码比写代码更为多见。
本指南所提供的代码风格是为了提升代码的可读性，也是为了保持不同源代码之间的一致性。

但是，也要明白该放弃一致性的时候——有时候代码风格建议是根本无法被应用的，此时，
请相信自己的判断。看看其他的例子，想想自己的决定。如遇问题，不要掖着不问。

特别的：不要为了与本指南兼容而去破坏向后兼容性！

以下是一些不使用代码风格指南的好理由：

1. 当依照本指南编码时，可读性不升反降；

2. 为了与周围的代码保持一致，而违反了本指南；

3. 相关代码早于本指南的引入，并且没有其他理由修改该代码；

4. 当代码需要与不支持样式指南推荐的功能的旧版本的Python保持兼容时。

随着其他惯例的确定和过去的惯例因语言本身的变化而过时，本风格指南会随着时间的推移而演变。

许多项目都有自己的代码风格指南。如果发生任何冲突，此类特定于项目的指南优先于本指南。

## 代码布局

### 缩进

使用4空格缩进。

过长的连续行应该使用**悬挂缩进**，并应做到以下两点：第一行不应有参数，
使用进一步的缩进来明确区分自己是一个连续行：

```python
# 正确

# 加四个空格（进一步缩进）
def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

foo = long_function_name(
    var_one, var_two,
    var_three, var_four)

# 一般长度，则不需要换行/悬挂缩进
bar = long_function_name(var_one, var_two)
```

```python
# 错误

# 第一行不为空
foo = long_function_name(var_one, var_two,
    var_three, var_four)

# 第二行开始没有进一步缩进
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print(var_one)
```

当`if`语句的条件部分足够长，要求它跨越多行书写时，值得注意的是，
两个字符关键字（即`if`）加上单个空格加上开头括号的组合为多行条件的后续行创建一个自然的4空格缩进。
PEP 8没有指定规范，这里我们同样使用悬挂缩进，增加一个缩进：

```python
if (this_is_one_statement
        and that_is_another_statement):
    do_something()
```

（另请参阅下面关于在二进制运算符之前还是之后换行的讨论。）

多行结构上的闭合大括号/括号/括号排列在开始多行构造的行的第一个字符下，如：

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

### 空格还是制表符？

请用空格，除非旧代码用制表符。

### 最大行宽

对于所有行来说，每行不应超过80个字符。

一般来说，限制行宽是为了适应一些小窗口，比如`80x24`的命令行窗口。但就现在来说，即便是命令行窗口
也拓展到了`120x36`，并且还有Visual Studio Code等及其美观的IDE来帮助我们编程，最大行宽
似乎变得没有那么必要。

但其实这个限制还是需要的，因为当你需要并排对比文件时，屏幕宽度会缩窄。比如说，
2023 Macbook Air 15'款，有一个默认`2880p`缩放到`1710p`的屏幕，用`12pt`字号，
只能显示244个字符，而由于现代编辑器的一些UI肯定远超4个字符宽，因此120个字符宽的两个文件
是无法在这个Macbook上并排展示的。而对于更加舒适`14pt`字号，甚至只能并排预览两个80个字符宽的
文件。于是乎，行宽默认限制在了80个字符。但是如果你屏幕够大，或者字号够小，抑或者愿意牺牲
自己的多文件平行处理的能力，这个限制可以被放宽至100甚至120个字符，但请记得打开自动换行，
以免一些意外情况。

### 该在二元运算符之前还是之后换行？

几十年来，推荐的风格是在二进制运算符之后换行。
但这可能会在两个方面降低可读性：运算符往往分散在屏幕上的不同列中，每个运算符都远离其上一行操作数。
比方说这个例子，目光必须瞟来瞟去才能判断哪些是加的，哪些是减的：

```python
# 错误
# 运算符离操作数太远了
income = (gross_wages +
            taxable_interest +
            (dividends - qualified_dividends) -
            ira_deduction -
            student_loan_interest)
```

为了解决这个可读性问题，数学家和他们的出版商遵循的是相反的惯例。
Donald Knuth在他的计算机和排版系列中解释了传统规则：
“尽管段落中的公式总是在二元运算符和关系运算符后换行，但单独展示的公式总是在二元运算符之前换行。”

遵循数学的传统通常会产生更易读的代码：

```python
# 正确
# 运算符前换行，既能对齐，还更美观
income = (gross_wages
            + taxable_interest
            + (dividends - qualified_dividends)
            - ira_deduction
            - student_loan_interest)
```

本指南确定与Knuth风格一致，代码应在二进制运算符之前换行。

### 空行

在顶级函数和类定义之间用两个空行包围。

类内的方法定义由单个空行包围。

可以使用额外的空行（适量地）来分隔相关函数组。
在一组相关的单行函数之间可以省略空行（例如一组虚拟实现）。

在函数中适度使用空行来指示逻辑部分。

Python接受`CTRL+L`（即`^L`）作为空白字符；许多工具将这些字符视为页面分隔符，
因此您可以使用它们来分隔文件中相关部分的页面。
请注意，某些编辑器和基于Web的代码查看器可能无法`CTRL+L`识别为换页符，并会显示其他字符。

### 源代码编码格式

Python代码应通过UTF-8进行编码，不需要代码中指明编码格式。
非UTF-8编码只能作为测试代码，不应出现在正式代码中。

需要注意的是，源代码文件应尽量避免出现非ASCII字符，也就是说不应出现非英语命名的变量、函数等。

### 导入

- 导入应按行分开：

```python
# 正确
import os
import sys
```

```python
# 错误
import sys, os
```

但是导入同一个模块下的对象允许写在同一行：

```python
# 正确
from subprocess import Popen, PIPE
```

- 导入应被放置在代码文件的头部，在模块文档和代码文档后，并在全局变量之前。

  导入应按如下顺序排列并分组：

  1. 标准库/模块
  2. 相关的第三方库/模块
  3. 本地的库/模块

  每个分组之间应用一个空行隔开。

- 应优先使用绝对导入，因为它们通常更易读，并且如果导入系统配置不正确
  （例如，当包内的目录出现在`sys.path`中时），它们往往表现更好（或至少提供更好的错误消息）。

  ```python
  # 正确

  import mypkg.sibling
  from mypkg import sibling
  from mypkg.sibling import example
  ```

  请不要使用相对路径导入！

  ```python
  # 错误

  from . import sibling
  from .sibling import example
  ```

- 当从一个模块导入子模块时，一般用如下写法：

  ```python
  from myclass import MyClass
  from foo.bar.yourclass import YourClass
  ```

  但是如果遇到了命名冲突，则可以使用`as`重命名后导入：

  ```python
  from myclass import SimpleClass as ClassA
  from foo.bar.yourclass import SimpleClass as ClassB
  ```

  并分别使用`ClassA`和`ClassA`。

  如果需要导入的重名类很多，则可以分别导入：
  
  ```python
  import myclass
  import foo.bar.yourclass
  ```

  并分别使用`myclass.SimpleClass`和`foo.bar.yourclass.SimpleClass`等类。
  需要注意的是，仅推荐在重名类过多的情况下使用本方法。
  若重名不多，建议使用重命名法。

- 禁止使用通配符导入（例如`from module import *`），
  因为它们会使得命名空间中存在哪些名称变得相当不清晰，并给读者和许多自动化工具带来困惑。
  
  但是有一个特例，那就是将内部接口重新发布为公共API的一部分
  （例如，使用可选的加速器模块的定义覆盖一个纯Python实现的接口，
  但无法提前知道将覆盖哪些定义）。

  在这种重新发布名称的情况下，仍然适用以下关于公共接口和内部接口的准则。

### 模块级别的双下划线命名

模块级别的双下划线命名是指以双下划线开头和结尾的名称，
例如`__all__`, `__author__`, `__version__`等，应当被放置在模块文档后，
但应放置在其他导入之前，除非这个模块是`__future__`。
Python强调过，`__future__`模块应在所有其他模块导入之前导入，前面仅能出现代码文档。

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

## 引号字符串

在Python中，有单引号包括的字符串与双引号包括的字符串行为一致。
PEP 8没有强制标准，但强调了一致性。

本指南使用双引号作为字符串的指示。

对于三重引号字符串，同样使用双引号。

```python
# 正确
example_short_string = "a, b, c"
example_long_string = """aaa, bbb, ccc"""
```

```python
# 错误
example_short_string = 'a, b, c'
example_long_string = '''aaa, bbb, ccc'''
```

## 表达式与语句中的空格

### 基础

在以下情况不要使用空格隔开：

- 在各种括号内的两侧：
  
  ```python
  # 正确
  spam(ham[1], {eggs: 2})
  ```

  ```python
  # 错误
  spam( ham[ 1 ], { eggs: 2 } )
  ```

- 末尾逗号和括号之间:

  ```python
  # 正确
  foo = (0,)
  ```

  ```python
  # 错误
  bar = (0, )
  ```

- 逗号、句号和分号前：

  ```python
  # 正确
  if x == 4: print(x, y); x, y = y, x
  ```

  ```python
  # 错误
  if x == 4 : print(x , y) ; x , y = y , x
  ```

- 然而，在切片操作中，冒号的行为类似于二元运算符，并且应该在两侧具有相等的空格
  （将其视为优先级最低的运算符）。在扩展切片中，两个冒号之间必须有相同数量的空格。
  例外情况是，当切片参数被省略时，空格也被省略：

  ```python
  # 正确
  ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
  ham[lower:upper], ham[lower:upper:], ham[lower::step]
  ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
  ham[lower+offset : upper+offset]
  ```

  ```python
  # 错误
  ham[lower + offset : upper + offset] # 这个为什么错，见下一节
  ham[lower + offset:upper + offset]
  ham[1: 9], ham[1 :9], ham[1:9 :3]
  ham[lower : : step]
  ham[ : upper]
  ```

- 括号前面是函数：

  ```python
  # 正确
  spam(1)
  ```

  ```python
  # 错误
  spam (1)
  ```

- 括号前面是列表：

  ```python
  # 正确
  dct['key'] = lst[index]
  ```

  ```python
  # 错误
  dct ['key'] = lst [index]
  ```

- 不要对齐等号：

  ```python
  # 正确
  x = 1
  y = 2
  long_variable = 3
  ```

  ```python
  # 错误
  x             = 1
  y             = 2
  long_variable = 3
  ```

### 其他

- 避免行末空格。除非你设置过你的编辑器，否则一般来说行末空格是看不见，而且会影响一些功能
  比方说，当一个反斜杠后面有一个空格时，这个反斜杠就不会被视为行连接符。

- 在二元运算符周围加上一个空格：赋值（`=`）、变化赋值（`+=`、`-=`等）、
  比较（`==`、`<`、`>`、`!=`、`<>`、`<=`、`>=`、`in`、`not in`、`is`、
  `is not`）和布尔运算（`and`, `or`, `not`）。

- 如果使用具有不同优先级的运算符，请考虑在具有最低优先级的运算符周围添加空格。
  请根据自己的判断进行处理；但是，永远不要使用超过一个空格，
  并且始终在二元运算符的两侧使用相同数量的空格：

  ```python
  # 正确
  i = i + 1
  submitted += 1
  x = x*2 - 1
  hypot2 = x*x + y*y
  c = (a+b) * (a-b)
  ```

  ```python
  # 错误
  i=i+1
  submitted +=1
  x = x * 2 - 1
  hypot2 = x * x + y * y
  c = (a + b) * (a - b)
  ```

- 函数注释应该遵循冒号的常规规则，并且如果存在`->`箭头，箭头周围应该始终有空格：
  
  ```python
  # 正确
  def munge(input: AnyStr): ...
  def munge() -> PosInt: ...
  ```

  ```python
  # 错误
  def munge(input:AnyStr): ...
  def munge()->PosInt: ...
  ```

- 在用于指示关键字参数或未注释的函数参数的默认值时，不要在等号周围使用空格：
  
  ```python
  # 正确
  def complex(real, imag=0.0):
      return magic(r=real, i=imag)
  ```

  ```python
  # 错误
  def complex(real, imag = 0.0):
      return magic(r = real, i = imag)
  ```

  当将参数注释与默认值结合时，确保在等号周围使用空格。

  ```python
  # 正确
  def munge(sep: AnyStr = None): ...
  def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
  ```

  ```python
  # 错误
  def munge(input: AnyStr=None): ...
  def munge(input: AnyStr, limit = 1000): ...
  ```

- 通常不鼓励使用复合语句（同一行上的多个语句）:

  ```python
  # 正确
  if foo == 'blah':
      do_blah_thing()
  do_one()
  do_two()
  do_three()
  ```

  ```python
  # 错误
  if foo == 'blah': do_blah_thing()
  do_one(); do_two(); do_three()
  ```

- 在某些情况下，将带有短小代码块的`if`/`for`/`while` 语句放在同一行上是可以接受的，
  但请勿将多个子句的语句放在同一行上。此外，请避免将过长的代码行折叠在同一行上:

  ```python
  # 错误
  if foo == 'blah': do_blah_thing()
  for x in lst: total += x
  while t < 10: t = delay()
  ```

  千万不要这么做，否则我会代表程序员们打死你的：

  ```python
  # 错误
  if foo == 'blah': do_blah_thing()
  else: do_non_blah_thing()

  try: something()
  finally: cleanup()

  do_one(); do_two(); do_three(long, argument,
      list, like, this)

  if foo == 'blah': one(); two(); three()
  ```

## 何时使用行末逗号

行末逗号通常是可选的，但在创建只有一个元素的元组时，它们是必需的。
为了清晰起见，建议在这种情况下使用括号来包围该元素。

```python
# 正确
FILES = ('setup.cfg',)
```

```python
# 错误
FILES = 'setup.cfg',
```

当尾随逗号是多余的时候，它们通常在用Git时很有帮助，
尤其是当预期随着时间推移需要修改值、参数或导入项的列表时。
这种模式是将每个值（等等）放在单独的一行上，始终添加尾随逗号，
并在下一行添加闭合的括号/方括号/花括号。
然而，在与结束分隔符（除了上述的单元素元组情况）相同的行上添加尾随逗号是没有意义的：

```python
# 正确
FILES = [
    'setup.cfg',
    'tox.ini',
]
initialize(
    FILES,
    error=True,
)

FILES = ['setup.cfg', 'tox.ini']
initialize(FILES, error=True)
```

```python
# 错误
FILES = ['setup.cfg', 'tox.ini',]
initialize(FILES, error=True,)
```

## 注释

非英语国家的Python开发者，请使用英语编写您的注释，
除非你非常确定代码永远不会被不懂您语言的人阅读，或者你正在撰写一门教程。

与代码相矛盾的注释比没有注释更糟糕。当代码发生变化时，切记优先确保注释保持最新！

注释应该是完整的句子。除非是以小写字母开头的标识符（不要改变标识符的大小写），
否则第一个单词应该大写。

块注释通常由一个或多个段落组成，每个句子都以句号结尾，构成完整的句子。

在多句注释中，在句子结尾的句号后使用一个或两个空格，除了最后一句之后。

确保你的注释清晰易懂，对其他使用您所编写的语言的人来说容易理解。

### 块注释

块注释通常适用于紧随其后的一些（或全部）代码，并且缩进与该代码相同。块注释的每一行以`#`和一个空格开头（除非是注释内的缩进文本）。

块注释内的段落由包含单个`#`的行分隔。

### 内联注释

请谨慎使用内联注释。

内联注释是与语句在同一行上的注释。内联注释应该与语句分隔至少两个空格。
它们应该以`#`和一个空格开头。

如果内联注释只是陈述了显而易见的内容，则是不必要的，事实上会分散注意力。不要这样做：

```python
x = x + 1                 # Increment x
```

但一些情况下，内联注释也很有用：

```python
x = x + 1                 # Compensate for border
```

所以写好注释真的很重要。

### 文档字符串

在编写良好的文档字符串（也称为docstrings）方面，有一些约定俗成的规范，
这些规范被记录在PEP 257中。

- 为所有公共模块、函数、类和方法编写文档字符串。
  非公共方法不需要文档字符串，但你应该有一条注释来描述该方法的功能。
  这个注释应该出现在`def`行之后。

- PEP 257描述了良好的文档字符串约定。
  需要注意的是，多行文档字符串的结束处的`"""`应该独占一行：

  ```python

  """Return a foobang

  Optional plotz says to frobnicate the bizbaz first.
  """
  ```

- 对于但行的文档字符串，请将结束处的`"""` 放在同一行：

  ```python
  """Return an ex-parrot."""
  ```

## 命名约定

Python库的命名约定有些混乱，所以我们永远无法完全保持一致。
然而，以下是目前推荐的命名规范。新的模块和包（包括第三方框架）应该按照这些标准编写，
但是对于已有的库，内部一致性是首选。

### 覆盖原则

对于用户可见的作为API公共部分的名称，应该遵循反映用法而非实现的约定。

### 描述性的命名风格

存在许多不同的命名风格。能够独立于其用途识别所使用的命名风格是有帮助的。

以下命名风格是被广泛认可且易于分辨的：

- `b`：单个小写字母
- `B`：单个大写字母
- `lowercase`：全小写
- `lower_case_with_underscores`：全小写带下划线
- `UPPERCASE`：全大写
- `UPPER_CASE_WITH_UNDERSCORES`：全大写带下划线
- `uncapitalizedWords`：小驼峰式
- `CapitalizedWords`：大驼峰式
  
  请注意：在大驼峰式中使用缩写时，请保持全大写。
  也就是说`HTTPServerError`的写法是优于`HttpServerError`的。
- ``Capitalized_Words_With_Underscores``：大驼峰带下划线（丑死了）

还有一种使用短唯一前缀将相关名称分组的风格。在Python中并不常用，但为了完整起见提到它。
例如，`os.stat()` 函数返回一个元组，其中的元素传统上具有类似
`st_mode`、`st_size`、`st_mtime`等名称。
（这样做是为了强调与POSIX系统调用结构的字段对应关系，这有助于熟悉该结构的程序员。）

X11库在其所有公共函数前面使用前缀X。在Python中，这种风格通常被认为是不必要的，
因为属性和方法名带有对象前缀，而函数名带有模块名前缀。

此外，还识别以下使用前导或尾随下划线的特殊形式（这些通常可以与任何命名约定结合使用）：

- `_single_leading_underscore`：单下划线开头，弱一级的“内部使用”指示器。
  比方说`from M import *`不会导入那些以下划线开头的对象。

- `single_trailing_underscore_`：单下划线结尾，用于避免与Python名称冲突，例如：
  
  ```python
  tkinter.Toplevel(master, class_='ClassName')
  ```

- `__double_leading_underscore`：双下划线开头，当命名类属性时，会触发名称修饰
  （在类`FooBar`内，`__boo`会变成`_FooBar__boo`；参见下文）

- ``__double_leading_and_trailing_underscore__``：双下划线开头加结尾，
  存在于用户可控制的命名空间中的“魔术”对象或属性。
  例如，`init`、`import__`或`__file`。永远不要自己发明这样的名称；
  只按照文档使用它们。

### 规定性的命名约定

#### 不要使用的字符

永远不要使用字符 `l`、`O`或 `I`作为**单字符**变量名。

在某些字体中，这些字符与数字`1`和`0`难以区分。当想使用`l`时，请改用`L`。

#### ASCII兼容性

标准库中使用的标识符必须与 ASCII 兼容，如PEP 3131所述。

#### 包和模块名称

`module`

模块应具有简短的全小写名称。如果使用下划线可以提高可读性，可以在模块名称中使用下划线。
Python 包也应具有简短的全小写名称，尽管不推荐使用下划线。

当用C/C++编写的扩展模块具有相应的Python模块，
提供更高级别（例如更面向对象）的接口时，C/C++模块的名称以下划线开头（例如`_socket`）。

#### 类名称

`ClassName`

类名通常应遵循大驼峰式命名规则。

在接口已经有文档并且主要用作可调用对象的情况下，也可以使用函数的命名约定。

请注意，内置名称有单独的约定：大多数内置名称是单词（或两个单词连在一起），只有在异常名称和内置常量中才使用大驼峰式命名规则。

#### 类型变量名称

`CustomInt64`

在PEP 484中引入的类型变量的命名通常应使用大驼峰式命名规则，
优先选择简短的名称：`T`，`AnyStr`，`Num`。
建议在用于声明协变或逆变行为的变量上添加后缀`_co`或`_contra`：

```python
from typing import TypeVar

VT_co = TypeVar('VT_co', covariant=True)
KT_contra = TypeVar('KT_contra', contravariant=True)
```

#### 异常名称

`CustomNameConfictError`

因为异常应该是类，所以在这里应用类命名约定。
然而，如果异常实际上是错误，应该在异常名字上使用后缀“Error”。

#### 全局变量名

`GlobalVariable`

> 希望这些变量只在一个模块内使用

约定与函数的约定基本相同。

为了防止导出全局变量，设计用于通过`from M import *`方式使用的模块应使用`__all__`机制，
或者使用旧的约定，即给这些全局变量加上下划线前缀
（这样做可以表示这些全局变量是“模块非公开”的）。

#### 函数和变量名称

`function_a_b_c`

函数名应该使用小写字母，单词之间用下划线分隔以提高可读性。

变量名遵循与函数名相同的约定。

小驼峰式命名只允许在已经存在的风格中使用（例如，`threading.py`），
以保持向后兼容性。

#### 函数和方法参数

实例方法的第一个参数应始终使用`self`。

类方法的第一个参数应始终使用`cls`。

如果函数参数的名称与保留关键字冲突，通常最好在末尾添加一个下划线而不是使用缩写或拼写错误。
因此，`class_`比`clss`更好。（最好的做法是通过使用同义词来避免此类冲突。）

#### 方法名和实例变量

`method_a_b_c`

遵循函数命名规则：使用小写字母，使用下划线分隔单词以提高可读性。

仅在非公共方法和实例变量中使用一个前导下划线。

为了避免与子类发生名称冲突，使用两个前导下划线来调用Python的名称混淆规则。

Python会使用类名对这些名称进行混淆：如果类Foo有一个名为`__a`的属性，
就无法通过`Foo.__a`访问它（不过可以通过调用`Foo._Foo__a`强行访问）。
通常，双前导下划线应仅用于避免与设计为可被子类化的类中的属性发生名称冲突。

注意：对于使用`__names`的使用存在一些争议（见下文）。

#### 常量

`IMMUTABLE_CONSTANT`

常量通常在模块级别上定义，并使用全大写字母，用下划线分隔单词。例如``MAX_OVERFLOW``和``TOTAL``。

#### 设计继承性

始终决定一个类的方法和实例变量（统称为“属性”）是公共的还是非公共的。
如果不确定，选择非公共的；将公共属性变为非公共属性比将非公共属性变为公共属性更容易。

公共属性是你希望与你的类无关的客户端使用的属性，你承诺避免不向后兼容的更改。
非公共属性是不打算由第三方使用的属性；你不保证非公共属性不会发生更改，甚至被删除。

我们这里不使用`private`这个术语，因为在Python中没有真正的私有属性
（除非做一些通常不必要的工作）。

另一类属性是“子类API”的一部分（在其他语言中通常称为protected）。
有些类被设计为可被继承，可以扩展或修改类的行为。
在设计这样的类时，要仔细考虑哪些属性是公共的，哪些是子类API的一部分，
以及哪些只能由你的基类使用。

在这个基础上，以下是Pythonic的指南：

- 公共属性不应该有前导下划线。

- 如果你的公共属性名与保留关键字冲突，可以在属性名末尾添加一个单个下划线。
  这比使用缩写或拼写错误更好。
  （然而，尽管有这个规则，对于已知是类的变量或参数，
  特别是类方法的第一个参数，`cls`是首选的拼写方式。）

  注意1：参考上面关于类方法的参数名称建议。

- 对于简单的公共数据属性，最好只暴露属性名称，而不是复杂的访问器/修改器方法。
  请记住，Python提供了一个简单的路径来进行未来的增强，
  如果你发现一个简单的数据属性需要增加功能性的行为。
  在这种情况下，可以使用属性（properties）将功能实现隐藏在简单数据属性访问语法之后。

  注意1：尽量确保功能行为没有副作用，尽管缓存等副作用通常是可以接受的。

  注意2：避免将属性用于计算开销大的操作；属性表示使调用者认为访问（相对而言）是廉价的。

- 如果你的类被设计为可被子类化，并且你有一些不希望子类使用的属性，
  请考虑使用双前导下划线和没有尾随下划线的命名方式。
  这将调用Python的名称混淆算法，其中类的名称被混淆到属性名中。
  这有助于避免子类中意外包含具有相同名称的属性时的属性名称冲突。

  注意1：混淆的名称只使用简单的类名，因此如果子类选择相同的类名和属性名，
  仍然可能发生名称冲突。

  注意2：名称混淆可能会使某些用途（如调试和`__getattr__()`）变得不太方便。
  然而，名称混淆算法有很好的文档，并且可以手动执行。

  注意3：并非每个人都喜欢名称混淆。
  需要权衡避免意外名称冲突的需要与高级调用者的潜在使用需求。

### 公共和内部接口

任何向后兼容性保证仅适用于公共接口。因此，用户能够清楚地区分公共接口和内部接口非常重要。

除非文档明确声明其为临时或内部接口，并免除向后兼容性保证，
否则被记录的接口被视为公共接口。所有未记录的接口都应被视为内部接口。

为了更好地支持内省，模块应该使用`__all__`属性明确声明其公共API中的名称。
将`__all__`设置为空列表表示模块没有公共API。

即使设置了适当的`__all__`，内部接口（包、模块、类、函数、属性或其他名称）
仍应以单个前导下划线作为前缀。

如果包含命名空间（包、模块或类）被视为内部，则该接口也被视为内部接口。

导入的名称应始终被视为实现细节。除非它们是包含模块API的明确记录部分
（例如`os.path`或公开子模块功能的包的`__init__`模块），
否则其他模块不得依赖对这些导入名称的间接访问。

## 编程建议

- 代码应该以不对其他Python实现（如PyPy、Jython、IronPython、Cython、Psyco等）
  造成不利影响的方式编写。

  例如，不要依赖于CPython对形如`a += b`或`a = a + b`的语句中就地字符串拼接的高效实现。
  即使在CPython中，这种优化也很脆弱（它只适用于某些类型），
  而且在不使用引用计数的实现中根本不存在。在库的性能敏感部分，
  应该使用`"".join()`形式。这将确保在各种实现中拼接操作以线性时间进行。

- 对于像None这样的单例对象，比较应该始终使用`is`或`is not`运算符，而不是等值运算符。

  此外，注意在你真正意思是`if x is not None`时不要写成`if x`的形式——例如，
  在测试默认为None的变量或参数是否被设置为其他值时。
  其他值可能具有在布尔环境中为假的类型（如容器类型）！
  使用`is not`运算符而不是`not ... is`。
  虽然这两个表达式在功能上是相同的，但前者更易读，更受推荐:

  ```python
  # 正确
  if foo is not None:
  ```

  ```python
  # 错误
  if not foo is None:
  ```

- 当使用丰富比较进行排序操作时，最好实现所有六个操作
  （``__eq__``、``__ne__``、``__lt__``、``__le__``、``__gt__``、``__ge__``），
  而不是依赖其他代码只执行特定的比较。

  为了减少工作量，`functools.total_ordering()`装饰器提供了一个工具来生成缺失的比较方法。

  PEP 207表明Python假设了自反性规则。因此，解释器可能会将`y > x`替换为`x < y`，
  `y >= x`替换为`x <= y`，并且可能会交换`x == y`和`x != y`的参数。
  `sort()`和`min()`操作保证使用`<`运算符，而`max()`函数使用`>`运算符。
  然而，最好实现所有六个操作，以避免在其他上下文中引起混淆。

- 总是使用`def`语句而不是将`lambda`表达式直接绑定到标识符的赋值语句：
  
  ```python
  # 正确
  def f(x): return 2*x
  ```

  ```python
  # 错误
  f = lambda x: 2*x
  ```

  第一种形式意味着生成的函数对象的名称具体为`f`，而不是通用的`lambda`。
  这在追溯和字符串表示中通常更有用。
  使用赋值语句消除了`lambda`表达式相对于显式`def`语句所能提供的唯一好处
  （即它可以嵌入到更大的表达式中）。

- 异常应该从`Exception`而不是`BaseException`派生。
  直接继承`BaseException`保留给那些几乎总是不应该捕获的异常。

  设计异常层次结构应基于代码可能需要捕获异常的区分，而不是异常发生的位置。
  目标是以编程方式回答“出了什么问题？”而不仅仅是声明“发生了问题！”
  （参见PEP 3151，其中介绍了这个教训在内置异常层次结构中的应用示例）。

  类命名约定在这里也适用，但如果异常是错误，则应为异常类添加后缀`Error`。
  用于非局部流程控制或其他形式的信号传递的非错误异常不需要特殊后缀。

- 适当使用异常链。应该使用`raise X from Y`来表示明确的替换，而不丢失原始的追溯信息。

  当有意替换内部异常时（使用`raise X from None`），确保将相关细节传递给新的异常
  （例如，在将`KeyError`转换为`AttributeError`时保留属性名，
  或者将原始异常的文本嵌入到新的异常消息中）。

- 当尝试捕获异常时，使用特定的异常名称而非用一个`except:`语句捕获所有异常：

  ```python
  try:
      import platform_specific_module
  except ImportError:
      platform_specific_module = None
  ```

  一个单独的`except:`会捕获`SystemExit`和`eyboardInterrupt`异常，
  让程序无法用Control-C终止，并且也会掩盖其他问题。
  如果你想捕获所有的异常，请用`except Exception:`。

  限制使用单独的`except:`语句有以下好处：

  1. 如果异常处理程序能够印出完整的日志或者错误追踪，至少用户能知道有错误发生了。

  2. 如果代码代码需要进行一些清理工作，但又希望将异常向上传播，可以使用`raise`语句。
     但在这种情况下，使用`try...finally`可能是更好的处理方式。

- 在捕获操作系统错误时，推荐使用在 Python 3.3 中引入的显式异常层次结构，
  而不是通过检查`errno`值来进行判断。

- 另外，在所有的`try/except`语句中，将`try`子句限制在绝对必要的最小代码量上。
  这样可以避免隐藏错误：

  ```python
  # 正确
  try:
      value = collection[key]
  except KeyError:
      return key_not_found(key)
  else:
      return handle_value(value)
  ```

  ```python
  # 错误
  try:
      # 过于宽泛
      return handle_value(collection[key])
  except KeyError:
      # 会捕获由handle_value()抛出的KeyError
      return key_not_found(key)
  ```

- 当资源仅在代码的特定部分使用时，使用`with`语句确保在使用后及时且可靠地进行清理。
  也可以使用`try`/`finally`语句。

- 只有在上下文管理器除了获取和释放资源之外还执行其他操作时，
  才应通过单独的函数或方法来调用上下文管理器:

  ```python
  # 正确
  with conn.begin_transaction():
      do_stuff_in_transaction(conn)
  ```

  ```python
  # 错误
  with conn:
      do_stuff_in_transaction(conn)
  ```

  后一个示例没有提供任何信息来表明`__enter__`和`__exit__`
  方法除了在事务之后关闭连接之外还做其他操作。在这种情况下，明确表示是很重要的。

- 在返回语句中保持一致。要么函数中的所有返回语句都返回一个表达式，
  要么所有返回语句都不返回值。如果任何返回语句返回一个表达式，
  那么任何没有返回值的返回语句都应明确指明为`return None`，
  并且在函数末尾（如果可达）应该存在一个明确的返回语句。

  ```python
  # 正确

  def foo(x):
      if x >= 0:
          return math.sqrt(x)
      else:
          return None

  def bar(x):
      if x < 0:
          return None
      return math.sqrt(x)
  ```

  ```python
  # 错误

  def foo(x):
      if x >= 0:
          return math.sqrt(x)

  def bar(x):
      if x < 0:
          return
      return math.sqrt(x)
  ```

- 使用`''.startswith()`和`''.endswith()`而不是字符串切片来检查前缀或后缀。

  `startswith()`和`endswith()`更清晰且更不容易出错：

  ```python
  # 正确
  if foo.startswith('bar'):
  ```

  ```python
  # 错误
  if foo[:3] == 'bar':
  ```

- 在进行对象类型比较时，应始终使用`isinstance()`而不是直接比较类型：

  ```python
  # 正确
  if isinstance(obj, int):
  ```

  ```python
  # 错误
  if type(obj) is type(1):
  ```

- 对于序列（字符串、列表、元组），可以利用空序列为假的特性：
  
  ```python
  # 正确
  if not seq:
  if seq:
  ```

  ```python
  # 错误
  if len(seq):
  if not len(seq):
  ```

- 不要编写依赖于尾部空白的字符串字。这样的尾部空白在视觉上无法区分，
  而且某些编辑器（或者最近的reindent.py）会将其修剪掉。

- 不要使用`==`将布尔值与`True`或`False`进行比较：

  ```python
  # 正确
  if greeting:
  ```

  ```python
  # 错误
  if greeting == True:
  ```

  千万不要：

  ```python
  # 错误
  if greeting is True:
  ```

- 不建议在`try...finall`的`finally`块中使用流程控制语句
  `return`/`break`/`continue`，因为这些语句会跳过`finally`块，
  从而隐式地取消正在通过`finally`块传播的任何活动异常：

  ```python
  # 错误
  def foo():
      try:
          1 / 0
      finally:
          return 42
  ```

### 函数注解

随着PEP 484的接受，函数注解的风格规则已经发生了变化。

- 函数注解应该使用PEP 484的语法（在前面的部分中有一些关于注解的格式建议）。

- 以前在这个 PEP 中推荐的注解风格的实验不再鼓励。

- 然而，在标准库之外，现在鼓励在PEP 484规则下进行实验。
  例如，使用PEP 484风格的类型注解来标记一个大型第三方库或应用程序，
  审查添加这些注解的难度，并观察它们的存在是否增加了代码的可理解性。

- Python 标准库在采纳这样的注解时应保持保守，但允许在新代码和大规模重构中使用。

- 对于想要以不同方式使用函数注解的代码，建议在形式为的注释中加以说明：

  ```python
  # type: ignore
  ```

  将此行放在文件的顶部附近，这会告诉类型检查器忽略所有的注解。
  （有关如何在类型检查器中细粒度地禁用警告的方法，请参阅 PEP 484。）

- 类似于代码检查工具，类型检查器是可选的、独立的工具。
  Python 解释器默认情况下不应发出任何由于类型检查而引起的消息，
  并且不应根据注解改变其行为。

- 不想使用类型检查器的用户可以自由地忽略它们。
  然而，预计第三方库包的用户可能希望在这些包上运行类型检查器。
  为此，PEP 484 建议使用存根文件`.pyi`文件优先于相应的`.py`文件被类型检查器读取。
  存根文件可以与库一起分发，或者（在库作者的许可下）通过`typeshed`仓库单独分发。

### 变量注解

PEP 526引入了变量注解。对于变量注解的风格建议与上面描述的函数注解类似：

- 模块级变量、类变量和实例变量以及局部变量的注解在冒号后应有一个空格。

- 冒号前不应有空格。

- 如果赋值语句有右侧表达式，则等号两侧应有且仅有一个空格：

  ```python
  # 正确

  code: int

  class Point:
      coords: Tuple[int, int]
      label: str = '<unknown>'
  ```

  ```python
  # 错误

  code:int  # 冒号后无空格
  code : int  # 冒号前多空格

  class Test:
      result: int=0  # 等号两侧无空格
  ```

- 虽然PEP 526已被 Python 3.6 接受，
  但变量注解语法是所有 Python 版本上存根文件的首选语法（有关详细信息，请参阅PEP 484）。
