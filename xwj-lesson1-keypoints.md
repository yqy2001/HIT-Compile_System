## 前言

整体架构——词法分析、语法分析、语义分析

画语法树，求短语、直接短语、句柄

题目：2004.4、2007.2.1

## 词法分析

token的表示：（种别码，属性值）

一符一码是用不到属性值的，哪些类是一符一码表示的？

一些单词的正则表达式、自动机是需要知道的：
标识符、八进制、十进制、十六进制

识别关键字和标识符的自动机是相同的，怎么处理

正则文法——自动机——程序

错误处理

## 自顶向下的语法分析

从开始符号进行展开推导

LL(k)各符号的意义

完整流程：

* 文法改造——消除回溯、左递归
* 求FIRST/FOLLOW
* 构造分析表（求SELECT）
* 根据分析表推导：id+id*id

对这个完整流程<font color='red'>逻辑要清晰</font>，前三步必考，最后一步可能考。

FIRST集是针对一个串的，FOLLOW集是针对一个非终结符的，SELECT集是针对一个产生式的。

LL(1)文法必考——改文法、求FIRST/FOLLOW、构造分析表(求SELECT)、依据分析表推导分析

题目：2004.6、2005.5、2006.5、2019.LL(1)