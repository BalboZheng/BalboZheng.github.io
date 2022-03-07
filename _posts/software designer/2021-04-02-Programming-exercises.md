---
layout:     post
title:      "程序语言习题"
subtitle:   "本章考点在综合知识考试平均分数为6分，约总分的8%"
date:       2021-04-02 13:44:05
author:     "Balbo"
header-img: "img/post-bg-2021.jpg"
tags:
    - software-designer
---

1. 下面关于编程语言的各种说法中，$\underline{D}$ 是不正确的。

   A. 逻辑型语言适用于书写自动定理证明

   ~~B~~. Smalltalk、C++、Java、C#都是面向对象语言

   C. 函数型语言适用于人工智能领域

   D. 由于 C 语言程序是由函数构成的，依次它是一种函数型语言

2. 传值调用和引用调用时常用的函数调用方式，下列描述正确的是$\underline{C}$。

   ~~A~~. 在传值调用方式下，是将形参的值传给实参

   B. 在传值调用方式下，形参可以是任意形式的表达式

   C. 在引用调用方式下，是将实参的地址传给形参

   D. 在引用调用方式下，实参可以是任意形式的表达式

3. 高级语言编译的过程可以分成若干个阶段，其中把单词符号分解成句子属于$\underline{B}$ 阶段的工作

   ~~A~~. 词法分析	B. 语法分析

   C. 语义分析	D. 代码生成

4. 有限自动机可分为确定的有限自动机和不确定的有限自动机。那么确定的有限自动机 A 于不确定的有限自动机 B 等价，则$\underline{B}$。

   A. A 与 B 的状态个数相等

   B. A 与 B 可识别的记号完全相同

   C. B 能识别的正规集是 A 所识别正规集的真子集

   D. A 能识别的正规集是 B 所识别正规集的真子集

5. 集合 $L=\lbrace a^mb^m\mid m\geq 0\rbrace\underline{D}$。

   A. 可用正规式 “$a^\ast b^\ast$” 表示

   B. 不能用正规式表示，但可用非确定的有限自动机识别

   C. 可用正规式 “$a^mb^m$” 表示

   D. 不能用正规式表示，但可用上下文无关法表示

6. 某以确定有限自动机（DFA）的状态转换图如图，与该 DFA 等价的正规式是$\underline{D}$。

   ![DFA transfont.png](https://i.loli.net/2021/04/02/HtTvjEwdl8QO4iI.png)

   A. $10\ast (0\mid 1)\ast$

   B. $((0\ast 0)\ast 1\ast)\ast$

   C. $1\ast ((0 \mid1)00)\ast$

   D. $(1\ast(01\ast 0)\ast)\ast$

7. 图所示为一确定有限自动机的转台转换图，图中的 $\underline{B}$ 是可以合并的状态。

   ![DFA transfont_1.png](https://i.loli.net/2021/04/02/p1htwxB5nT8W9u6.png)

   ~~A~~. 0 和 1	B. 2 和 3	C. 1 和 2	D. 0 和 3

8. 已知函数 f1()、f2() 的定义如下所示，设调用函数 f1 时传递给形参 x 的值是 10，若函数调用 f2(a) 以引用调用（Call By Reference)方式传递信息和以值调用（Call By Value）方式传递信息，则函数 f1 的返回值分别是$\underline{B}$。

   ```c
   f1 (int x){
       int a=x;
       f2(a);
       return a+x;
   }
   f2 (int y){
       y=5*y-1;
       return ;
   }
   ```

   ~~A~~. 20 和 20	B.. 59 和 20	C. 59 和 95	D. 20 和 95

9. 某一确定性有限自动化（DFA）的状态如图，则以下字符串中，不能被该 DFA 接受的是$\underline{C}$。

   F. 0010	S. 0001	T. 0101

   ![DFA transfont_2.png](https://i.loli.net/2021/04/02/MjANiPsaoGheTVF.png)

   A. F、S	~~B~~. F、T	C. S、T	D. F、S、T

10. 下列叙述中正确的是$\underline{D}$。

    A. 算法的时间、空间复杂性与实现该算法所采用的程序射击语言相关

    ~~B~~. 面向对象程序射击语言不支持对一个对象的成员变脸经行直接访问

    C. 与汇编语言相比，采用脚本语言编程可或i的更高的运行效率

    D. 面向对象程序设计语言不支持过程化的程序设计，只支持面向对象程序设计

11. 若程序中存在死循环，那么这属于$\underline{C}$错误。

    ~~A~~. 语法	B. 语用	C. 语义	D. 语境

12. 表达式 $a\ast(b+c)-d$ 的后缀表达式为 $\underline{B}$。

    ~~A~~. $abcd\ast+-$	B. $abc+\ast d-$	C. $abc\ast +d-$	D. $-+\ast abcd$

13. 已知某文法 $D[S]:S\to aSa,S\to b$，从 S 推导除的符号串可用$\underline{B}(n\geq 0)$ 描述。

    A. $(aba)^n$	 B. $a^nba^n$	C. $b^n$	D. $ab^na$

14. 对于正规式 $0\ast(010101)\ast 0$，其正规式中字符串的特点是$\underline{D}$。

    A. 开头和结尾必须是 0

    B. 1 必须出现奇数次

    ~~C~~ 0 不能连续出现

    D. 1 不能连续出现

15. 下面的代码段在运行中会出现 $\underline{D}$ 错误。

    ```c
    int	i=0;
    while(i<10);
    {i=i+1;}
    ```

    ~~A~~ 语法	B. 类型不匹配	C. 变量定义	D. 动态语义
