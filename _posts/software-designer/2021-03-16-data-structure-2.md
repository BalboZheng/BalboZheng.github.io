---
layout:     post
title:      "数据结构（下）"
subtitle:   "数据结构基础知识"
date:       2021-03-16 08:37:54
author:     "Balbo"
header-img: "img/post-bg-2021.jpg"
tags:
    - software-designer
---

# 3.4 图

## 3.4.1 图的定义与存储

图是比树结构更复杂的一种数据结构。在线性结构中，除首结点没有前驱、末尾结点没有后继外，一个结点只有唯一的一个直接前驱和唯一的一个直接后继。在树结构中，除根结点没有前驱结点外，其余的每个结点只有唯一的一个前驱结点（双亲）和多个后继结点（子树）。而在图中，任一两个结点之间都可能有直接的关系，所以图中一个结点到前驱结点和后继结点的数目是没有限制的。

1. 图的定义

   图 G 是由集合 V 和 E 构成的二元组，记为 $G=(V，E)$，其中，V 是图中顶点的非空有限集合，E 是图中边的有限集合。从数据结构的逻辑关系角度来看，图中任一顶点都有可能与其他顶点由关系，二图中所有顶点都有可能与某一顶点由关系。在图中，数据元素用顶点表示，数据元素之间的关系用边表示。

   1. 有向图。若图中每条边都是有方向的，那么顶点之间的关系用 $<v_i,v_j>$ 表示，它说明从 $v_i$ 到 $v_j$ 由一条有向边（也称为弧）。$v_i$ 是有向边的起点，称为弧尾；$v_j$ 是有向边的终点，称为弧头。所有边都有方向的图称为有向图。

   2. 无向图。若图中的每条边无方向的，顶点 $v_i$ 和 $v_j$ 之间的边用 $(v_i,v_j)$ 表示。因此，在有向图中 $<v_i,v_j>$ 与 $<v_j,v_i>$ 分别表示两条边，而在无向图中 $(v_i,v_j)$ 与 $(v_j,v_i)$ 表示的是同一条边。

   3. 完全图。若一个无向图具有 n 各顶点，而每一个顶点与其他 n-1 个顶点之间都有边，则称之为无向完全图。显然，含有 n 个顶点的无向完全图共有 $$\frac{n(n-1)}{2}$$ 条边。类似地，有 n 个顶点的有向完全图中弧的数目为 $n(n-1)$，即任意两个不同顶点之间都有方向相反的两条弧存在。

   4. 度、出度和入度。顶点 v 的度是指关联于该顶点的边的数目，记为 $D(v)$。若 G 为有向图，顶点的度表示该顶点的入度和出度之和。顶点的入度是以该顶点为终点的有向边的数目，而顶点的出度指以该顶点为起点的有向边的数目，分别记为 $ID(v)$ 和  $OD(v)$。无论是有向图还是无向图，顶点数 n，边数 e与各顶点的度之间有以下关系

      $$e=\frac12\sum_{i=1}^nD(v_i)$$

   5. 路径。在无向图 G 中，从顶点 $v_p$ 到顶点 $v_q$ 的路劲是指存在一个顶点序列 $v_p,v_{i1},v_{i2},\cdots,v_{in},v_q$，使得$(v_p,v_{i1}),(v_{i1},v_{i2}),\cdots,(v_{in},v_q)$ 均属于 $E(G)$。若 G 是有向图，其路径也是有方向的，它由 $E(G)$ 中的有向边 $<v_p,v_{i1}>,<v_{i1}.v_{i2}>,\cdots,<v_{in},v_q>$ 组成。路径长度是路径上边或弧的数目。第一个顶点和最后一个顶点相同的路径称为回路或环。若一条路径上除了 $v_p$ 和 $v_q$ 可以相同外，其余顶点均不相同，则称其为简单路径。

   6. 子图。若由两个图 $G=(V,E)$ 和 $G'=(V',E')$，如果 $V'\subseteq V$ 且 $E'\subseteq E$，则称 $G'$ 为 $G$ 的子图。

   7. 连通图与连通分量。在无向图 G 中，若从顶点 $v_i$ 到顶点 $v_j$ 由路径，则称顶点 $v_i$ 和顶点 $v_j$ 是连通的。如果无向图 G 中任意两个顶点都是连通的，则称为连通图。无向图 G 的极大连通子图称为 G 的连通分量。

   8. 强连通图与强连通分量。在有向图 G 中，如果对于每一对顶点 $v_i$，$v_j\in V$ 且 $v_i\not= v_j$，从顶点 $v_i$ 到顶点 $v_j$ 和从顶点 $v_i$ 到 $v_j$ 都存在路径，则称图 G 为强连通图。有向图中的极大连通子图称为有向图的强连通分量。

   9. 网。边（或弧）带权值的图称为网。

   10. 有向树。如果一个有向图恰有一个顶点的入度为 0，其余顶点的入度均为 1，则是一棵有向树。

2. 图的存储结构

   1. 邻接矩阵表示法

      图的邻接举证表示法是指用一个矩阵来表示图中顶点之间的关系。对于具有 n 个顶点的图 $G=(V,E)$，其邻接矩阵是一个 n 阶方阵，且满足：

      $$A[i][j]=\begin{cases} 1, & 若(v_i,v_j)或<v_i,v_j>属于E \\ 0, & 若(v_i,v_j)或<v_i,v_j>不属于E \end{cases} $$

      其中，$W_{ij}$ 是边（弧）上的权值。

      ```c
      /*用邻接举证表示图表示*/
      #define MaxN 30					/*图中顶点数目的最大值*/
      typedef int AdjMatrix[MaxN][MaxN];
      ```

      OR

      ```c
      tupedef double AdjMatrix[MaxN][MaxN];	/*邻接矩阵*/
      tupedef struct {
      	int Vnum;						/*图中的顶点数目*/
      	AdjMatrix Arcs;
      }Graph;
      ```

   2. 邻接链表表示法

      邻接链表表示法指的是为图的每个顶点建立一个单链表，第 i 个单链表中的结点表示依附于顶点 $v_i$ 的边（对于有向图是以 $v_i$ 为尾的弧）。邻接链表中的结点有表结点（或边结点）和表头结点两种类型

      |        | 表结点  |      |
      | :----: | :-----: | :--: |
      | adjvex | nextarc | Info |

      | 表头结点 |          |
      | :------: | :------: |
      |   data   | firstarc |

      + adjvex：指示与顶点 $v_i$ 邻接的顶点的序号
      + nextarc：指示下一条边或弧的结点
      + info：存储与边或弧有关的信息，如权值等
      + data：存储顶点 $v_i$ 的名或其他有关信息
      + firstarc：指示链表中的第一个接待你（邻接结点）

      ```c
      #define MaxN 50					/*图中顶点数目的最大值*/
      typedef struct ArcNode{			/*邻接链表的表结点*/
      	int adjvex;					/*邻接顶点的顶点序号*/
      	double weight;				/*边（弧）上的权值*/
      	struct ArcNode *nextarc;	/*指向下一个邻接顶点的指针*/
      }EdgeNode;
      typedef struct VNode{			/*邻接链表的头结点*/
      	char data;					/*顶点表示的数据，以一个字符表示*/
      	struct ArcNode *firstarc;	/*指向第一条依附于该顶点的边或弧的指针*/
      }AdjList[MaxN];
      typedef struct{
      	int Vnum;					/*图中顶点的数目*/
      	AdjList Vertices;
      }Graph;
      ```

      显然，对于有 n 个顶点、e 条边的无向图来说，其邻接链表需用 n 个头结点和 2e 个表结点。

      对于无向图的邻接链表，顶点 $v_i$ 的度恰为第 i 个链接链表中表结点的数目，而在有向图中，为求顶点的入度，必须扫描逐个链接表，这是因为第 i 个邻接链表中表姐点的数目只是顶点 $v_i$ 的出度。为此，可以建立一个有向图的逆邻接链表。

      ![inverse_adjacency.png](https://i.loli.net/2021/03/21/7MRsLadxJ2IFqD4.png)

## 3.4.2 图的遍历

1. 深度优先搜索(Depth First Search, DFS)

   从图 G 中任一结点 v 出发按深度优先搜索法进行遍历的步骤如下

   1. 设置搜索指针 p，使 p 指向顶点 v。
   2. 访问 p 所指顶点，并使 p 指向与相邻接的且尚未被访问过的顶点。
   3. 若 p 所指顶点存在，则重复步骤2，否则执行步骤4
   4. 沿着刚才访问的次序和方向回溯到一个尚有邻接顶点且未被访问过的顶点并使 p 指向这个未被访问的顶点，然后重复步骤2，直到可以得到其递归遍历算法。

   该算法的特点是尽可能先对纵深分析搜索

   ```c
   int visited[MaxN] = {0};		/*调用遍历算法前设置所有的顶点都没有被反问过*/
   void Dfs(Grqph G,int i) {
   	EdgeNode *t; int j;
   	printf("%d", i);			/*访问序号为 i 的顶点*/
   	visited[i] = 1;				/*序号为 i 的顶点已被访问过*/
   	t = G.Vertices[i].firstarc;	/*取顶点 i 的第一个接邻顶点*/
   	while(t != NULL){			/*检查所有与顶点 i 相邻接的顶点*/
   		j = t->adjvex;			/*顶点 j 为顶点 i 的一个邻接顶点*/
   		if(visited[j] == 0)		/*若顶点 j 未被访问则从顶点 j 出发进行深度优化搜索*/
   			Dfs(G,j);
   		t = t->nextarc;			/*取顶点 i 的下一个邻接顶点*/
   	}/*while*/
   }/*Dfs*/
   ```

   从函数 Dfs() 之外调用 Dfs 可以访问到所有与起始顶点有路径相通的其他顶点。若图是不连通的，则下一次应从另一个未被的呢过的顶点出发，再次调用 Dfs 进行遍历，直到将图中所有的顶点都访问到为止。

   深度优先遍历图的过程实质上是对某个顶点查找其邻接点的过程，其耗费的时间取决于所采用的存储结构。当图用邻接矩阵表示时，查找所有顶点的邻接点所需时间为 $O(n^2)$。若以邻接表作为图的存储结构，则需要 $O(e)$ 的时间复杂度查找所有顶点的邻接点，因此当以邻接表作为存储结构时，深度优先搜索遍历图的时间复杂度 $O(n+e)$。

2. 广度优先搜索(Breadth First Search, BFS)

   图的广度优先搜索方法为：从图中的某个顶点 v 出发，在访问了 v 之后依次访问 v 的各个未被访问过的邻接点，然后分别从这些邻接点出发依次访问它们的邻接点，并使“先被访问的顶点的邻接点”先于“后被访问的顶点的邻接点”被访问，直到图中所有已被访问的顶点的邻接点都被访问到。若此时还有未被访问的顶点，则另选图中的一个未被访问的顶点作为起点，重复上述过程，直到图中所有的顶点都被访问到为止。

   广度优先遍历图的特点是尽可能先进行横向搜索，即最先访问的顶点的邻接点也先被访问。为此，引入队列来保存已访问过的顶点序列，即每当一个顶点被访问后，就将其放入队列中，当队头顶点出队时，就访问其未被访问的邻接点并令这些邻接顶点入队。

   ```c
   void Bfs(Graph G)
   {	/*广度优先遍历图 G*/
   	EdgeNode *t; int i,j,k;
   	int visited[MaxN] ={0};					/*调用遍历算法前设置所有的顶点都没有被访问过*/
   	InitQueue(Q);							/*创建一个空队列*/
   	for(i=0;i<G.Vnum;i++) {
   		if(!visited[i]){					/*顶点i未被访问过*/
   			EnQueue(Q,i);
   			printf("%d",i); visited[i] = 1;	/*访问顶点i并设置已访问标志*/
   			while(!isEmpty(Q)){				/*若会裂不空，则继续取顶点进行广度优先搜索*/
   				DeQuque(Q,k);
   				t = G.Vertices[k].firstarc;
   				for(;t;t=->nextarc){		/*检查所有与顶点k相邻接的顶点*/
   					j = t->adjvex;			/*顶点j是顶点k的一个邻接顶点*/
   					if(visited[j] == 0){	/*若顶点j未被访问过，将j加入队列*/
   						EnQueue(Q,j);
   						printf("%d",j);		/*访问序号为j的顶点并设置已访问标志*/
   						visited[j] = 1;
   					}/*if*/
   				}/*for*/
   			}/*while*/
   		}/*if*/
   	}/*for i*/
   }/*Bfs*/
   ```

   在广度优先遍历算法中，每个顶点最多进一次队列。

   遍历图的过程实质上是通过边或弧找邻接点的过程，因此广度优先搜索遍历图和深度优先搜索遍历图的运算时间复杂度相同，其不同之处仅仅在于对顶点访问的次序不同。

## 3.4.3 生成树及最小生成树

1. 生成树的概念

   对于有 n 个顶点的连通图，至少有 n-1 条边，而生成树中恰好有 n-1 条边，所以连通图的生成树是该图的极小连通子图。若在图的生成树中任意加一条边，则必然形成回路。

   ![graph.png](https://i.loli.net/2021/03/21/x6tQ1RhKMGC7ESz.png)

2. 最小生成树

   对于连通网来说，边是带权值的，生成树的各边也带权值，因此把生成树个边的权值总和称为生成树的权，把权值最小的生成树称为最小生成树。求解最小生成树有许多实际应用。

   1. 普利姆（Prim）算法

      假设 $N=(V,E)$ 是连通网，TE 是 N 上最小生成树中边的集合。算法从顶点集合 $U=\lbrace u_0\rbrace\lbrace u_0 \in V\rbrace$、边的集合 TE={}开始，重复执行下述操作：在所有 $u\in U,v\in V-U$ 的边 $(u,v)\in E$ 中找一条代价最小的边 $(u_0,v_0)$，把这条边并入集合 TE，同时将 $v_0$ 并入集合 $U$，直到 $U=V$ 时为止，此时 $TE$ 中必有 n-1 条边，$T=(V,\lbrace TE\rbrace)$  为  的最小生成树。

      由此可知，普利姆算法构造最小生成树的过程是以一个顶点集合 $U=\lbrace u_0\rbrace$ 作为初态，不断寻找与 U 章顶点相邻且代价最小的边的另一个顶点，扩充 U 集合直到 $U=V$ 时为止。

      用普利姆算法构造最小生成树的过程：

      ![Prim.png](https://i.loli.net/2021/03/21/a6d1POn9jVC7GEB.png)

      <center>普利姆算法构造最小生成树的过程</center>

      普利姆算法的时间复杂度为 $O(n^2)$，与图中的变数无关，因此该算法适合于求边稠密的网的最小生成树

   2. 克鲁斯卡尔（Kruskal）算法

      克鲁斯卡尔求最小生成树的算法思想为：假设连通网 $N=(V,E)$，令最小生成树的初始状态为只有 n 个顶点而无边的非连通图 $T=(V,\lbrace\rbrace)$，图中每个顶点自成一个连通分量。在 E 中选择代价最小的边，若该边依附的顶点落在在 T 中不同的邻接分量上，则将此边加入到 T 中，否则舍去此边而选择下一条代价最小的边。以此类推，直到 T 中所有顶点在同一连通分量上为止。

      克鲁斯卡尔算法是时间复杂度为 $O(e\log e)$，于图中的顶点数无关，因此该算法适合于求边稀疏的网的最小生成树。

      ![Kruskal.png](https://i.loli.net/2021/03/21/4dojxCFqMImuSZK.png)

      <center>克鲁斯卡尔算法构造最小生成树的过程</center>

## 3.4.4 拓扑排序和关键路径

1. AOV 网

   在过程利于，一个大的工程项目通常被划分为许多较小的子工程（称为活动）。显然，当这些子工程都完成时，整个过程也就完成了。在有向图总，若以顶点表示活动，用有向边表示活动之间的优先关系，则称这样的有向图为以顶点表示活动的网（Activity On Vertex network, AOV 网）。在 AOV 网中，若从顶点 $v_i$ 到顶点 $v_j$ 有一条有向路径，则顶点 $v_i$ 是 $v_j$ 的前驱，顶点 $v_j$ 是 $v_i$ 的后继。若 $<v_i,v_j>$ 是网中的一条弧，则顶点 $v_i$ 是 $v_j$ 的直接前驱，顶点 $v_j$ 是 $v_i$ 的直接后继。AOV 网中的弧表示了活动之间的优先关系，也可以说是一种活动进行时的制约关系。

   在 AOV 网中不应出现有向环，若存在，则一维着某项活动必须以自身任务的完成为先决条件，显然这是荒谬的。因此，若要检测一个过程是否可行，首先应检查对应的 AOV 网是否存在回路。不存在回路的有向图称为有向无环图，或 DAG（Directed Acycline Graph）图。检测的方法是对有向图构造其顶点的拓扑有序序列。若图中所有的顶点都在它的拓扑有序序列中，则该 AOV 网中必定不存在环。

2. 拓扑排序及其算法

   拓扑排序是将 AOV 网中的所有顶点排成一个线性序列的过程，并且该序列满足：若在 AOV 网中从顶点 $v_i$ 到 $v_j$ 有一条路径，则在该线性序列中，顶点 $v_i$ 必然在顶点 $v_j$ 之间。

   一般情况下，假设 AOV 图代表一个工程计划，则 AOV 网顶点一个拓扑排序就是一个工程顺利完成的可行方案。对 AOV 网进行拓扑排序的方法如下：

   1. 在 AOV 网中选择一个入度为 0 （没有前驱）的顶点且输出它
   2. 从网中输出该顶点及与该顶点有关的所有弧
   3. 重复上述两步，直到网中不存在入度为 0 的顶点为止

   执行结果有两种情况：

   1. 所有顶点以输出，此时整个拓扑排序完成，说明网中不存在回路
   2. 尚有未输出的顶点，剩余的顶点均有前驱顶点，表明网中存在回路，拓扑排序无法进行下去

   当有向图中无环时，还可以利用深度优先遍历进行逆拓扑排序

3. AOE 网

   若在带权有向图 G 中以顶点表示事件，以有向边表示活动，以边上的权值表示该活动持续的时间，则这种带权有向图称为用边表示活动的网（Activity On Edge network, AOE 网）。通常在 AOE 网中列出来完成预定工程计划所需进行的活动、每项活动的计划完成时间、活动开始或结束的时间以及这些事件和活动间的关系，从而可以分析该项工程是否实际可行并估计工程完成的最短事件，以及影响工程进度的关键活动；进一步可以进行人力、物力的调度和分配，以达到缩短工期的目的。

   在用 AOE 网表示一项工程计划时，顶点所表示的事件实际上就是某些活动以及完成、某些活动可以动工的标志。具体来说，顶点所表示的事件是指该顶点所有加入边所表示的活动均已完成、从它出发的边所表示的活动均可以开始的一种事件。

   一般情况下，每项工程都有一个开始事件和一个结束事件，所以在 AOE 网中至少有一个入度为 0 的开始顶点，称为源点。另外，应有一个出度为 0 的结束顶点，称为汇点。AOE 网中不应存在有向回路，否则整个工程无法完成。

   AOE 网所关心的问题有：

   1. 完成该工程至少需要多少事件？
   2. 那些活动是影响整个过程进度的关键？

   由于 AOE 网中的某些活动能够并行地进行，因此完成整个过程所需的时间是从开始顶点到结束顶点的最长路径的长度。这里的路径长度是指该路径上的权值之和。

4. 关键路径和关键活动

   在从源点到汇点的路径中，长度最长的路径称为关键路径。关键路径1上的所有活动均是关键活动。

   1. 顶点事件的最早发生时间 $ve(j)$。

      $ve(j)$ 是指从源点 $v_0$ 到 $v_j$ 的最长路径长度（时间）。这个时间决定了所有从 $v_j$ 发出的弧所表示的活动能够开工的最早时间，计算方法为：

      $$\begin{cases} ve(0)=0 \\ ve(j)=max\lbrace ve(i)+dut(<i,j>)\rbrace & <i,j>\in T,1\leq j\leq n-1 \end{cases} $$

      其中，$T$ 是所有到达顶点 j 的弧的集合，$dut(<i,j>)$ 是弧 $<i,j>$ 上的权值，n 是网中的顶点数。

   2. 顶点事件的最晚发生时间 $vl(i)$。

      $vl(i)$ 是指在不推迟整个工期的前提下，事件 $v_i$ 的最晚发生事件，计算方法为：

      $$\begin{cases} vl(n-1)=ve(n-1) \\ vl(i)=min\lbrace vl(j)-dut(<i,j>)\rbrace & <i,j>\in S,1\leq i\leq n-2 \end{cases} $$

      其中，$S$ 是所有从顶点 i 发出的弧的集合。

   3. 活动 $a_k$ 的最早开始时间 $e(k)$。

      $e(k)$ 是指弧 $<i,j>$ 所表示的活动 $a_k$ 最早可开工时间

      $$e(k)=ve(i)$$

   4. 活动 $a_k$ 的最晚开始时间 $l(k)$。

      $l(k)$ 是指在不推迟整个工期的前提下，该活动的最晚开始时间。

      $$l(k)=cl(j)-dut(<i,j>)$$

# 3.5 查找

## 3.5.1 查找的基本概念

1. 基本概念

   查找是一种常用的基本运算。查找表是指由同一类型的数据元素（或记录）构成的集合。由于“集合”中的数据元素之间存在着完全松散的关系，因此，查找表是一种非常灵活的数据结构。

   对查找表经常要进行的两种操作

   1. 查询某个特定的数据元素是否在查找表中
   2. 检索某个特定的数据元素的各种属性

   通常将只进行着两种操作的查找表称为静态查找表

   对查找表经常要进行的另外两种操作如下

   1. 在查找表中插入一个数据元素
   2. 从查找表中删除一个数据元素

   若需要在查找表中插入不存在的数据元素，或者从查找表中删除已存在的某个数据元素，则称此类查找表为动态查找表

   关键字是数据元素（或记录）的某个数据项的值，用它来识别（标识）这个数据元素。主关键字是指能唯一标识一个数据元素的关键字。次关键字是指能标识多个数据元素的关键字。

   根据给定的某个值，在查找表中确定是否存在一个其关键字等于给定值的记录或数据元素。若表中存在这样的一个记录，则称查找成功，此时给出整个记录的信息，或者指出记录在查找表中的为止；若表中不存在关键字等于给定值的记录，则称查找不成功，此时的查找结果用一个“空”记录或“空”指针表示。

2. 平均查找长度

   对于查找算法来说，其基本操作是“将记录的关键字与给定值进行比较”。因此，通常以“其关键字和给定值进行过比较的记录个数的期望值”作为衡量查找算法好坏的依据。

   为确定记录在查找表中的位置，需和给定关键字值进行比较的次数的期望值称为查找算法在查找成功时的平均查找长度

   对含有 n 个记录的表，查找成功时的平均查找长度定义为

   $$ASL=\sum_{i=1}^nP_iC_i$$

   其中，$P_i$ 为对表中第 i 个记录进行查找的概率，且 $\sum^n_{i=1}P_i=1$，一般情况下，均认为查找每个记录的概率是相等的，即$P_i=1/N$;$C_i$ 为找到表中其关键字与给定值相等的记录时（为第 i 个记录），和给定值已进行比较的关键字个数，显然，$C_i$ 随查找方法的不同而不同。

## 3.5.2 静态查找表的查找方法

1. 顺序查找

   顺序查找的方法对于顺序存储方式和链式存储方式的查找表都适用

   从顺序查找的过程可知，$C_i$ 取决于所查记录在表中的位置。若需查找的记录正好是表中第一个记录，仅需比较一次；若查找成功时找到的是表中的最后一个记录，则需要比较 n 次，从表尾开始查找时正好相反。一般情况下，$C_i=n-i+1$，一次在等概率情况下，顺序查找成功的平均查找长度为

   $$ASL_ss=\sum^n_{i=1}P_iC_i=\frac1n\sum^n_{i=1}(n-i+1)=\frac{n+1}{2}$$

   也就是说，成功查找的平均比较次数约为表长的一半。若所查记录不在表中，则必须进行 n 次（不设监视哨，设置监视哨时为 n+1 次）比较才能确定失败。

2. 折半查找

   设查找表的元素存储在一维数组 $r[1,\cdots,n]$ 中，在表中的元素已经按关键字递增方式排序的情况下，进行折半查询的方法是：首先将待查元素的关键字（key）值与表 r 中间位置上（下标为 mid）记录的关键字进行比较，若相等，则查找成功；若 $key>r[mid].key$，则说明待查记录只可能在后半个子表 $r[mid+1,\cdots,n]$中，下一步应在后半个子表中进行查找；若 $key<r[mid].key$，说明待查记录只可能在前半个子表 $r[1,\cdots,mid-1]$中，下一步应在 r 的前半个子表中进行查找，这样逐步缩小范围，直到查找成功或子表为空时失败为止。

   ```c
   /*整型数组中的元素是按非递减的方式排列的，在其中进行折半查找的算法*/
   int Bsearch(int r[], int low, int high, int key)
   /*元素存储在数组 r[low..high]，用折半查找的方法在数组r中找值为key的元素*/
   /*若找到返回该元素的下标，否则返回-1*/
   {	int mid;
   	while(low <= high) {
   		mid = (low + high)/2;
   		if(key == r[mid]) return mid;
   		else if (key < r[mid]) high = mid - 1;
   		else low = mid+1;
   	}/*while*/
   	return -1;
   }/*Bsearch*/
   ```

   ```c
   /*整型数组中的元素是按非递减的方式排列的，在其中进行折半查找的递归算法*/
   int Bsearch_rec(int r[], int low, int high, int key)
   /*元素存储在数组 r[low..high]，用折半查找的方法在数组r中找值为key的元素*/
   /*若找到返回该元素的下标，否则返回-1*/
   {	int mid;
   	if(low <= high) {
   		mid = (low + high)/2;
   		if(key == r[mid]) 
   			return mid;
   		else if (key < r[mid]) 
   			return Bsearch_rec(r, low, mid-1, key);
   		else 
   			return Bsearch_rec(r, mid+1, high, key);
   	}/*if*/
   	return -1;
   }/*Bsearch_rec*/
   ```

   折半查找的过程可以用一棵二叉树描述，方法是以当前查找区间的中间位置序号作为根，左半个子表和右半个子表中的记录序号分别作为根的左子树和右子树上的结点，这样构造的二叉树称为折半查找判定树。
   从折半查找判定树可以看出，查找成功时，折半查找的过程恰好走了一条从根结点到被查找结点的路径，与关键字进行比较的次数即为被查找结点在树中的层数。因此，折半查找在查找成功时进行比较的关键字个数最多不超过树的深度，而具有 $n$ 个结点的判定树的深度为 $\lfloor \log_2n\rfloor+1$。
   给判定树中所有结点的空指针域加一个指向方形结点的指针，称这些为判定树的外部结点，那么折半查找不成功的过程就是走一条从根结点到外部结点的路径。与给定值进行比较的关键字个数等于该路径上内部结点个数，即 $\lfloor \log_2n\rfloor+1$。
   折半查找的平均查找长度是多少？设结点总数为 $n=2^h-1$，则判定树的深度为 $h=log_2n+1$ 的满二叉树。在等概率情况下，折半查找的平均查找长度为

   $$ASL_{bs}=\sum^n_{j=1}P_iC_i=\frac1n\sum^n_{j=1}j*2\lceil{j-1}=\frac{n+1}{n}\log_2(n+1)-1$$

   当 n 值较大时,$ASL_{bs}\approx \log(n+1)-1$

   折半查找比顺序查找效率要高，但它要求查找表进行顺序存储并且按关键字有序排列。因此，当需要对表进行插入或删除操作时，需要移动大量。所以折半查找适用于表不易不动且又经常进行查找的情况。

3. 分块查找

   分块查找又称索引顺序查找，是对顺序查找方法的一种改进，其效率介于顺序查找与折半查找之间。
   在分块查找过程中，首先将表分成若干块，每一块的关键字不一定有序，但块之间是有序的，即后一块中所有记录的关键字大于前一个块中最大的关键字。此外，还建立一个“索引表”，索引表按关键字有序。
   因此，分块查找过程分为两步：

   1. 在索引表中确定待查记录所在的块
   2. 在块内顺序查找

   由于分块查找实际上是两次查找的过程，因此其平均查找长度应该是两次查找的平均查找长度（索引查找与块内查找）之和，即

   $$ASL_{bs}=L_b+L_w$$

   其中，$L_b$ 为查找索引表的平均查找长度，$L_w$ 为块内查找时的平均查找长度。
   分块查找时，可将长度为 $n$ 的表均匀分成 $b$ 块，每块含有 $s$ 个记录，即有$$b=\lceil\frac{n}{s} \rceil$$。在等概率查找的情况下，块内查找的概率为 $\frac1s$，每块的查找概率为 $\frac1b$，若用顺序查找确定元素所在的块，则分块查找的平均查找长度为

   $$ASL_{bs}=L_b+L_w=\frac1b\sum^b_{j=1}j+\frac1s\sum^s_{i=1}i=\frac{b+1}{2}+\frac{s+1}{2}=\frac12(\frac{n}{s}+s)+1$$

   可见，其平均查找长度不仅与表长 $n$ 有关，而且与每一块的记录 $s$ 有关。可以证明，当 $s$ 取 $\sqrt{n}$ 时，$ASL_{bs}$ 取最小值 $\sqrt{n}+1$，此时的查找效率较顺序查找要好得多，但远不及折半查找。

## 3.5.3 动态查找表

1. 二叉排序树

   二叉树排序又称二叉查找树，它或者是一棵空树，或者是具有以下性质的二叉树。

   1. 若他的左子树非空，则左子树上所有结点的值均小于根结点的值。
   2. 若它的右子树非空，则右子树上所有结点的值均大于根结点的值。
   3. 左、右子树本身是二次排序树。

2. 二叉排序树的查找过程

   二叉排序树上进行查找的过程为：二叉排序树非空时，将给点值与根结点的关键字相比较，若相等，则查找成功；若不相等，则当根结点的关键字值大于给定值时，下一步到根的左子树中进行查找，否则到根的右子树中进行查找。若查找成功，则查找过程是走了一条从树根到所找到结点的路径；否则，查找过程终止于一棵空的子树。

   ```c
   typedef struct Tnode {
   	int data;						/*结点的关键字值*/
   	struct Tnode *lchild.*rchild;	/*指向左、右子树的指针*/
   }BSTnode,*BSTree;
   ```

   ```c
   BSTree SearchBST(BSTree root, int key, BSTree *father)
   /*在root指向根的二叉排序树中查找键值为key的结点*/
   /*若找到，返回该结点的指针；否则返回空指针NULL*/
   {	BSTree p = root; *father = NULL;
   	while(p && p->data != key){
   		*father = p;
   		if(key < p->data) p = p->lchild;
   		else p = p->rchild;
   	}/*while*/
   	return p;
   }/*SearchBST*/
   ```

3. 在二叉排序树中插入结点的操作

   二叉排序树是通过依次输入数据元素并把它们插入到二叉树的适当位置构造起来的，具体的过程是：每读入一个元素，建立一个新结点。若二叉排序树非空，则将新结点的值与根结点的值相比较，如果小于根结点的值，则插入到左子树中，否则插入到右子树中；若二叉排序树为空，则新结点作为二叉排序树的根结点。设关键字序列为｛46，25，54，13，29，91｝，则整个二叉排序树的构造过程如图

   ![Binary sort tree.png](https://i.loli.net/2021/03/22/VCejnOihoLBAtHE.png)

   <center>二叉排序树的构造过程</center>

   ```c
   int InsertBST(BSTree *root, int newkey)
   /*在*root指向根的二叉排序树中插入一个键值为 newkey 的结点，插入成功则返回0，否则返回-1*/
   {	BSTree s,p,f;
   	s = (BSTree)malloc(sizeof(BSTnode));
   	if(!s) return -1;
   	s->data = newkey; s->lchild = NULL; s->rchild = NULL;
   	p = SearchBST(*root, newkey, &f);
   	if(p) return -1;
   	if(!f) *root = s;
   	else if(newkey <f->data) f->lchild = s;
   		else f->rchild = s;
   	return 0;
   }/*InsertBST*/
   ```

   从上面的插入过程还可以看到，每次插入的新结点都是二叉排序树上的新的叶子结点，因此，插入结点时不必移动其他结点，仅需改动某个结点的孩子指针。
   另外，由于一棵二叉排序树的形态由输入序列决定，所以在输入序列已经有序的情况下，所构造的二叉排序树是一棵单枝树。例如，对于关键字序列（12，18，23，45，60），建立的二叉排序树如图，此时，查找效率与顺序查找的效率相同。

   ![Keyword sequence.png](https://i.loli.net/2021/03/22/9H8dEowhtgrMm1z.png)

4. 在二叉排序树中删除结点的操作

   在二叉排序树中删除一个结点，不能把以该结点为根的子树都删除，只能删除这个结点并仍旧保持二叉排序树的特性。也就是说，在二叉排序树上删除一个结点相当于在有序序列中删除一个元素。
   假设要在二叉排序树中删除结点 $*p$（$p$ 指向被删除结点），$*f$ 为其双亲结点，则该操作可分为3种情况：

   1. 若结点 $*p$ 为叶子结点且 $*p$ 不是根结点，即 $p->lchild$ 及 $p->rchild$ 均为空，则由于删去叶子结点后不破坏整棵树的结构，因此只需修改 $*p$ 的双亲结点 $*f$ 的相应指针即可。

      $$f->lchild(或f->rchild)=NULL;$$

   2. 若结点 $*p$ 只有左子树或者只有右子树且 $*p$ 不是根结点，此时只要 $*p$ 的左子树或右子树接成其双亲结点 $*f$ 的左子树（或右子树），即令

      $$f->lchild(或f->rchild)=p->lchild;$$

      或

      $$f->lchild(或f->rchild)=p->rchild;$$

   3. 若 $*p$ 结点的左、右子树均不空，则删除 $*p$ 结点时应将其左子树、右子树连接到适当的位置，并保持二叉排序树的特性。可采用两种方法进行处理：
         1. 令 $*p$ 的左子树为 $*f$ 的左子树（若 $*p$ 不是根结点且 $*p$ 是 $*f$ 的左子树，否则为右子树），而将 $*p$ 的右子树下接到中序遍历时 $*p$ 的直接前驱结点 $*s$（$*s$ 结点是 $*p$ 的左子树中最右下方的结点）的右孩子指针上。
         2. 用 $*p$ 的中序直接前驱（或后继）结点 $*s$ 代替 $*p$ 结点，然后删除 $*s$ 结点。

   ![Delete Keyword.png](https://i.loli.net/2021/03/22/LJbqYidGhUP1EmM.png)

   一个无序序列可以通过构造一棵二叉排序树而得到一个有序序列，构造二叉排序树的过程就是对无序序列进行排序的过程。

5. 平衡二叉树

   平衡二叉树又称为 AVL 树，它或者时一颗空树，或者时具有下列性质的二叉树。它的左子树和右子树都是平衡二叉树，且左子树和右子树的高度之差的绝对值不超过 1。若将二叉树结点的平衡因子（Balance Factor, BF）定义为该结点左子树的高度减去其右子树的高度，则平衡二叉树上所有结点的平衡因子只可能时-1、0 和 1。只要树上有一个结点的平衡因子的绝对值大于 1，则该二叉树就是不平衡的。

   分析二叉排序树的查找过程可知，只有在树的形态比较均匀的情况下，查找效率才能达到最佳。因此，希望在构造二叉排序树的过程中，保持其为一棵平衡二叉树。

   使二叉树保持平衡的基本思想使：每当在二叉排序树中插入一个结点时，首先检查时否因插入破坏了平衡。若是，则找出其中的最小不平衡二叉树，在保持二叉排序树特性的情况下，调整最小不平衡子树中结点之间的关系，以达到新的平衡。所谓最小不平衡子树，是指离插入结点最近且以平衡因子的绝对值大于 1 的结点作为根的子树。

   1. 平衡二叉树上的插入操作
      
      一般情况下，假设由于在二叉排序树上插入结点而失去平衡的最小子树根结点的指针为 a，也就是说，a 所指结点是离插入结点最近且平衡因子的绝对值超过 1 的祖先结点，那么失去平衡后进行调整的规律可归纳为：
      
      1. LL 型单向右旋平衡处理。由于在 $*a$（即结点 A）的右子树的右子树上插入新结点，是 $*a$ 的平衡因子由 -1 变为 -2，导致以 $*a$ 为根的子树失去平衡，因此需进行一次向左的逆时针旋转操作。
      2. RR型单向左旋平衡处理。由于在 $*a$（即结点 A）的左子树的左子树上插入新结点，是 $*a$ 的平衡因子由 1 增至 2，导致以 $*a$ 为根的子树失去平衡，因此需进行一次向右的顺时针旋转操作。
      3. LR型先左后右双向旋转平衡处理。由于在 $*a$（即结点 A）的左子树的右子树上插入新结点，是 $*a$ 的平衡因子由 1 增至 2，导致以 $*a$ 为根的子树失去平衡，因此需进行两次旋转（先左旋后右旋）操作。
      4. RL型先右后左双向旋转平衡处理。由于在 $*a$（即结点 A）的右子树的左子树上插入新结点，是 $*a$ 的平衡因子由 -1 增至 -2，导致以 $*a$ 为根的子树失去平衡，因此需进行两次旋转（先右旋后左旋）操作。
      
   2. 平衡二叉树上的删除操作

      在平衡二叉树上进行删除操作比插入操作更复杂。若待删结点的两个子树都不为空，就用该结点左子树上的中序遍历的最后一个结点（或其右子树上的第一个结点）替换该结点，将情况转化为待删除的结点只有一个子树后再进行处理。当一个结点被删除后，从被删结点到树根的路径上所有结点的平衡因子都需要更新。对于每一个位于该路径上的平衡因子为 $\pm2$ 的结点来说，都要进行平衡处理。

6. B_树

   一棵 m 阶的 B_树，或为空树，或为满足下列特性的 m 叉树。

   1. 树中每个结点最多有 m 棵子树。

   2. 若根结点不是叶子结点，则最少有两棵子树。

   3. 除根之外的所有非终端结点最少有 $$\lceil\frac m2\rceil$$ 棵子树。

   4. 所有的非终端结点中包含下列数据信息

      $$(n,A_0,K_1,A_1,K_2,A_2,\cdots,K_N,A_N)$$

      其中，$K_i(i=1,2,\cdots,n)$ 为关键字，且 $K_i<K_{i+2}(i=1,2,\cdots,n-1);A_i(i=0,1,\cdots,n)$ 为指向子树根结点的指针，且指针 $A_{i-1}$ 所指子树中所有结点的关键字均小于 $K_i(i=1,2,\cdots,n)$，$A_n$ 所指子树中所有结点的关键字均大于 $K_n$，n 为结点中关键字的个数且满足 $$(\lceil\frac m2\rceil-1\leq n\leq m-1)$$

   5. 所有的叶子结点都出现再同一层次上，并且不带信息（可以看作是外部结点或查找失败的结点，实际上这些结点不存在，指向这些结点的指针为空）。

## 3.5.4 哈希表

1. 哈希函数的构造方法

   常用的哈希函数构造方法有直接定址法、数字分析法、平方取中法、折叠法、随机数法和除留余数法等。

   对于哈希函数的构造，应解决好两个主要问题。

   1. 哈希函数应是一个压缩映像函数，它应具有较大的压缩性，以节省存储空间
   2. 哈希函数应具有较好的散列性，虽然冲突是不可避免的，但应尽量减少

   要减少冲突，就要设法使哈希函数尽可能均匀第把关键字映射到存储区的各个存储单元，这样就可以提高查找效率。

2. 处理冲突的方法

   解决冲突就是为出现冲突的关键字找到另一个“空”的哈希地址。再处理冲突的过程中，可能得到一个地址序列 $H_i(i=1,2,\cdots,k)$。常见的处理冲突的方法有以下几种。

   1. 开放定址法。

      $$H_i=(H(key)+d_i)/m$$  $i=1,2,\cdots,k(k\leq m-1)$

      其中，$H(key)$ 为哈希函数；$m$ 为哈希表表长; $d_i$ 为增量序列

      常见增量序列

      1. $d_i=1,2,3,\cdots,m-1$,称为线性探测再散列
      2. $d_i=1^2,-1^2,2^2,-2^2,3^2,\cdots,\pm k^2(k\leq\frac m2)$，称为二次探测再散列
      3. $d_i=$ 伪随机数序列，称为随机探测再散列

      最简单的产生探测序列的方法是进行线性探测，也就是发生冲突时，顺序地到存储区的下一个单元进行探测

   2. 链地址法。

      链地址法（或拉链法）时一种经常使用且很有效的方法。它再查找表的每一个记录中增加一个链域，链域中存放下一个具有相同哈希函数值的存储地址。利用链域，就把若干个发生冲突的记录链接再一个链表内。当链域的值为 NULL 时，表示已没有后继记录了。一次，对于发生冲突时的查找和插入操作就跟线性表一样了。

   3. 再哈希法。

      $$H_i=RH_i(key)$$ $（i=1,2,\cdots,k)$

      $RH_i$ 均时不同的哈希函数，即在同义词发生地址冲突时计算另一个哈希函数地址，直到冲突不再发生。这种方法不易产生聚集现象，但增加了计算时间。

   4. 建立一个公共溢出区。无论由哈希函数得到的哈希地址时什么，一旦发生冲突，都填入到公共溢出区中。

3. 哈希表的查找

   在哈希表中进行查找操作时，用与存入元素时相同的哈希函数和冲突处理方法计算得到待查记录的存储地址，然后到相应的存储单元获得有关信息再判定查找是否成功。因此，哈希查找的特点如下。

   1. 虽然哈希表再关键字与记录的存储位置之间建立了直接映像，但由于“冲突”的产生，使得哈希表的查找过程仍然时一个给定值和关键字进行比较的过程。因此，仍需要以平均查找长度衡量哈希表的查找效率
   2. 再查找过程中需要和给定值进行比较的关键字的个数取决于下列 3 个因素：哈希函数、处理冲突的方法和哈希表的装填因子。

   一般情况下，冲突处理方法相同的哈希表，其平均查找长度依赖于哈希表的装填因子。哈希表的装填因子定义为

   $$\alpha=\frac{\text{表中装入的记录数}}{\text{哈希表的长度}}$$

   $\alpha$ 标志着哈希表的装满程度。直观地看，$\alpha$ 越小，发生冲突的可能性就越小；反之，$\alpha$ 越大，表中已填入的记录越多，再填记录时，发生冲突的可能性就越大，则查找时，给定值需与之惊醒比较的关键字的个数也就越多。

# 3.6 排序

## 3.6.2 简单排序

1. 直接插入排序

   直接插入排序时一种简单的排序方法。具体做法是：再插入第 i 个记录时，$R_1、R_2、\cdots、R_{i-1}$ 已经排好序，这时将 $R_i$ 的关键字 $k_i$ 一次与关键字 $k_{i-1}、k_{i-2}$ 等进行比较，从而找到应该插入的位置并将 $R_i$ 插入位置及其后的记录一次向后移动。

   ```c
   void Insertsort(int data[], int n)
   /*将数组 data[0]~data[n-1] 中的n个整数按非递减有序的方式进行排列*/
   {	int i,j;
   	int tmp;
   	for(i=1;i<n;i++){
   		if(data[i]<data[i-1]){
               tmp=data[i]; data[i]=data[i-1];
               for(j=i-1;j>=0&&data[j]>tmp;j--) data[j+1]=data[j];
               data[j+1]=tmp;
   		}/*if*/
   	}/*for*/
   }/*Insertsort*/
   ```

   直接插入排序是一种稳定的排序方法，其时间复杂度为 $O(n^2)$。在排序过程中仅需要一个元素的辅助空间，空间复杂度为 $O(1)$。

2. 冒泡排序

   n 个记录进行冒泡排序的方法是：首先将第一个记录的关键字和第二个记录的关键字进行比较，若为逆序，则交换着两个记录的值，然后比较第二个记录和第三个记录的关键字，以此类推，直到第 n-1 个记录和第 n 个记录的关键字比较过为止。删除过程称为第一趟冒泡排序，其结果是关键字最大的记录被交换到第 n 个记录的位置上。然后进行第二趟冒泡排序，对前 n-1 个记录进行同样的操作，其结果是关键字次大的记录被交换到第 n-1 个记录的位置上。最多进行 n-1 躺，所有记录有序排列。若再某躺冒泡排序过程没有进行相邻位置的元素交换处理，则可结束排序过程。

   冒泡排序是一种稳定的排序方法，其时间复杂度为 $O(n^2)$。再排序过程中仅需要一个元素的辅助空间用于元素的交换，空间复杂度为 $O(1)$。

3. 简单选择排序

   n 个记录进行简单选择排序的基本方法是：其时间复杂度为 $O(n^2)$。在排序过程中仅需要一个元素的辅助空间用于元素的交换，空间复杂度为 $O(1)$。

   ```c
   void SelectSort(int data[], int n)
   /*将数组data中的n个整数按非递减有序的方式进行排列*/
   {	int i,j,k,tmp;
   	for(i=0;i<n-1;i++){
   		k=i;
   		for(j=i+1;j<n;j++)
   			if(data[j]<data[k]) k=j;
   		if(k!=i) {
   			tmp = data[i]; data[i] = data[k]; data[k] = tmp;
   		}/*if*/
   	}/*for*/
   }/*electSort*/
   ```

   简单选择排序是一种不稳定的排序方法，其时间复杂度为 $O(n^2)$。在排序过程中仅需要一个元素的辅助空间用于数据元素值的交换，空间复杂度为 $O(1)$。

## 3.6.3 希尔排序

希尔排序又称为“缩小增量排序”，它是对直接插入排序方法的改进。

希尔排序的基本思想是：先将整个待排记录序列分割成若干子序列，然后分别进行直接插入排序，待整个序列中的记录基本有序时，在对全体记录进行一次直接插入排序。具体做法时：先取一个小于 n 的整数 $d_1$ 作为第一个增量，把文件的全部记录分成 $d_1$ 个组，即将所有距离为 $d_1$ 倍数序号的记录放在同一个组中，在各组内进行直接插入排序；然后取第二个增量 $d_2(d_2<d_1)$，重复上述房租和排序工作，以此类推，直到所取的增量 $d_i=1(d_i<d_{i-1}<\cdots<d_2<d_1)$，即所有记录放在同一组进行直接插入排序为止。

当增量序列为“5，3，1”时，希尔插入排序过程如下。

| [初始关键字]： | 48   | 37   | 64   | 96   | 75   | 12   | 26   | $\overline {48}$ | 54   | 03   |
| -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---------------- | ---- | ---- |
|                | 48   |      |      |      |      | 12   |      |                  |      |      |
|                |      | 37   |      |      |      |      | 26   |                  |      |      |
|                |      |      | 64   |      |      |      |      | $\overline {48}$ |      |      |
|                |      |      |      | 96   |      |      |      |                  | 54   |      |
|                |      |      |      |      | 75   |      |      |                  |      | 03   |

| 第一趟排序结果： | 12   | 26   | $\overline {48}$ | 54   | 03   | 48   | 37   | 64   | 96   | 75   |
| ---------------- | ---- | ---- | ---------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
|                  | 12   |      |                  | 54   |      |      | 37   |      |      | 75   |
|                  |      | 26   |                  |      | 03   |      |      | 64   |      |      |
|                  |      |      | $\overline {48}$ |      |      | 48   |      |      | 96   |      |

| 第一趟排序结果： | 12   | 03   | $\overline {48}$ | 37   | 26               | 48   | 54   | 64   | 96   | 75   |
| ---------------- | ---- | ---- | ---------------- | ---- | ---------------- | ---- | ---- | ---- | ---- | ---- |
| 第二趟排序结果： | 03   | 12   | 26               | 37   | $\overline {48}$ | 48   | 54   | 64   | 75   | 96   |

```c
void ShellSort(int data[], int n)
{	int *delta,k,i,t,dk,j;
	k = n;
	/*从k=n开始，重复k=k/2运算，直到k等于1，所得k值的序列作为增量序列存入delta*/
	delta = (int*)malloc(sizeof(int)*(n/2));
	i = 0;
	do{
		k = k/2; delta[i++] = k;
	}while(k > 1);
	i = 0;
	while((dk = delta[i])>0) {
		for(k=delta[i];k<n;k++)
			if(data[k]<data[k-dk]){					/*将元素data[k]插入到有序增量子表中*/
				t = data[k];						/*备份待插入的元素，空出一个元素位置*/
				for(j=d-dk;j>=0 && t<data[j];j-=dk)
					data[j+dk] = data[j];			/*寻找插入位置的同时元素后移*/
				data[j+dk] = t;						/*找到插入位置，插入元素*/
			}/*if*/
		++i;										/*取下一个增量值*/
	}/*while*/
}/*ShellSort*/
```

希尔排序时一种不稳定的排序方法，据统计分析其时间复杂度约为 $O(n^{1.3})$，在排序过程中仅需要一个元素的辅助空间用于数组元素值的交换，空间复杂度为$O(1)$。

## 3.6.4 快速排序

快速排序的基本思想是：通过一趟牌组将待排的记录划分为独立的俩个部分，称为前半区和后半区，其中，前半区中记录的关键字均不大于后半区的关键字，然后再分别对这两部分记录继续进行快速排序，从而使整个序列有序。

一趟快速排序的过程称为一次划分，具体做法是：附设两个位置指示变量 i 和 j，他猛的初值分别指向序列的第一个记录和最后一个记录。设枢轴记录（通常是第一个记录）的关键字为 pivot，则首先从 j 所指位置起向前搜索，找到第一个关键字小于 pivot 的记录时将该记录向前移到 i 指示的位置，然后从 i 所指位置起向后搜索，找到第一个关键字大于 pivot 的记录时将该记录向后移到 j 所指位置，重复该过程直至 i 与 j 相等位置。

```c
int partition(int data[], int low, int high)
/*用data[low]作为枢轴元素pivot进行划分*/
/*使得data[low..i-1]均大于pivot，data[i+1..high]均不小于pivot*/
{	int i,j; int pivot;
	pivot = data[low]; i = low; j = high;
	while(i < j){						/*从数组的两端交替地向中间扫描
		while(i<j && data[j]>=pivot) j--;
		data[i] = data[j];				/*比枢轴元素小者往前移*/
		while(i<j && data[i]<=pivot) i++;
		data[j] = data[i];				/*比枢轴元素大者往后移*/
	}
	data[i] = pivot;
	return i;
}
```

```c
void quickSort(int data[], int low, int high)
/*用快速排序方法对数组元素data[low..high]作非递减排序*/
{	
	if(low<high) {
		int loc = partition(data, low, high);	/*进行划分*/
		quicksort(data, low, loc-1);			/*对前半区进行快速排序*/
		quicksort(data, loc+1, high);			/*对后半区进行快速排序*/
	}
}/*quickSort*/
```

快速排序算法的时间复杂度为 $O(n\log_2n)$，再所有算法复杂度为此数量级的排序方法中，快速排序被认为时平均性能最好的一种。

## 3.6.5 堆排序

对于 n 个元素的关键字序列 $\lbrace K_1,K_2,\cdots,K_n\rbrace$，当且仅当满足下列关系时称其为堆，其中 2i 和 2i+1 应不大于 n。

$$\begin{cases} K_i\leq K_{2i} \\ K_i\leq K_{2i+1} \end{cases} 或 \begin{cases} K_i\geq K_{2i} \\ K_i\geq K_{2i+1} \end{cases}$$

堆排序的基本思想是：对一组待排序记录的关键字，首先按堆的定义排成一个序列（即建立初始堆），从而可以输出堆顶的最大关键字（对于大根堆而言），然后将剩余的关键字再调整成新堆，便得到次大的关键字，如此反复，直到全部关键字拍成有序序列为止。

初始堆的建立方法是：将待排序的关键字分发放到一棵完全二叉树的各个结点中（此时完全二叉树并不一定具备堆的特性），显然，所有 $$i>\lfloor\frac n2\rfloor$$ 的结点 $K_i$ 都没有子结点，以这样的 $K_i$ 为根的子树已经是堆，一次初始建堆可从完全二叉树的第 $$i(i=\lfloor\frac n2\rfloor)$$ 个结点 $K_i$ 开始，通过调整逐步使以 $$K_{\lfloor\frac n2\rfloor}、K_{\lfloor\frac n2\rfloor-1}、K_{\lfloor\frac n2\rfloor-2}、\cdots、K_2、K_1$$ 为根的子树满足堆的定义。

```c
void HeapAdjust(int data[], in s, int m)
/*在data[s..m]所构成的一个元素序列中，主力data[s]外，其余元素均满足大顶堆的定义*/
/*调整元素data[s]的文职，使data[s..m]称为一个大顶堆*/
{	int tmp,j;
	tmp = data[s];						/*备份元素data[s]，为其找到适当为止后再插入*/
	for(j=2*s;j<=m;j=j*2+1){				/*沿值较大的孩子结点向下筛选*/
		if(j<m && data[j]<data[j+1]) ++j;	/*j 是值较大的元素的下标*/
		if(tmp >= data[j]) break;
		data[s] = data[j]; s = j;			/*用s记录待插入元素的位置（下标）*/
	}/*for*/
	data[s] = tmp;							/*将备份元素插入由s所指出的插入位置*/
}/*HeapAdjust*/
```

```c
void HeapSort(int data[], int n)			/*数组data[0..n-1]中的n个元素进行堆排序*/
{	int i;
	int tmp;
	for(i=n/2-1;i>=0;--i)					/*将data[0..n-1]调整为大根堆*/
		HrapAdjust(data, i, n-1);
	for(i=n-1;i>0;--i)
	{	tmp = data[0]; data[0] = data[i];
		data[i] = tmp;						/*堆顶元素data[0]与序列末的元素sata[i]交换*/
		HeapAdjust(data, 0, i-1);			/*待排元素的个数减1，将data[0..i-1]重新调整为大根堆*/
	}/*for*/
}/*HeapSort*/
```

对于记录数较少的文件来说，对排序的优越性并不明显，但对于大量的记录来说，堆排序是很有效的。堆排序的整个算法时间是由建立初始堆和不断调整堆着两部分时间构成的。可以证明，堆排序算法的时间复杂度为 $O(n\log n)$。此外，堆排序只需要一个记录大小的辅助空间。堆排序是一种不稳定的排序方法。

## 3.6.6 归并排序

所谓“归并”,是将两个或两个以上的有序文件合并成为一个新的有序文件。归并排序的一种实现方法是把一个有 n 个记录的无序文件看成是由 n 个长度为 1 的有序子文件组成的文件，然后进行两两归并，得到 $$\lfloor\frac n2\rfloor$$ 个长度为 2 或 1 的有序文件，再两两归并，如此反复，直到最后形成包含 n 个记录的有序文件为止。这种反复将两个有序文件归并成一个有序文件的排序方法称为两路归并排序。

```c
void Merge(int data[], int s, int m, int n)
{	int i,start = s,k = 0;
	int *temp;
	temp = (int*)malloc((n-s+1)*sizeof(int));		/*辅助空间*/
	for(i=m+1;s<=m && i<=n;++k)						/*将data[s..m]与data[m+1..n]归并后存入temp*/
		if(data[s]<data[i]) temp[k] = data[s++];
		else temp[k] = data[i++];
	for(;s<=m;++k)								/*将剩余的data[s..m]复制到temp*/
		temp[k] = data[s++];
	for(;i<=n;i++)								/*将剩余的data[i..n]复制到temp*/
		temp[k] = data[i++];
	for(i=0;i<k;i++)
		data[start++] = temp[i];
		free(temp);
}/*Merge*/
```

```c
void MSort(int data[], int s, int t)		/*对data[s..t]进行归并排序*/
{	int m;
	if(s<t){
		m = (s+t)/2;						/*将data[s..t]均分为data[s..m]和data[m+1..t]*/
		MSort(data, s, m);					/*递归地对data[s..m]进行归并排序*/
		MSort(data, m+1, t);				/*递归地对data[m+1..t]进行归并排序*/
		Merge(data, s, m, t);				/*将data[s..m]和data[m+1..t]归并为data[s..t]*/
	}
}/*MSort*/
```

```c
void MergeSort(int data[], int n)
{
	MSort(data, 0, n-1);
}/*MergeSort*/
```

## 3.6.7 基数排序

技术排序的思想是按组成关键字的各个数位的值进行排序，它是分配排序的一种，在该排序方法中把一个关键字 $K_i$ 看成一个 d 元组，即

$$K_i^1,K_i^2,\cdots.K_i^d$$

其中，$0\leq K_i^j<r,i=1~n,j=1~d$。这里的 r 称为基数。若关键字是十进制的，则 $r=10$；若关键字是八进制的，$r=8$。d 是关键字的位数，d 值取所有待排序的关键字位数的最大值，其他不足 d 位的关键字在前面补零。

在 $$K_i^1,K_i^2,\cdots.K_i^d$$ 中，$K_i^1$ 称为最高有效位，$K_i^2$ 称为次高有效位，$K_i^d$ 称为最低有效位。技术排序可以从最高有效位开始，也可以从最低有效位开始。

基数排序的基本思想是：设立 r 个队列，队列的编号分别为 $0、1、2、\cdots、r-1$。首先按最低有效问的值把 n 个关键字分配到这 r 个队列中；然后按照队列编号从小到大将各队列中的关键字依次收集起来；接着再按照最高有效位的值把刚收集起来的关键字分配到 r 个队列中。重复上述分配和手机过程，直到按照最高有效位分配和收集。这样就得到了一个从小到大有序的关键字序列。为了减少记录移动的次数，队列可以采用链式存储分配。每个链队列设两个指针，分别指向队头和队尾。

技术排序是一种稳定的偶爱徐方法。对于 n 个记录，执行依次分配和收集的时间为 $O(n+r)$。如果关键字由 d 位，则要执行 d1遍。所以总的运算时间为 $O(d(n+r))$。可见，对于不同的基数 r，所用的时间是不同的。当 r 或 d 较小时，这种排序方法较为节省时间。另外，基数排序适用于链式分配的记录的排序，其要求的附加存储量是 r 个队列的头、尾指针，所以附加存储量为 2r 个存储单元。由于待排序记录是以链表方式存储的，相对于顺序分配而言，还增加了 n 个指针域的空间。
