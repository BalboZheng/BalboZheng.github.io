---
layout:     post
title:      "数据结构（上）"
subtitle:   "数据结构基础知识"
date:       2021-03-14 10:37:54
author:     "Balbo"
header-img: "img/post-bg-2021.jpg"
tags:
    - software-designer
---

# 3.1 线性结构

## 3.1.1 线性表

1. 线性表的定义

   一个线性表是 $n(n\geq0)$ 个元素的有限序列，通常表示为 $(a_1,a_2,\cdots,a_n)$。非空线性表的特点

   1. 存在唯一的一个称作 “第一个” 的元素。
   2. 存在唯一的一个称作 “最后一个” 的元素。
   3. 除第一个元素外，序列中的每个元素均只有一个直接前驱。
   4. 出最后一个元素外，序列中的每个元素均有一个直接后驱。

2. 线性表的存储结构

   线性表的存储结构分为顺序存储和链式存储

   1. 线性表的顺序存储

      线性表的顺序存储是指用一组地址连续的存储单元一次存储线性表中的数据元素，从而使得逻辑上相邻的两个元素在屋里位置上也相邻。

      在顺序存储结构中，第 $i$ 个元素的 $a_i$ 的存储位置为 $LOC(a_i)=LOC(a_1)+(i-1)*L$，其中，$L$ 是表中每个数据原民俗所占空间的字节数。

      优点：可以随机存取表中的元素

      缺点：插入和删除操作需要移动元素

      因此

      1. 插入一个新元素需要移动的元素个数期望值 $E_{insert}$ 为

          $$E_{insert}=\sum_{i=1}^{n+1}P_i*(n-i+1)=\frac{1}{n+1}\sum_{i=1}^{n+1}(n-i+1)=\frac n2$$

         其中，$P_i$ 表示在表中的位置 $i$ 插入新元素的概率。

      2. 删除元素时时需要移动的元素个数期望值 $E_delete$ 为

         $$E_{delete}=\sum_{i=1}^{n}q_i*(n-i)=\frac 1n\sum_{i=1}^{n}(n-i)=\frac {n-1}{2}$$

         其中，$q_i$ 表示删除第 $i$ 个元素的概率

   2. 线性表的链式存储

      线性表的链式存储是用通过指针链接起来的结点来存储数据元素。

      存储各数据元素的结点的地址并不要求是连续的，因此存储数据元素的同时必须存储元素之间的逻辑关系。另外，结点空间只有在需要的时候菜申请，无须事先分配。

      结点之间通过指针域构成一个链表，若结点中只有一个指针域，则称线性链表/单链表

      设线性表中的元素是整型，则单链表结点类型的定义为

      ```c
      typedet struct node{
      		int data;            /*结点的数据域*/
      		struct node *next;   /*结点的指针域*/
      }NODE, *LinkList;
      ```

      在链式存储结构下进行出啊如和删除，其实都是对相关指针的修改。在单链表中，若在 $p$ 所指结点后插入新元素结点（$s$ 所指结点，已经生成），其基本步骤为：

      1. $s\to next=p\to next;$
      2. $p\to next=s;$

      即先将 $p$ 所指结点的后继结点指针赋给 $s$ 所指结点的指针域，然后将 $p$ 所指结点的指针域修改为向 $s$ 所指结点。

      同理，在单链表中删除 $p$ 所指结点的后继结点，其基本步骤为：

      1. $q=p\to next$
      2. $p\to next=p\to next\to next$
      3. $free(q)$

      即先令临时指针 $q$ 指向待删除的结点，然后修改 $p$ 所指结点的指针域为指向 $p$ 所指结点的后继的后继结点，从而将元素 $b$ 所在的结点从链表中删除，最后释放 $q$ 所指结点的空间。

      在单链表上查找、插入和删除运算的实现过程

      1. 查找运算

         ```c
         LinkList Find_List(LinkList L,int k) /*L为带头结点单链表的头指针*/
         /*在表中查找第k个元素，若找到。返回改元素结点的指针；否则。返回空指针 NULL*/
         {	LinkList p;	int i;
         	i = 1;p = L->next;			/*初始时，令p指向第一个元素结点，i为计数器*/
          	while(p && i<k) {			/*顺时针链向后查找，直到p指向第k个元素结点或p为空指针*/
                 p = p->next; i++;
             }
             if(p && i == k) return p;	/*存在第k个元素且指针p指向改元素结点*/
             return NULL;				/*第k个元素不存在，返回空指针*/
         } /*Find_List*/
         ```

         

      2. 插入运算

         ```c
         LinkList Insert_List(LinkList L,int k,int newElem) /*L为带头结点单链表的头指针*/
         /*该元素 newElem 插入表中的第k个元素之前，若成功则返回0，否则返回-1*/
         /*该插入操作等同于将元素 newElem 插入在第k-1个元素之后*/
         {	LinkList p,s;						/*p、s为临时指针*/
         	if(k == 1) p = L;					/*元素 newElem 要插入到第1个元素之前*/
          	else p = Find_List(L,k-1);			/*查找表中的第k-1个元素并令p指向该元素结点*/
          	if(!p) return -1;					/*表中不存在第k-1个元素，不满足运算要求*/
          	s = (NODE*)malloc(sizeof(NODE));	/*创建新元素的结点空间*/
          	if(!s) return -1;
          	s->data = newElem;
          	s->next = p->next;  p->next = s;	/*将元素 newElem 插入第k-1个元素之后*/
          	return 0;
         	} /*Insert_List*/
         ```

         

      3. 删除运算

         ```c
         LinkList Delete_List(LinkList L,int k) /*L为带头结点单链表的头指针*/
         /*删除表中的第k个元素结点，若成功则返回0，否则返回-1*/
         /*删除第k个元素相当于令第k-1个元素结点的指针域指向第k+1个元素所在结点*/
         {	LinkList p,q;					/*p、q为临时指针*/
          	if(k == 1) p = L;				/*删除的是第一个元素结点*/
          	else p = Find_List(L,k-1);		/*查找表中的第k-1个元素并令p指向该元素结点*/
          	if(!p||!p->next) return -1;		/*表中不存在第k个元素*/
         	q = p-next;						/*令q指向第k个元素结点*/
          	p->next = q->next;  free(q);	/*删除结点*/
          	return 0;
         } /*Delete_List*/
         ```

      优点：具有插入和删除操作不需要移动元素

      缺点：不能对数据元素惊醒随机访问

根据结点中指针域的设置方式，还有其他几种链表结构。

+ 双向链表。每个结点包含两个指针，分别指出当前元素的直接前驱和直接后驱。其特点是可以从表中任意的结点出发，从两个方向上遍历链表。
+ 循环链表。在单向链表（或双向链表）的基础上令表尾结点的指针指向链表的第一个结点，构成循环链表。其特点是可以从表中任意结点开始遍历整个链表。
+ 静态链表。借助数组来描述线性表的链式存储结构，用数组元素的下标表示元素所在结点的指针。

若双向链表中结点的 $front$ 和 $next$ 指针域分别指示当前结点的直接前驱和直接后驱，则在双向链表中插入结点 $^*s$ 时指针的变化情况：

1. $s\to front=p\to front;$
2. $p\to front\to next=s;$
3. $s\to next=p;$
4. $p\to front=s;$

在双向链表中删除结点时指针的变化情况：

1. $p\to front\to next=p\to next;$
2. $p\to next\to front=p\to front;free(p);$

## 3.1.2 栈和队列

1. 栈

   1. 栈的定义及基本运算

      1. 栈是只能通过访问它的一端来实现数据存储和检索的一种线性数据结构。换句话说，栈的修改是按先进后出的原则进行的。
      2. 栈的运算
         1. 初始化栈 InitStack(S)：创建一个空栈 S
         2. 判栈空 isEmpty(S)：当栈 S 为空时返回 ”真“，否则 ”假“
         3. 入栈 Push(S,x)：将元素 x 加入栈顶，并更新栈顶指针
         4. 出栈 PoP(S)：将栈顶元素从栈中删除，并更新栈顶指针。若需要得到栈顶元素的值，可将 PoP(S) 定义为一个返回栈顶元素值的函数。
         5. 读栈顶元素 Top(S)：返回栈顶元素的值，但不修改栈顶指针。

   2. 栈的存储结构

      1. 顺序存储。栈的顺序存储是指一组地址连续的存储单元一次存储自栈顶带栈底的数据元素，同时附设指针 top 指示栈顶元素的位置。采用顺序存储结构的栈也称为顺序栈。在这种存储方式下，需要预先定义（或申请）栈的存储空间，也就是说，栈空间的容量的有限的。因此，在顺序栈中，当一个元素入栈时，需要判断是否栈满（栈空间中没有空闲单元），若栈满，则元素不能入栈。

      1. 栈的链式存储。用链表作为存储结构的栈也称为链栈。由于栈中元素的插入和删除仅在栈顶一端进行，因此不必另外设置头指针，链表的头指针就是栈顶指针。
      2. 栈的应用。栈的典型应用包括表达式求值、口号匹配等，在计算机语言的实现以及将递归过程转变为非递归过程的处理中，栈由重要作用。

2. 队列

   1. 队列的定义及基本运算

      1. 队列的定义。队列是一种先进先出(First In First Out, FIFO)的线性表，它只允许在表的一端插入元素，而在表的另一端删除元素。在队列中，允许插入元素的一端称为队尾(Rear)，允许删除元素的一端称为对头(Front)。
      2. 队列的基本运算
         1. 初始化队列 InitQueue(Q)：创建一个空队列Q
         2. 判队空 isEmpty(Q)：当队列为空时返回 ”真“，否则 ”假“
         3. 入队 EnQueue(Q,x)：将元素 x 加入到队列 Q 的队尾，并更新队尾指针
         4. 出队 DelQueue(Q)：将队头元素从队列 Q 中删除，并更新队头指针
         5. 读栈顶元素 FrontQue(Q)：返回队头元素的值，但不更新队头指针。

   2. 队列的存储结构

      1. 队列的顺序存储。

         设顺序队列 Q 的容量为 6，其队头指针为 front，队尾指针为 rear，头、尾指针和队列元素之间的关系

         ![Queue.png](https://i.loli.net/2021/03/21/3pBPJw6vXU1rsTy.png)

         在顺序队列中，为了降低运算的复杂度，元素入队时只修改队尾指针，元素出队时只修改队头指针。由于顺序队列的存储空间容量时提前设定的，所以队尾指针会由一个上限值，当队尾指针达到该上限时，就不能只通过修改队尾指针来实现新元素的入队操作了。若将顺序队列假想成一个环状结构，则可维持入队、出队操作运算的简单性，称为循环队列。

         设循环队列 Q 的容量为 MAXSIZE，初始时队列为空，且 Q.rear 和 Q.front 都等于 0。

         1. 元素入队时，修改队尾指针 Q.rear = (Q.rear+1)%MAXSIZE。
         2. 元素出队时，修改队头指针 Q.front = (Q.front+1)%MAXSIZE。
         3. 根据队列操作的定义，当出队操作导致队列变为空时，则由 Q.rear==Q.front；当入队操作导致队列满，则 Q.rear==Q.front。
         4. 在队列空和队列满的情况下，循环队列的队头、队尾指向的位置时相同的，此时仅仅根据 Q.rear 和 Q.front 之间关系无法断定队列的状态。为了区别队空和队满的情况，有两种处理方式可采用：
            1. 设置一个标志，以区别头、尾指针的值相同队列时空还是满
            2. 牺牲一个存储单元，约定以“队列的尾指针所指位置的下一个位置是队头指针时”表示队列满

         ```c
         #define MAXQSIZE 100
         typedef struct {
         	int *base;		/*循环队列的存储空间，假设队列中存放的时整型数*/
         	int front;		/*指示队头，称为队头指针*/
         	ont rear;		/*指示队尾，称为队尾指针*/
         }SqQueue;
         ```

         ```c
         int InitQueue(SqQueue *Q)
         /*常见容量为 MAXQSIZE 的空队列，若成功返回0，否则-1*/
         {	Q->base = (int*)malloc(MAXQSIZE*sizeof(int));
         	if(!Q->base) return -1;
         	Q->front = 0; Q->rear = 0; return 0;
         }/*InitQueue*/
         ```

         ```c
         int EnQueue(SqQueue *Q,int e) /*元素e入队，若成功返回0，否则-1*/
         {	if((Q->rear+1)%MAXQSIZE == Q->front) return -1;
         	Q->base[Q->rear] = e;
         	Q->rear = (Q->rear + 1)%MAXQSIZE;
         	return 0;
         }/*EnQueue*/
         ```

         ```c
         int DelQueue(SqQueue *Q,int *e)
         /*若队列不空，则删除队头元素，有参数e带回其值并返回0，否则返回-1*/
         {	if(Q->rear == Q->front) return -1;
         	*e = Q->base[Q->front];
         	Q->front = (Q->front + 10)%MAXQSIZE;
         	return 0;
         }/*DelQueue*/
         ```

         

      2. 队列的链式存储。

         队列的链式存储也称为链队列。这里为了便于操作，给链队列添加一个头结点，并令头指针指向头结点。因此，队列为空的判定条件时头指针和为指针的值相同，且均指向头结点。

      3. 队列的应用。

         队列结构仓用于处理需要排队的场合，例如操作系统中处理打印任务的打印队列、离散事件的计算机模拟等。

## 3.1.3 串

串（字符串）时一种特殊的线性表，其数据元素为字符。计算机中非数值问题处理的对象经常时字符串数据。

1. 串的定义及基本运算

   1. 串的定义

      串是仅由字符构成的有限序列，是一种线性表。一般记为 $S='a_1a_2\cdots a_n'$，其中，$S$ 是串名，单引号括起来的字符序列是串值。

   2. 串的几个基本概念

      + 空串：长度为零的串称为空串，空串不包含任何字符
      + 空格串：由一个或多个空格组成的串。
      + 子串：由串中任意长度的连续字符构成的序列称为子串。含有子串的串称为主串。子串在主串中的位置是指子串首次出现时，该子串的第一个字符在主串中的位置。空串是任意串的子串。
      + 串相等：指两个串长度相等且对应序号的字符也相同
      + 串比较：两个串比较大小时以字符的 $ASCII$ 码值（或其他字符编码集合）作为依据。实质上，比较操作从两个串的第一个字符开始进行，字符的码值大者所在的串为大；若其中一个串线结束，则以串长较大者为大。

   3. 串的基本操作

      1. 赋值操作 StrAssign(s,t)：将串 s 的值赋给串 t。
      2. 连接操作 Concat(s,t)：将串 t 接续在串 s 的尾部，形成一个新串。
      3. 求串长 StrLrngth(s)： 返回串 s 的长度。
      4. 串比较 StrCompare(s,t)：比较两个串的大小。
      5. 求子串 SubString(s.start,len)：返回串 s 中从 start 开始的、长度为 len 的字符序列。

2. 串的存储结构

   串可以进行顺序存储或链式存储

   1. 串的顺序存储结构。串的顺序存储结构是值用一组地址连续的存储单元来存储串值的字符序列。
   2. 串的链式存储结构。当用链表存储串中的字符时，每个结点中可以存储一个字符，也可以存储多个字符，此时要考虑存储密度问题。

3. 串的模式匹配

   子串的定位操作通常称为串的模式匹配，它是各种串处理系统中最只要的运算之一。子串也称为模式串。

   1. 朴素的模式匹配算法

      该算法也称为布鲁特——福斯算法，其基本思想是从主串的第一个字符起与模式串的第一个字符比较，若相等，则继续逐一对字符进行后续的比较，否则从主串第二个字符起与模式串的第一个字符重新比较，直到模式串中每个字符一次和主串中一个连续的字符序列相等时为止，此时称为匹配成功。如果不能在主串中找到与模式串相同的子串，则匹配失败。

      ```c
      /*以字符数组存储字符串。实现朴素的模式匹配算法*/
      int Index(char S[],char T[],int pos)
      /*查找并返回模式串T在主串S中从pos开始的位置（下标），若T不是S的子串，则返回-1*/
      /*模式串T和主串S第一个字符的小标为0*/
      {	int i,j,slen,tlen;
      	i = pos; j = 0;						/*i、j分别用于指出主串字符和模式串字符的位置*/
      	slen = strlen(S); tlen = strlen(T);	  /*计算主串和模式串的长度*/
      	while(i<slen && j<tlen) {
      		if(S[i] == T[i]) {i++; j++;}
      		else{
      			i = i- j + 1;				/*主串字符的位置指针回退*/
      			j = 0;
      		}
      	}/*while*/
      	if(j >= tlen) return i - tlrn;
      	return -1;
      }/*Index*/
      ```

      

   2. 改进的模式匹配算法

      改进的模式匹配算法又称为 KMP 算法，其改进之处在于：每当匹配过程中出现相比较的字符不相等时，不需要回退主串的字符位置指针，而是利用已经得到的“部分匹配”结果将模式串向右“滑动”尽可能远的地方，在继续进行比较。

      <font color="red">设模式串为 “$p_0\cdots p_{m-1}$"，KMP 匹配算法的思想时：当模式串中的字符 $p_j$ 与主串中相应的字符 $S_i$ 不相等时，因其前 j 个字符("$p_0\cdots p_{j-1}$")已经获得了成功的匹配，所以若模式串中的 "$p_0\cdots p_{k-1}$" 与 "$p_{j-k}\cdots p_{j-1}$" 相同，这时可令 $p_k$ 与 $S_i$ 进行比较，从而使 i 无需回退。</font>

      ```c
      void Get_next(char *p,int next[])		/*求模式串p的next函数值并存入数组next*/
      {	int i,j,slen;
      	slen = strlen(p); i = 0;
      	next[0] = -1; j = -1;
      	while(i < slen) {
      		if(j == -1 || p[i] == p[j]) {++i;++j;next[i] = j;}
      		else j = next[i];
      	}/*while*/
      }/*Get_next*/
      ```

      ```c
      int Index_KMP(char *s,char *p,int pos,int next[])
      /*利用模式串p的next函数，求p在主串s中从第pos个字符开始的位置*/
      /*若匹配成功。返回模式串在主串中的位置（下标），否则返回-1*/
      {	int i,j,slen,plen;
      	i = pos-1;
      	j = -1;
      	slen = strlen(s); plen = strlen(p);
      	while(i < slen && s[i] == p[j]) {
      		if(j == -1 || s[i] == p[j]) {++i; ++j;}
      		else j = next[j];
      	}/*while*/
      	if(j >= plen) return i - plen;
      	else return -1;
      }/*Index_KMP*/
      ```

      

# 3.2 数组、矩阵和广义表

## 3.2.1 数组

1. 数组的定义及基本运算

   1. 数组的定义

      数组使定长线性表在维数上的扩展，即线性表中的元素又是一个线性表。n维数组是一种”同构“的数据结构，其每个数组元素类型相同、结构一致。

      数据结构的特点：

      1. 数据元素数目固定，一旦定义了一个数组结构，就不再有元素个数的增减变化。
      2. 数据元素具有相同的类型。
      3. 数据元素的下标关系具有上下界的约束且下标有序。

   2. 数组的两个基本运算

      1. 给定一组下标，存取相应的数据元素。
      2. 给定一组下标，修改相应的数据元素中某个数据项的值。

2. 数组的顺序存储

   数组一般不做插入和删除运算，一旦定义了数组，则结构中的数据元素个数和元素之间的关系就不再发生变动，因此数组适合于采用顺序存储结构。

   二维数组的存储结构可分为以行为主序很大以列为主序的两种方法

   设每个数据元素占用L个单元，$m、n$ 为数组的航叔和列数，$Loc（a_{11})$ 表示元素 $a_{11}$ 的地址，那么以行为主序优先存储的地址计算公式为：

   $$Loc(a_{ij})=Loc(a_{11})+((i-1)*n+(j-1))*L$$

   同理，以列为主序优先存储的地址计算公式为：

   $$Loc(a_{ij})=Loc(a_{11})+((j-1)*m+(i-1))*L$$

## 3.2.2 矩阵

矩阵式很多科学与过程计算领域研究的数学对象。在数据结构中，主要讨论如何在节省存储空间的情况下使矩阵的各种运算能搞笑地进行。

1. 特殊矩阵

   若矩阵中元素（或非0元素）的分布有一定的规律，则称为特殊矩阵。常见的特殊矩阵有对称矩阵、三角矩阵和对焦矩阵等。对于特殊矩阵，由于其非0元素的发布有一定的规律，所以可将其压缩存储在一维数组中,并建立起每个非0元素在矩阵中的位置与其在一维数组中的位置之间的对应关系

   若矩阵 $a_{n*n}$ 中的元素特点为 $a_{ij}=a_{ji}(1\leq i,j\leq n)$，则称为 n 阶对称矩阵

   若对称矩阵中的每一对元素仅占用一个存储单元，那么可将 $n^2$ 个元素压缩存储到能存放 $n(n+1)/2$ 个元素的存储空间中。不失一般性，以下为主序存储下三角（包括对角线）中的元素。假设以一维数组 $B[n(n+1)/2]$ 作为 n 阶堆成矩阵 A 中元素的存储空间，则 $B[k](1\leq k<n(n+1)/2) 与矩阵元素 a_{ij}(a_{ji})$ 之间存在着一一对应的关系：

   $$ k= \begin{cases} \frac{i(i-1)}{2}+j, & 当 i\geq j \\ \frac{j(j-1)}{2}+i, & 当i<j \end{cases} $$ 

   对角矩阵是指矩阵中的非 0 元素都集中在主对焦线为中心的带状区域中，即处理主对角线上和直接在对角线上、下方若干条对角线上的元素外，其余的矩阵元素都为 0。一个 $n$ 阶的三对角矩阵如图

   $$A_{n*n}=\begin{bmatrix} a_{1,1}&a_{1,2} \\ a_{2,1}&a_{2,2}&a_{1,3}&&&0 \\ &a_{3,2}&a_{3,3}&a_{3,4} \\ &&&\cdots&\cdots&\cdots& \\ &&&a_{i,i-1}&a_{i,i}&a_{i,i+1} \\ &&&&\cdots&\cdots&\cdots \\ &&&&&a_{n,n-1}&a_{n,n} \\ \end{bmatrix} $$

   若以行为主序将 $n$ 阶三对角矩阵 $A_{n*n} 的非0元素 a_{ij} 存储在一维数组 B[k](1\leq k\leq 3*n-2)$ 中，则元素位置之间的对应关系为$$k=3*(i-1)-1+j-i+1+1=2i+j-2(1\leq i,j\leq n)$$

2. 稀疏矩阵

   在一个矩阵中，若非0元素的个数远远少于 0 元素的个数，且非0元素的发布没有规律，则称之为稀疏矩阵。对于稀疏矩阵，存储非0元素时必须同时存储其位置，所以三元组 $(i,j,a_{ij})$ 可唯一确定矩阵 A 中的一个元素。由此，一个稀疏矩阵可由表示非0元素的三元组机器行、列数唯一确定。如图，其三元组表为 $((1,2,12),(1.3,9),(3,1,-3),(3,6,14),(4,3,24),(5,2,18),(6,1,15),(6,4,-1))$。

   $$ M_{6*7}=\begin{bmatrix} 0&12&9&0&0&0&0 \\ 0&0&0&0&0&0&0& \\ -3&0&0&0&0&14&0 \\ 0&0&24&0&0&0&0& \\ 0&18&0&0&0&0&0 \\ 15&0&0&-7&0&0&0  \end{bmatrix} $$ 

## 3.2.3 广义表

广义表时线性表的推广，是由0个或多个单元素或子表组成的有限序列。

广义表与线性表的区别在于：线性表的元素都是结构上不可分的单元素，而广义表的元素既可以是单元素，也可以是由结构的表

一般记为 $LS=(a_1,a_2,\cdots,a_n)$

其中，$a_i(1\leq i\leq n)$既可以是单个元素，又可以是广义表，分别称为原子和子表。

广义表的长度是指广义表中元素的个数。广义表的深度是指广义表展开后所含的看好的最大层数。

1. 广义表的基本操作

   1. 取表头 head(LS)。非空广义表 LS 的第一个元素称为表头，它可以是一个单元素，也可以是一个子表
   2. 取表尾 tail(LS)。在非广义表中，除表头元素之外，由其余元素所构成的表称为标表尾。非空广义表的表尾必定是一个表。

2. 广义表的特点

   1. 广义表可以是多层次的结构，因为广义表的元素可以是子表，而子表的元素还可以是子表
   2. 广义表中的元素可以是已经定义的广义表的名字，所以一个广义表可被其他广义表所共享
   3. 广义表可以是一个递归的表，即广义表中的元素也可以是本广义表的名字

3. 广义表的存储结构

   由于广义表中的元素本身由可以具有结构，它是一种带有层次的非线性结构，因此难以用顺序存储结构表示，通常采用链式存储结构。由上面的讨论可知，若广义表不空，则可分解为表头和表尾两部分。反之，一对确定的表头和表尾可唯一确定一个广义表。

# 3.3 树

## 3.3.1 树与二叉树的定义

1. 树的定义

   树是 $n(n\geq 0)$ 个结点的有限集合，当 $n=0$ 时称为空树。在任一非空树中，有且仅有一个称为根的结点；其余结点可分为 $m(m\geq 0)$ 个互不相交的有限子集 $T_1,T_2,\cdots,T_m$，其中，每个 $T_i$ 又都是一棵树，并且称为根结点的子树。

2. 树的基本概念

   1. 双亲、孩子和兄弟。结点的子树的根称为该结点的孩子；相应地，该结点称为其子结点的双亲。具有相同双亲的结点互为兄弟。
   2. 结点的度。一个结点的子树的个数记为该结点的度。
   3. 叶子结点。叶子结点也称为终端结点，指度为 0 的结点。
   4. 内部结点。度不为 0 的结点，也称为分支结点或非终端结点。除根结点以外，分支结点也称为内部结点。
   5. 结点的层次。根为第一层，根的孩子为第二层，以此类推，若某结点在第i层，则其孩子结点在第 $i+1$ 层。
   6. 树的高度。一棵树的最大层数记为树的高度/深度。
   7. 有序(无序)树。若将树中结点的各子树看成是从左到右具有次序的，即不能交换，则称该树为有序树，否则无序树。

3. 二叉树的定义

   二叉树是 $n(n\geq 0)$ 各结点的有限集合，它或者是空树 $(n=0)$，或者是由一个根系欸但及两棵不相交的且分别称为左、右子树的二叉树所组成。可见，二叉树同样具有递归性质。

   需要特别注意的是，尽管树和二叉树的概念之间由许多联系，但它们是两个不同的概念。树和二叉树之间最主要的区别是：二叉树中结点的子树要区分坐子树和右子树，即使在结点只由一棵子树的情况下，也要明确指出该子树是左子树还是右子树。另外，二叉树结点最大度为2，而树中不限制结点的度数。

## 3.3.2 二叉树的性质与存储结构

1. 二叉树的性质

   1. 二叉树第 $i$ 层 $(i\geq 1)$ 上最多有 $2^{i-1}$ 个结点
   2. 高度为 $k$ 的二叉树最多有 $2^{k}-1$ 个结点 $(k\geq 1)$
   3. 对于任何一棵二叉树，若其终端结点树为 $n_0$，度为 2 的结点树为 $n_2$，则 $n_0=n_2+1$
   4. 具有 $n$ 个结点的完全二叉树的深度为 $\lfloor\log_2n\rfloor+1$

   若深度为 $k$ 的二叉树有 $2^{k}-1$ 个结点，则称其为满二叉树。可以对满二叉树中的结点进行连续编号：约定编号从根结点其，自上而下、自左向右一次进行。深度为 $k$、有 $n$ 个结点的二叉树，当且仅当其每一个结点都与深度为 $k$ 的满二叉树中编号从 1 至 $n$ 的结点一一对应时，称之为完全二叉树。

   在以一个高度为 $h$ 的完全二叉树中，处理第 $h$ 层（即最后一层），其余各层都是满的。在第 $h$ 层上的结点必须从左到右一次放置，不能留空。

2. 二叉树的存储结构

   1. 二叉树的顺序存储结构

      顺序存储是用一组地址连续的存储单元存储二叉树中的结点，必须把结点排成一个适当的线性序列，并且系欸但在这个序列中的相互位置能放映出结点之间的逻辑关系。

      假设有编号为 $i$ 的结点，则有

      + 若 $i=1$，则该结点为根结点，无双亲；若 $i>1$，则该结点的双亲为 $\lfloor i/2\rfloor$
      + 若 $2i\leq n$，则该结点的左孩子编号为 $2i$，否则无左孩子
      + 若 $2i+1\leq n$，则该结点的右孩子编号为 $2i+1$，否则无右孩子

      显然，完全二叉树采用顺序存储结构既简单有节省空间，对于一般的二叉树，则不宜采用顺序存储结构。因为一般的二叉树也必须按照完全二叉树的形式存储，也就是要添上一些实际并不存在的”虚结点“，这将造成空间的浪费。

      | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   |
      | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
      | A    | B    | C    | D    | E    | F    | G    | H    | I    |      |
      <center>a.完全二叉树</center>

      | 1    | 2    | 3           | 4    | 5           | 6           | 7           | 8           | 9    | 10   |
      | ---- | ---- | ----------- | ---- | ----------- | ----------- | ----------- | ----------- | ---- | ---- |
      | A    | B    | $\emptyset$ | C    | $\emptyset$ | $\emptyset$ | $\emptyset$ | $\emptyset$ | D    |      |
      <center>b.一般二叉树</center>

      在最坏情况下，一个深度为 $k$ 且只有 $k$ 各结点的二叉树（单支树）需要 $2^{k}-1$ 个存储单元

   2. 二叉树的链式存储结构

      由于二叉树的接待你中包含有数据元素、左子树的根、右子树的根及双亲等信息，一次可以用三叉链表或二叉链表（即一个结点含有 3 个指针或两个指针）来存储二叉树，链表的头指针指向二叉树的根结点。

      二叉链表的结点类型定义为

      ```c
      typedef struct BiTnode{
      	int data;
      	struct BiTnode *lchild,*rchild;
      }BiTnode, *Bitree;
      ```

      在不同的存储结构中，实现二叉树的运算方法也不同，具体应采用什么存储结构，除考虑二叉树的形态外还应考虑需要进行的运算特点。

## 3.3.3 二叉树的遍历

```c
/*二叉树的先序遍历*/
void PreOrder(BiTree root){
	if(root != NULL){
		printf("%d",root->data);		/*访问根结点*/
		PreOrder(root->lchild);			/*先序遍历根结点的左子树*/
		PreOrder(root->rchild);			/*先序遍历根节点的右子树*/
	}/*if*/
}/*PreOrder*/
```

```c
/*二叉树的中序遍历*/
void InOrder(BiTree root){
	if(root != NULL){
		InOrder(root->lchild);			/*中序遍历根结点的左子树*/
		printf("%d",root->data);		/*访问根节点*/
		InOrder(root->rchild);			/*中序遍历根节点的右子树*/
	}/*if*/
}/*InOrder*/
```

```
/*二叉树的后序遍历*/
void PostOrder(BiTree root){
	if(root != NULL){
		PreOrder(root->lchild);			/*后序遍历根结点的左子树*/
		PreOrder(root->rchild);			/*后序遍历根节点的右子树*/
		printf("%d",rppt->data);		/*访问根结点*/
	}/*if*/
}/*PostOrder*/
```

![先序遍历](https://img-blog.csdn.net/20150711163642225)

<center>先序遍历</center>

![](https://img-blog.csdn.net/20150711163947878)

<center>中序遍历</center>

![](https://img-blog.csdn.net/20150711164230063)

<center>后序遍历</center>

遍历二叉树的基本操作就是访问结点，不论按照那种次序遍历，对于含有 $n$ 个结点的二叉树，遍历算法的时间复杂度为 $O(n)$。

借助于一个栈，可将二叉树的递归遍历算法转换为非递归算法。

```c
/*二叉树的中序非递归遍历算法*/
int InorderTraverse(BiTree root)		/*二叉树的非递归中序遍历算法*/
{	BiTreebp;
	InitStack(St);						/*创建一个空栈*/
	p = root;							/*p指向树根结点*/
	while(p != Null || !isEmpty(St)){
		if(p != NULL)					/*不是空树*/
		{
			Push(St, p);				/*根结点指针入栈*/
			p = p->lchild;				/*进入根的左子树*/
		}
		else {
			q = Top(St); Pop(St);		/*栈顶元素出栈*/
			printf("%d", q->data);		/*访问根结点*/
			p = q->rchild;				/*进入根的右子树*/
		}/*if*/
	}/*while*/
}/*InOrederTraverse*/
```

遍历二叉树的过程实质上是按一定的规则将树中的结点拍成一个线性序列的过程，因此遍历操作得到的是树中结点的一个线性序列。在每一个序列中，有且仅有一个起始点和一个终结点，其余结点有且仅有唯一的直接前驱和直接后继。显然，关于结点的前驱和后继的讨论是针对某一个遍历序列而言的。

对二叉树还可以进行层序遍历。设二叉树的根结点所在的层数为 1，层序遍历就是从树的根结点出发，首先访问第一层的树根结点，然后从左到右一次访问第二层上的结点，其次是第三层上的结点，以此类推，自下而上、自左至右逐层访问数中各结点的过程就是层序遍历。

```c
coid LevelOrder(BiTree root)				/*二叉树的层序遍历算法*/
{	BiTree p;
	InitQueue(Q);							/*创建一个空队列*/
	EnQueue(Q, root)						/*将根指针加入队列*/
	while (!isEmpty(Q)){					/*队列不空*/
		DeWueue(Q, p);						/*队头元素出队，并使p去队头元素的值*/
		printf("%d", p->data);				/*访问结点*/
		if(p->lchild) EnQueue(p->lchild);
		if(p->rchild) EnQueue(P->rchild);
	}/*while*/
}/*LevelOrder*/
```



## 3.3.4 线索二叉树

1. 线索二叉树的定义

   二叉树的遍历实质上是对一个非线性结构进行线性化的过程，它使得每个结点（除第一个和最后一个）在这些线性序列中有且仅有一个直接前驱和直接后继。但在二叉链表存储结构中，只能找到一个结点的左、右孩子，不能直接得到结点在任一遍历序列中的前驱和后继，这邪恶星系只有在遍历的动态过程中才能得到，因此，引入线索二叉树来保存这些到那个动态过程得到的信息。

2. 建立线索二叉树

   为了保存结点在任一序列终端前驱和后继信息，可以考虑在每个结点中增加两个指针域来存放遍历时得到的前驱和后继信息，这样就可以为以后访问带来方便。但增加指针信息会降低存储空间的利用率，因此可考虑其他方法。

   若 $n$ 各结点的二叉树采用二叉链表做存储结构，则链表中必然右 $n+1$ 各空指针域，可以利用这些空指针域来存放结点的前驱和后继信息。为此，需要在结点中增加标志 $ltag$ 和 $rtag$，以区分孩子指针的指向。

   $$
   ltag =
           \begin{cases}
           0  & \text{lchild域指示结点的左孩子} \\
           1 & \text{lchild域指示结点的直接前驱}
           \end{cases}
   $$

   $$
   rtag =
           \begin{cases}
           0  & \text{rchild域指示结点的右孩子} \\
           1 & \text{rchild域指示结点的直接后继}
           \end{cases}
   $$

   若二叉树的二叉链表采用以上的结点结构，则相应的链表称为线索链表，其中指向结点前驱、后继的执政称为线索。加上先岁哦的二叉树称为线索二叉树。对二叉树以某种次序遍历使其称为线索二叉树的过程称为线索化。

   那么如何进行线索化呢？按某种次序将二叉树线索化，实质上是在遍历过程中用线索取代空指针。因此，若设指针 p 指向正在访问的接待你，则遍历时设立一个指针 pre，使其始终指向刚刚访问过的结点（即 p 所示结点的前驱结点），这样就记下了遍历过程中系欸但被访问的先后关系。

   在遍历过程中，设指针p指向正在访问的结点

   1. 若p所指向的结点有空指针域，则将相应的标志域置为1
   2. 若 pre!=NULL 且 pre 所指结点的 rtag 等于 1，则令 pre->rchild=p。
   3. 若 p 所指向结点的 ltag 等于 1，则令 p->lchild=pre
   4. 使 pre 指向刚刚访问过的1结点，即令 pre=p

   用这种方法得到的线索二叉树，其线索并不完整。也就是说，部分结点的前驱或后继信息还需要通过进一步的运算得到

3. 访问线索二叉树

   以中序线索二叉树为例，令 p 指向数中的某个结点，查找 p 所指系欸但的后继结点的方法：

   1. 若 p->rtag==1，则 p->rchild 即指向其后继结点
   2. 若 p->rtag==0，则 p 所指结点的中序后继必然使其右子树中进行中序遍历得到的第一个结点。

## 3.3.5 最优二叉树

1. 最优二叉树

   最优二叉树又称哈夫曼树，它是一类带权路径长度最短的树。路径是从树中一个结点到另一个结点之间的通路，路径上的分支数目称为路径长度。

   树的路径长度是从树根到每一个叶子之间的路径长度之和。结点的带权路径长度为从该结点到树根之间的路径长度与该结点权值的乘积

   树的带权路径长度为树中所有叶子结点的带权长度之和，记为

   $$WPL=\sum_{k=1}^n w_kl_k$$

   其中，n为带权叶子结点数目，$w_k$ 为叶子结点的权值，$l_k$ 为叶子结点到根的路径长度

   那么，如何构造最优二叉树？

   1. 根据给定的 $n$ 个权值 $\lbrace w_1,w_2,\cdots,w_n\rbrace$，构成 n 棵二叉树的集合 $F=\lbrace T_1,T_2,\cdots,T_n\rbrace$，其中每棵树 $T_i$ 中只有一个带权为 $w_i$ 的根结点，其左、右子树均为空
   2. 在 $F$ 中选取两棵权值最小的树作为左、右子树构造一棵树的二叉树，置新构造二叉树的根结点的权值为其左、右子树根结点的权值之和
   3. 从 $F$ 中删除这两棵树，同时将新得到的二叉树加入到 $F$ 中
   4. 重复2、3步，直到 $F$ 中只含一棵树为止，这棵树就是最优二叉树

   由此算法可知，以选中的两棵子树构成新的二叉树，哪个作为左子树，哪个作为右子树，并没有明确。所以，具有 $n$ 个叶子结点的权值为 $w_1,w_2,\cdots,w_n$ 的最优二叉树是不唯一的，但其 $WPL$ 值是唯一确定的。

   ```c
   #define MAXLEAFNUM 50					/*最优二叉树中的最多叶子数目*/
   typedef struct node {
   	char ch;							/*结点表示的字符，对于非叶子结点，此域不用*/
   	int weight;							/*结点的权值*/
   	int parent;							/*结点的父结点的下标，为0时表示无父结点*/
   	int lchild,rchild;					/*结点的左、右孩子结点的下标，为0时表示无孩子结点*/
   }Huffman Tree[2*MAXLEAFNUM];
   typrdef char* HuffmanCode[MAXLEAFNUM+1];
   ```

   ```c
   void createHTree(HuffmanTree HT,char *c,int *w,int n)
   /*数组 c[0..n-1] 和 w[0..n-1] 存放了 n 个字符及其概率，构造哈夫曼树HT*/
   {	int i,s1,s2;
   	if(n <= 1) reurn;
   	for(i=1;i<=n;i++) {	/*根据 n 个权值构造 n 棵只有根结点的二叉树*/
   		HT[i].ch = c[i-1]; HT[i].weight = w[i-1];
   		HT[i].parent=HT[i].lchild=HT[i].rchild=0;
   	}
   	for(;i<2*n;++i) { /*初始化*/
   		HT[i].parent=0; HT[i].lchild=HT[i].rchild=0;
   	}
   	for(i=n+i;i<2*n;i++) {
   		/*从 HT[1.i-1] 中选择 parent 为0且 weight 最小的两棵树，其序号为s1和s2*/
   		select(HT,i-1,s1,s2);
   		HT[s1].parent = i;
   		HT[i].lchild = s1;
   		HT[i].weight - HT[s1].weight + HT[s2].weight;
   	}/*for*/
   }/*createHTree*/
   ```

2. 哈夫曼编码

   如果要射击长度不等的编码，必须满足下面的条件：任一字符的编码都不是另一个字符的编码的前缀，这种编码也称为前缀码；对给定的字符集 $D=\lbrace d_1,d_2,\cdots,d_n\rbrace$ 及字符的使用频率 $W=\lbrace w_1,w_2,\cdots,w_n\rbrace$，构造其最优前缀码的方法为：以 $d_1,d_1,\cdots,d_n$ 作为叶子结点，$w_1,w_2,\cdots,w_n$ 作为叶子结点的权值，构造除一棵最优二叉树，然后将树中每个结点的左分支标上 0，右分支标上 1，则每个叶子结点代表的字符的编码就是从根到叶子的路径上的 0、1 组成的串

   ![](https://www.pianshen.com/images/687/f5b53d36b12684e8b6f3d144abc93e47.JPEG)

   译码时就从树根开始，若编码序列中当前编码为 0，则进入当前结点的左子树；为 1 则进入右子树，到达叶子时一个字符就翻译出来了，然后在从树根开始重复上述过程，直到编码序列结束。

   ```c
   void HuffmanCoding(HuffmanTree HT, HuffmanCode HC, int n)
   /*n 个叶子结点在哈夫曼树 HT 中的下标为 1~n，第 i(1<=i<=n) 个叶子的编码存放 HC[i] 中*/
   {	char *cd; int i,start,c,f;
   	if(n <= 1) return;
   	cd = (char*)malloc(n*sizeof(char));
   	for(i=1;i<=n;i++) {
   		start = n-1;
   		for(c=i,f=HT[i].parent;f!=0;c=f,f=HT[f].parent)
   			if(HT[f].lchild == c) cd[--start] = '0';
   			else cd[--start] = '1'
   		HC[i] = (char*)malloc((n-start)*sizeof(char));
   		strcpy(HC[i], &cd[start]);
   	}/*for*/
   	free(cd);
   }/*HaffmanCoding*/
   ```

   利用哈夫曼树译码的过程为：从根结点出发，按二进制位串的 0 和 1 确定是进入左分支还是右分支，当到达叶子结点时译出一个字符。若位串未结束，则回到根结点继续上述译码过程，直到位串结束。

   ```c
   void Decoding(HuffmanTree HT, int n, char *buff)
   /*利用具有 n 个叶子结点的最优二叉树（存储在数组 HT 中）进行译码，叶子的下标位 1~n*/
   /*buff 指向二进制位串编码序列*/
   {	int p = 2*n-1
   	while(*buff) {
   		if((*buff) == '0') p= HT[p].lchild;		/*进入左分支*/
   		else p = HT[p].rchild;					/*进入右分支*/
   		if(HT[p].lchild==0 && HT[p].rchild==0){	/*到达一个叶子结点*/
   			printf("%c", HT[p].ch);
   			p = 2*n-1;							/*回到树根*/
   		}/*if*/
   		buff++;
   	}/*while*/
   }/*Decofing*/
   ```

## 3.3.6 树和森林

1. 树的存储结构

   1. 树的双亲表示法。该表示法用一组地址连续的单元存储树的结点，并在每个结点中附设一个指示器，指出其双亲结点在该存储结构中的位置（即结点所在数组元素的下标）。显然，这种表示法对于求指定结点的双亲和祖先都十分方便，但对于求指定结点的孩子及后代则需要遍历整个数组。

   2. 树的孩子表示法。该表示法在存储结构中用指针指示出结点的每个孩子，为树中每个结点的孩子建立一个链表，即令每个结点的所有孩子结点构成一个用单链表表示的线性表，则 n 个结点的树具有 n 个链表。将这 n 个单链表的头指针又拍成一个线性表。显然，树的孩子表示法便于查找每个结点的子孙，若要找出指定结点的双亲则可能需要遍历所有的链表。

   3. 孩子兄弟表示法。孩子兄弟表示法又称二叉链表表示法，它在链表的结点中设置两个指针域分别指向该结点的第一个孩子和下一个兄弟

      树的孩子兄弟表示法为实现树、森林与二叉树之间的转换提供了可能，充分利用二叉树的有关算法来实现树及森林的操作，对难以把握规律的树和森林有着主要的现实意义

2. 树和森林的遍历

   1. 树的遍历
      1. 树的先根遍历。树的先根遍历是先访问树的根结点，然后依次先根遍历根的各棵子树。对树的先根遍历等同于对转换所得的二叉树进行先序遍历。
      2. 树的后根遍历。树的后根遍历时先依次后根遍历树根的各棵子树，然后访问树根结点。树的后根遍历等同于对转换所得的二叉树进行中序遍历。
   2. 森林的遍历
      1. 先序遍历森林。若森林为空，首先访问森林中第一棵树的根结点，然后先序遍历第一棵树根结点的子树森林，最后先序遍历除第一棵树之外剩余的树所构成的森林。
      2. 中序遍历森林。若森林为空，首先中序遍历森林中第一棵树的子树森林，然后访问第一棵树根结点的根结点，最后中序遍历除第一棵树之外剩余的树所构成的森林。

3. 树、森林和二叉树之间的相互转换

   树、森林和二叉树之间可以互相进行转换，即任何一个森林或一棵树可以对应表示为一棵二叉树，而任何一棵二叉树也能对应到一个森林或一棵树上。

   1. 树、森林转换为二叉树。

      利用树的孩子兄弟表示法可导出树与二叉树的对应关系，在树的孩子兄弟表示法中，从物理结构上看与二叉树的二叉链表表示法相同，因此就可以用这种同一存储结构的不同解释将一棵树转换为一棵二叉树。一棵树可转换成唯一的一棵二叉树。

      由于树根没有兄弟，所以树转换为二叉树后，二叉树的根一定没有右子树。这样，将一个森林转换为一棵二叉树的方法是：先将森林中的每一棵树转换为二叉树，再将第一棵树的根作为转换后的二叉树的根，第一棵树的左子树作为转换后二叉树根的左子树，第二棵树作为转换后二叉树的右子树，第三棵树作为转换后二叉树根的右子树的右子树，以此类推，森林就可以转换为一棵二叉树。

   2. 二叉树转换为树和森林。一棵二叉树可转换为唯一的树或森林。
