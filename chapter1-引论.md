# chapter1-引论

本课使用复杂数据结构。

静态，编译时确定；动态，运行时确定。一个特性特别好，为什么有的语言支持，有的语言不支持，因为不同语言有不同的应用场景，C、python。

抽象、形式化、文法，在文法上做数据变换。

—————————————————————————————————————————————————————

## 1.1 程序设计语言

机器语言，01；汇编语言，用助记符，和硬件紧密关联；高级语言，程序员编写的语言

强制式语言、申述式语言SQL、面向对象语言

编译：将高级语言翻译成汇编语言或机器语言的过程

## 1.2 程序设计语言的翻译

翻译程序：将一种语言描述的程序翻译成等价的另一种语言描述的程序的程序。
解释程序：一边解释一边执行，效率低
编译程序：将源程序完整地转换成机器语言程序或汇编语言程序，再处理、执行的翻译程序——no ud。
编译程序需要对不同的语言开发不同的目标程序，20x5；脚本只需要固定数量的解释器，**解释方式**解决了可移植性的问题，让程序在什么条件下都能运行——no ud

编译系统=编译程序+运行系统。编译语言的总结

## 1.3 编译程序总体结构

|                          | 输入                     | 输出     | 做的事                                                       |
| ------------------------ | ------------------------ | -------- | ------------------------------------------------------------ |
| 词法分析器               | 源程序(字符串)           | 单词符号 | 识别单词                                                     |
| 语法分析器               | 单词符号(单词串)         | 语法单位 | 组词成句，形成表达式                                         |
| 语义分析与中间代码生成器 | 语法单位(语法成分的串树) | 中间代码 | 1分析出做什么、按什么顺序做；<br />2合法检测，并用中间代码的形式表示出怎么做 |
| 代码优化器               | 中间代码                 | 中间代码 | 代码优化                                                     |
| 目标代码生成器           | 中间代码                 | 目标代码 | 生成目标代码                                                 |
| 符号表管理器             |                          |          | 管理标识符                                                   |
| 出错处理器               |                          |          | 有错误仍能继续编译，识别错误并处理                           |

### 词法分析：

从左到右扫描源程序字符串，识别出一个个单词，并转换成单词（记号—token）串；同时查词法错误。
输入字符串，输出（种别码，属性值）的序对，分析的依据是语言的词法规则，可以用自动机识别

<img src="https://i.loli.net/2021/03/14/A7N81fvHjaJmyKz.png#" alt="截屏2021-03-14 上午8.24.13" style="zoom: 50%;" />

其中实词，例如10、sum、num，需要加入符号表；符号不需要记，例如+、=。组词成句需要此序对和文法

### 语法分析器：

根据语法规则，将词组成各类语法成分，构造语法分析树，同时要指出语法错误
输入是单词符号序列，输出是语法成分（语法分析树），分析的依据是语言的语法规则，描述方式是上下文无关文法

<img src="https://i.loli.net/2021/03/14/FvEg1uCeMVYIPqG.png" alt="image-20210314083856922" style="zoom:50%;" />

### 语义分析与中间代码生成：

和语法分析同时进行，分析由语法分析器识别出来的语法成分的语义，并查找语义错误
输出是符号表的查、填和维护，分析依据是语义规则——not ud

### 中间代码优化：

生成：四元式表示、三地址码
中间代码的特点：简单规范、与机器无关、易于优化与转换
优化：对中间代码进行优化变换，更省空间、速度更快，分为与机器无关的优化、与机器有关的优化

**目标代码生成：**将中间代码转换成目标机器上的机器指令代码或汇编代码

**表格管理：**查、填

**错误处理：**运行错误检查、报告、纠正，及相关的续编译处理

## 1.4 编译程序的组织

编译程序可以对源程序或中间代码进行多遍扫描，并在每遍扫描中完成相应的任务。

遍和阶段**不一样**，一遍扫描可以完成一个或多个阶段的任务，一个阶段的任务也可能需要多遍扫描才能完成

## 1.5 编译程序的生成

T形图，表示语言翻译，实现语言（描述编译程序的语言）、源语言（被编译程序的描述语言）、目标语言（目标程序的描述语言）

T形图的下端表示编译程序本身的描述语言，上端表示该编译程序的**功能**：

### 自展

如何在一个机器上实现C语言编译器？

用汇编语言实现一个C子集的编译程序P0-人；

用汇编程序P1处理该程序，即将该程序转换为机器语言，得到P2-自动运行；

用C子集编写C语言的编译程序P3-人；

用P2编译P3，得到P4。

