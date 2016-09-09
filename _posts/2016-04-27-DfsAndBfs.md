---
layout: post
title: 深度优先遍历和广度优先遍历
categories: 数据结构和算法

---

>图的遍历，就是指对图中所有节点的访问。图中的节点有很多，正常的遍历策略有两种，分别是深度优先遍历和广度优先遍历

#### 深度优先遍历
**选定一个初始节点，每次访问完当前节点之后，都首先访问当前节点的第一个邻接节点**

* 访问初始节点v，并标记v为已访问
* 查找v的第一个邻接节点w，若w存在，则对w继续进行深搜
* 若w不存在，则访问v的下一个邻接节点

![](/images/pages/datastructure/dfs.png)

如图，当前这个图的深搜结果为：1 -> 2 -> 4 -> 8 -> 5 -> 3 -> 6 -> 7

#### 广度优先遍历

**广度优先遍历的过程类似于分层搜索，需要使用一个队列，以保持访问过的节点的顺序**

* 访问初始节点v，并标记v已访问
* 节点v加入队列，若队列非空时，继续执行。若队列为空了，则算法结束
* 出队列，取得队列头元素节点u
* 查找u的第一个邻接节点w，若w不存在，则返回上一步，出队列
* w存在时，若w尚未被访问，则标记节点w为已访问，将w入队列，然后查找u的下一个邻接节点。
* 直至队列为空

![](/images/pages/datastructure/dfs.png)

如图，当前这个图的深搜结果为：1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8


#### 实现算法

这里用邻接矩阵的形式给出一个实现算法，如下：

```
package com.algorithm;

public class DFS_BFS {
	
    private char[] mVexs;       // 顶点集合
    private int[][] mMatrix;    // 邻接矩阵
    private static final int INF = Integer.MAX_VALUE;   // 最大值
	
    /*
     * 创建图(用已提供的矩阵)
     *
     * 参数说明：
     *     vexs  -- 顶点数组
     *     matrix-- 矩阵(数据)
     */
    public DFS_BFS(char[] vexs, int[][] matrix) {
        
        // 初始化"顶点数"和"边数"
        int vlen = vexs.length;

        // 初始化"顶点"
        mVexs = new char[vlen];
        for (int i = 0; i < mVexs.length; i++)
            mVexs[i] = vexs[i];

        // 初始化"边"
        mMatrix = new int[vlen][vlen];
        for (int i = 0; i < vlen; i++)
            for (int j = 0; j < vlen; j++)
                mMatrix[i][j] = matrix[i][j];
    }
	
    
    /*
     * 返回顶点v的第一个邻接顶点的索引，失败则返回-1
     */
    private int firstVertex(int v) {

        if (v<0 || v>(mVexs.length-1))
            return -1;

        for (int i = 0; i < mVexs.length; i++)
            if (mMatrix[v][i]!=0 && mMatrix[v][i]!=INF)
                return i;

        return -1;
    }

    /*
     * 返回顶点v相对于w的下一个邻接顶点的索引，失败则返回-1
     */
    private int nextVertex(int v, int w) {

        if (v<0 || v>(mVexs.length-1) || w<0 || w>(mVexs.length-1))
            return -1;

        for (int i = w + 1; i < mVexs.length; i++)
            if (mMatrix[v][i]!=0 && mMatrix[v][i]!=INF)
                return i;

        return -1;
    }

    /*
     * 深度优先搜索遍历图的递归实现
     */
    private void DFS(int i, boolean[] visited) {

        visited[i] = true;
        System.out.printf("%c ", mVexs[i]);
        // 遍历该顶点的所有邻接顶点。若是没有访问过，那么继续往下走
        for (int w = firstVertex(i); w >= 0; w = nextVertex(i, w)) {
            if (!visited[w])
                DFS(w, visited);
        }
    }

    /*
     * 深度优先搜索遍历图
     */
    public void DFS() {
        boolean[] visited = new boolean[mVexs.length];       // 顶点访问标记

        // 初始化所有顶点都没有被访问
        for (int i = 0; i < mVexs.length; i++)
            visited[i] = false;

        System.out.printf("DFS: ");
        for (int i = 0; i < mVexs.length; i++) {
            if (!visited[i])
                DFS(i, visited);
        }
        System.out.printf("\n");
    }

    /*
     * 广度优先搜索（类似于树的层次遍历）
     */
    public void BFS() {
        int head = 0;
        int rear = 0;
        int[] queue = new int[mVexs.length];            // 辅组队列
        boolean[] visited = new boolean[mVexs.length];  // 顶点访问标记

        for (int i = 0; i < mVexs.length; i++)
            visited[i] = false;

        System.out.printf("BFS: ");
        for (int i = 0; i < mVexs.length; i++) {
            if (!visited[i]) {
                visited[i] = true;
                System.out.printf("%c ", mVexs[i]);
                queue[rear++] = i;  // 入队列
            }

            while (head != rear) {
                int j = queue[head++];  // 出队列
                for (int k = firstVertex(j); k >= 0; k = nextVertex(j, k)) { //k是为访问的邻接顶点
                    if (!visited[k]) {
                        visited[k] = true;
                        System.out.printf("%c ", mVexs[k]);
                        queue[rear++] = k;
                    }
                }
            }
        }
        System.out.printf("\n");
    }

	public static void main(String[] args) {
		char[] vexs = {'1', '2', '3', '4', '5', '6', '7', '8'};
        int[][] matrix = {
                 /*1*//*2*//*3*//*4*//*5*//*6*//*7*//*8*/
          /*1*/ {   0,  1,    1, INF, INF, INF, INF, INF},
          /*2*/ {   1,   0, INF,   1,   1, INF, INF, INF},
          /*3*/ {   1, INF,   0, INF, INF,   1,   1, INF},
          /*4*/ { INF,   1, INF,   0, INF, INF, INF,   1},
          /*5*/ { INF,   1, INF, INF,   0, INF, INF,   1},
          /*6*/ { INF, INF,   1, INF, INF,   0,   1, INF},
          /*7*/ { INF, INF,   1, INF, INF,   1,   0, INF},
          /*8*/ { INF, INF, INF,   1,   1, INF, INF,  0}};
        DFS_BFS db = new DFS_BFS(vexs, matrix);
        db.DFS();
        db.BFS();
	}
}
```









