## 4.1 自顶向下的语法分析概述

从文法的开始符号推导出词串w。每一步推导都需要做两个选择：1、替换当前句型的哪一个非终结符；2、用该非终结符的哪个产生式进行替换。

自顶向下的语法分析采用最左推导的方式，选择每个句型的最左非终结符进行替换。并根据接下来的输入符号，选择最左非终结符的一个产生式进行推导。

<img src="chapter4 自顶向下的语法分析.assets/image-20210517191329119.png" alt="image-20210517191329119" style="zoom:50%;" />

当有多个左部相同的产生式时，若挨个试，可能有的产生式不能推出最终结果，需要回溯。

如果每次都能确定地选择唯一的产生式进行展开，称为确定的自顶向下的语法分析，通常需要向前看k个符号。

## 4.2 自顶向下的语法分析面临的问题与文法的改造

### 问题1：同一非终结符的多个候选式存在共同前缀，将导致回溯

提取左公因子，本质是延迟决定：

<img src="chapter4 自顶向下的语法分析.assets/image-20210517195700416.png" alt="image-20210517195700416" style="zoom:50%;" />

### 问题2：左递归文法会使得递归下降分析器陷入无限循环

左递归分为直接左递归和间接左递归

* 直接左递归：含有产生式$A→A\alpha$形式产生式的文法
* 间接左递归：经过两步以上能推出左递归形式的文法

消除直接左递归：

将$A→A\alpha|\beta$转换成$A→\beta A'\\A'\rightarrow\alpha A'|\varepsilon$，其中$\alpha \ne \varepsilon$，这两个文法都推出语言$\beta \alpha^*$

一般形式：

<img src="chapter4 自顶向下的语法分析.assets/image-20210517194234829.png" alt="image-20210517194234829" style="zoom:50%;" />

消除左递归付出的代价——引进了一些非终结符和$\varepsilon$产生式

消除间接左递归，一般是带入消除：

<img src="chapter4 自顶向下的语法分析.assets/image-20210517194616814.png" alt="image-20210517194616814" style="zoom:50%;" />

## 4.3 LL(1)文法

预测分析法的工作过程：从开始符号S出发，每一步根据当前句型的最左非终结符A和当前输入a，选择正确的A-产生式。为保证分析的确定性，选出的候选式必须是唯一的。

S文法（过于严格）：

* 每个产生式的右部都已非终结符开始
* 同一非终结符的各个候选式的首终结符都不同——不含$\varepsilon$产生式

使用空产生式的情况：非终结符A和当前的输入符号a不匹配时，若存在$A\rightarrow\varepsilon$，则可以通过检查a是否可能出现在A的后面，来决定是否使用空产生式。故引入FOLLOW集的概念。

FOLLOW(A)：可能紧跟在A后面的终结符a的集合，若A是某个句型的最右符号，则将结束符$添加到FOLLOW(A)中

产生式的可选集：指可以用该产生式进行推导时对应的输入符号的集合，记为${\rm SELECT}(A\rightarrow\beta)$

q文法：

* 每个产生式右部或以终结符开始，或为$\varepsilon$
* 左部相同的产生式有不相交的SELECT集
* $SELECT(A\rightarrow a\beta)={a}\\SELECT(A\rightarrow \varepsilon)=FOLLOW(A)$

对于更一般的情况，文法中产生式的右部可以以终结符开始，则$A\rightarrow\alpha$的可选集为：

* 若$\varepsilon\notin FIRST(\alpha)$，$SELECT(A\rightarrow\alpha)=FIRST(\alpha)$
* 若$\varepsilon\in FIRST(\alpha)$，$SELECT(A\rightarrow\alpha)=(FIRST(\alpha)-{\varepsilon})\cup FOLLOW(A)$

LL(1)文法：

文法G是LL(1)的，当且仅当G的任意两个具有相同左部的产生式$A\rightarrow \alpha|\beta$满足下面的条件：

* 不存在非终结符a使得这两个产生式都能推出以a开头的串，即FIRST(a)交FIRST($\beta$)为空
* $\alpha$和$\beta$至多有一个能推导出$\varepsilon$
* 若$\beta$推出$\varepsilon$，则FIRST(a)交FOLLOW(A)为空
    若$\alpha$推出$\varepsilon$，则FIRST($\beta$)交FOLLOW(A)为空

就是同一非终结符的各个产生式的可选集互不相交。

第一个L表示从左向右扫描，第二个L表示最左推导。1表示向前看1个符号来决定语法分析动作。

## 4.4 FIRST集和FOLLOW集的计算

${\rm FIRST}(\alpha)$被定义为可以从$\alpha$推导得到的串的首终结符的集合。若$\alpha\Rightarrow^*\varepsilon$，则$\varepsilon\in FIRST(\alpha)$。

FIRST集的应用：若有两个A的产生式$A→\alpha|\beta$，其中${\rm FIRST}(\alpha)$和${\rm FIRST}(\beta)$是两个不相交的集合，那么只需要查看下一个输入符号a，就可以在这两个产生式中做出选择。

### FIRST集的计算：

不断重复以下规则，直到没有新的符号被加入到任何FIRST集合中：

* 若X是终结符，则FIRST(X)=X；
* 若X是非终结符，有产生式$X→Y_1Y_2\dots Y_k$，若$Y_1Y_2\dots Y_{i-1}$能推出$\epsilon$，则将${\rm FIRST}(Y_1)$、${\rm FIRST}(Y_2)$、…、${\rm FIRST}(Y_{i})$加入到${\rm FIRST}(X)$中；若$Y_1Y_2\dots Y_k$都能推出空，则将$\epsilon$也加入到${\rm FIRST}(X)$中；

例子：

<img src="chapter4 自顶向下的语法分析.assets/image-20210519083924756.png" alt="image-20210519083924756" style="zoom:50%;" />

计算串$X_1X_2\dots X_n$的FIRST集合：

* 先加入$FIRST(X_1)$中所有非空符号。若$\varepsilon\in FIRST(X_1)$，再加入$FIRST(X_2)$中所有非空符号。以此类推。
* 若所有的$FIRST(X_i)$都包含非空符号，则将$\varepsilon$加到$FIRST(X_1X_2\dots X_n)$中

### FOLLOW集的计算：

不断重复以下规则，直到没有新的符号被加入到任何FOLLOW集合中：

* 将$放到FOLLOW(S)中，S是开始符号，\$是输入结束标记
* 若存在一个产生式$A\rightarrow \alpha B\beta$，那么将FIRST($\beta$)中除了空以外的符号放入FOLLOW(B)
* 若存在一个产生式$A\rightarrow \alpha B$，或存在$A\rightarrow \alpha B\beta$且FIRST($\beta$)包含空，则将FOLLOW(A)中的符号放入FOLLOW(B)中

例子：

<img src="chapter4 自顶向下的语法分析.assets/image-20210519085919030.png" alt="image-20210519085919030" style="zoom:50%;" />

计算出FIRST和FOLLOW集后，即可计算各产生式的SELECT集。若左部相同的产生式的SELECT集互不相交，则此文法是LL(1)文法，可以对其构造预测分析表：

<img src="chapter4 自顶向下的语法分析.assets/image-20210519090600107.png" alt="image-20210519090600107" style="zoom: 33%;" />

## 4.7 预测分析中的错误处理

两种错误：

* 栈顶的**终结符**和当前输入符号不匹配
* 栈顶**非终结符**与当前输入符号在预测分析表对应项中的信息为空

### 恐慌模式：

忽略输入中的一些符号，直到输入中出现了由设计者选定的**同步词法单元集合**中的某个词法单元

* 可以把FOLLOW(A)中的所有终结符放入非终结符A的同步集合
* 一直弹出输入符号，直到出现了同步词法单元，则将栈中的非终结符弹出

若终结符在栈顶而不能匹配，弹出此终结符。

### 带错误恢复的分析表的使用方法：

若M[A, a]是空，表示检测到错误，根据恐慌模式，**忽略输入符号a**

若M[A, a]是synch，弹出**栈顶的非终结符A**，继续分析后面的语法成分

若栈顶的终结符和输入符号不匹配，则弹出**栈顶的终结符**

例子如下，synch就是同步集合：

<img src="chapter4 自顶向下的语法分析.assets/image-20210521082520645.png" alt="image-20210521082520645" style="zoom:50%;" />