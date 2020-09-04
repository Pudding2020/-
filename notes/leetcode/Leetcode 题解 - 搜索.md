<!-- GFM-TOC -->
* [BFS](#bfs)
    * [1091. 二进制矩阵中的最短路径（网格中从原点到特定点的最短路径长度）](#1091-二进制矩阵中的最短路径网格中从原点到特定点的最短路径长度)
    * [279. 完全平方数（组成整数的最小平方数数量）](#279-完全平方数组成整数的最小平方数数量)
    * [3. 最短单词路径](#3-最短单词路径)
* [DFS](#dfs)
    * [1. 查找最大的连通面积](#1-查找最大的连通面积)
    * [2. 矩阵中的连通分量数目](#2-矩阵中的连通分量数目)
    * [3. 好友关系的连通分量数目](#3-好友关系的连通分量数目)
    * [4. 填充封闭区域](#4-填充封闭区域)
    * [5. 能到达的太平洋和大西洋的区域](#5-能到达的太平洋和大西洋的区域)
    * [6. 由房间中的钥匙能到达的房间数](#6-由房间中的钥匙能到达的房间数)
* [Backtracking](#backtracking)
    * [1. 数字键盘组合](#1-数字键盘组合)
    * [2. IP 地址划分](#2-ip-地址划分)
    * [3. 在矩阵中寻找字符串](#3-在矩阵中寻找字符串)
    * [4. 输出二叉树中所有从根到叶子的路径](#4-输出二叉树中所有从根到叶子的路径)
    * [5. 排列](#5-排列)
    * [6. 含有相同元素求排列](#6-含有相同元素求排列)
    * [7. 组合](#7-组合)
    * [8. 组合求和](#8-组合求和)
    * [9. 含有相同元素的组合求和](#9-含有相同元素的组合求和)
    * [10. 1-9 数字的组合求和](#10-1-9-数字的组合求和)
    * [11. 子集](#11-子集)
    * [12. 含有相同元素求子集](#12-含有相同元素求子集)
    * [13. 分割字符串使得每个部分都是回文数](#13-分割字符串使得每个部分都是回文数)
    * [14. 数独](#14-数独)
    * [15. N 皇后](#15-n-皇后)
    * [16. 电话号码的字母组合](#16-电话号码的字母组合)
<!-- GFM-TOC -->


# BFS

广度优先搜索一层一层地进行遍历，每层遍历都是以上一层遍历的结果作为起点，遍历一个距离能访问到的所有节点。需要注意的是，遍历过的节点不能再次被遍历。

每一层遍历的节点都与根节点距离相同。设 d<sub>i</sub> 表示第 i 个节点与根节点的距离，推导出一个结论：对于先遍历的节点 i 与后遍历的节点 j，有 d<sub>i</sub> <= d<sub>j</sub>。利用这个结论，可以求解最短路径等   **最优解**   问题：第一次遍历到目的节点，其所经过的路径为最短路径。应该注意的是，使用 BFS 只能求解无权图的最短路径，无权图是指从一个节点到另一个节点的代价都记为 1。

在程序实现 BFS 时需要考虑以下问题：

- 队列：用来存储每一轮遍历得到的节点；
- 标记：对于遍历过的节点，应该将它标记，防止重复遍历。

BFS模板：

```html

void BFS()
{
    判断边界条件，是否能直接返回结果的。 

    定义队列;
    定义备忘录，用于记录已经访问的位置；

    

    将起始位置加入到队列中，同时更新备忘录。

    while (队列不为空) {
        获取当前队列中的元素个数。
        for (元素个数) {
            取出front一个位置节点，同时pop该节点。
            判断是否到达终点位置。
            获取它对应的下一个所有的节点。
            条件判断，过滤掉不符合条件的位置。
            新位置重新加入队列。
        }
    }

}
```

## 1091.二进制矩阵中的最短路径（网格中从原点到特定点的最短路径长度）

[leetcode-1091.二进制矩阵中的最短路径](https://leetcode-cn.com/problems/shortest-path-in-binary-matrix/)

在一个 N × N 的方形网格中，每个单元格有两种状态：空（0）或者阻塞（1）。可在8个方向移动，返回从左上角到右下角的最短通畅路径长度，不存在，则返回-1。

```c
//bfs求最短路径：第一次遍历到的目的节点，其所经过的路径为最短路径
class Solution
{
   public:
      int dir_x[8]={-1,-1,-1,0,0,1,1,1};
      int dir_y[8]={-1,0,1,-1,1,-1,0,1};
      int shortestPathBinaryMatrix(vector<vector<int>>& grid)
      {
         if(grid.empty() || grid.size() || grid[0].size()) return -1;
         int rows=grid.size(),cols=grid[0].size();
         queue<pair<int,int>> q;
         int route=0;
         q.push({0,0});
         while(!q.empty())
         {
            int sz=q.size();
            route++;
            while(sz-->0)
            {
               auto pos=q.front();
               q.pop();
               int x=pos.first,y=pos.second;
               if(grid[x][y]==1) continue;//只有当前不是1，才能进行下一步，这一句和下一句的位置不能互换
               if(x==rows-1 && y==cols-1) return route;
               grid[x][y]=1;//标记，代替了visited数组
               for(int i=0;i<8;++i)
               {
                  int tx=x+dir_x[i];
                  int ty=y+dir_y[i];
                  if(tx<0 || tx>=rows || ty<0 || ty>=cols) continue;
                  q.push(make_pair(tx,ty));
               }
               
            }
         }
         return -1;
      }
};
```

## 279.完全平方数（组成整数的最小平方数数量）

[leetcode-279.完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

```html
For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9.
```

可以将每个整数看成图中的一个节点，如果两个整数之差为一个平方数，那么这两个整数所在的节点就有一条边。

要求解最小的平方数数量，就是求解从节点 n 到节点 0 的最短路径。

本题也可以用动态规划求解，在动态规划部分中会再次出现。

```c
//bfs优先遍历到的路径最短，n每次减去平方数，首次为0时，所用的个数最少
class Solution {
public:
     //产生小于n的完全平方数
    void GenerateSquares(int n,vector<int> &squares)
    {
        int square=1,index=1;
        int diff=3;
        
        while(square<=n)
        {
            squares.push_back(square);
            ++index;
            square=index*index;
            //这样也可
            //squares.push_back(square);
            //square+=diff;
            //diff+=2;
        }
        /*这样也可
        for(int i=1;i*i<=n;++i)
        {
            squares.push_back(i*i);
        }
        */
    }

    int numSquares(int n) {
        if(n<=0) return 0;

        vector<int> squares;
        GenerateSquares(n,squares);
        queue<int> q;
        q.push(n);
        vector<int> visited(n+1,0);
        visited[n]=1;
        int nums=0;
        while(!q.empty())
        {
            int sz=q.size();
            ++nums;
            while(sz-->0)
            {
                int cur=q.front();//判断要在下面的for里，因为根本不允许有负数的值push，所以cur根本取不到负数
                q.pop();
                for(auto i:squares)
                {
                    int next=cur-i;
                    if(next<0) break;
                    if(next==0) return nums;//此时为第一次到达，一定是个数最少的（画一个树就明白啦）
                    if(visited[next]) continue;//如果这个值之前被访问过，则这个时候再访问，肯定不是个数最少的
                    visited[next]=1;
                    q.push(next);
                } 
            }
        }
        return n;//没有的话，返回自身
    }
};
```
# DFS

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/74dc31eb-6baa-47ea-ab1c-d27a0ca35093.png"/> </div><br>

广度优先搜索一层一层遍历，每一层得到的所有新节点，要用队列存储起来以备下一层遍历的时候再遍历。

而深度优先搜索在得到一个新节点时立即对新节点进行遍历：从节点 0 出发开始遍历，得到到新节点 6 时，立马对新节点 6 进行遍历，得到新节点 4；如此反复以这种方式遍历新节点，直到没有新节点了，此时返回。返回到根节点 0 的情况是，继续对根节点 0 进行遍历，得到新节点 2，然后继续以上步骤。

从一个节点出发，使用 DFS 对一个图进行遍历时，能够遍历到的节点都是从初始节点可达的，DFS 常用来求解这种   **可达性**   问题。

在程序实现 DFS 时需要考虑以下问题：

- 栈：用栈来保存当前节点信息，当遍历新节点返回时能够继续遍历当前节点。可以使用递归栈。
- 标记：和 BFS 一样同样需要对已经遍历过的节点进行标记。

## 1. 查找最大的连通面积

695\. Max Area of Island (Medium)

[Leetcode](https://leetcode.com/problems/max-area-of-island/description/) / [力扣](https://leetcode-cn.com/problems/max-area-of-island/description/)

```html
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

```java
private int m, n;
private int[][] direction = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

public int maxAreaOfIsland(int[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    m = grid.length;
    n = grid[0].length;
    int maxArea = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            maxArea = Math.max(maxArea, dfs(grid, i, j));
        }
    }
    return maxArea;
}

private int dfs(int[][] grid, int r, int c) {
    if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] == 0) {
        return 0;
    }
    grid[r][c] = 0;
    int area = 1;
    for (int[] d : direction) {
        area += dfs(grid, r + d[0], c + d[1]);
    }
    return area;
}
```

## 2. 矩阵中的连通分量数目

200\. Number of Islands (Medium)

[Leetcode](https://leetcode.com/problems/number-of-islands/description/) / [力扣](https://leetcode-cn.com/problems/number-of-islands/description/)

```html
Input:
11000
11000
00100
00011

Output: 3
```

可以将矩阵表示看成一张有向图。

```java
private int m, n;
private int[][] direction = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    m = grid.length;
    n = grid[0].length;
    int islandsNum = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] != '0') {
                dfs(grid, i, j);
                islandsNum++;
            }
        }
    }
    return islandsNum;
}

private void dfs(char[][] grid, int i, int j) {
    if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == '0') {
        return;
    }
    grid[i][j] = '0';
    for (int[] d : direction) {
        dfs(grid, i + d[0], j + d[1]);
    }
}
```

## 3. 好友关系的连通分量数目

547\. Friend Circles (Medium)

[Leetcode](https://leetcode.com/problems/friend-circles/description/) / [力扣](https://leetcode-cn.com/problems/friend-circles/description/)

```html
Input:
[[1,1,0],
 [1,1,0],
 [0,0,1]]

Output: 2

Explanation:The 0th and 1st students are direct friends, so they are in a friend circle.
The 2nd student himself is in a friend circle. So return 2.
```

题目描述：好友关系可以看成是一个无向图，例如第 0 个人与第 1 个人是好友，那么 M[0][1] 和 M[1][0] 的值都为 1。

```java
private int n;

public int findCircleNum(int[][] M) {
    n = M.length;
    int circleNum = 0;
    boolean[] hasVisited = new boolean[n];
    for (int i = 0; i < n; i++) {
        if (!hasVisited[i]) {
            dfs(M, i, hasVisited);
            circleNum++;
        }
    }
    return circleNum;
}

private void dfs(int[][] M, int i, boolean[] hasVisited) {
    hasVisited[i] = true;
    for (int k = 0; k < n; k++) {
        if (M[i][k] == 1 && !hasVisited[k]) {
            dfs(M, k, hasVisited);
        }
    }
}
```

## 4. 填充封闭区域

130\. Surrounded Regions (Medium)

[Leetcode](https://leetcode.com/problems/surrounded-regions/description/) / [力扣](https://leetcode-cn.com/problems/surrounded-regions/description/)

```html
For example,
X X X X
X O O X
X X O X
X O X X

After running your function, the board should be:
X X X X
X X X X
X X X X
X O X X
```

题目描述：使被 'X' 包围的 'O' 转换为 'X'。

先填充最外侧，剩下的就是里侧了。

```java
private int[][] direction = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
private int m, n;

public void solve(char[][] board) {
    if (board == null || board.length == 0) {
        return;
    }

    m = board.length;
    n = board[0].length;

    for (int i = 0; i < m; i++) {
        dfs(board, i, 0);
        dfs(board, i, n - 1);
    }
    for (int i = 0; i < n; i++) {
        dfs(board, 0, i);
        dfs(board, m - 1, i);
    }

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (board[i][j] == 'T') {
                board[i][j] = 'O';
            } else if (board[i][j] == 'O') {
                board[i][j] = 'X';
            }
        }
    }
}

private void dfs(char[][] board, int r, int c) {
    if (r < 0 || r >= m || c < 0 || c >= n || board[r][c] != 'O') {
        return;
    }
    board[r][c] = 'T';
    for (int[] d : direction) {
        dfs(board, r + d[0], c + d[1]);
    }
}
```

## 5. 能到达的太平洋和大西洋的区域

417\. Pacific Atlantic Water Flow (Medium)

[Leetcode](https://leetcode.com/problems/pacific-atlantic-water-flow/description/) / [力扣](https://leetcode-cn.com/problems/pacific-atlantic-water-flow/description/)

```html
Given the following 5x5 matrix:

  Pacific ~   ~   ~   ~   ~
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * Atlantic

Return:
[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (positions with parentheses in above matrix).
```

左边和上边是太平洋，右边和下边是大西洋，内部的数字代表海拔，海拔高的地方的水能够流到低的地方，求解水能够流到太平洋和大西洋的所有位置。

```java
private int m, n;
private int[][] matrix;
private int[][] direction = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

public List<List<Integer>> pacificAtlantic(int[][] matrix) {
    List<List<Integer>> ret = new ArrayList<>();
    if (matrix == null || matrix.length == 0) {
        return ret;
    }

    m = matrix.length;
    n = matrix[0].length;
    this.matrix = matrix;
    boolean[][] canReachP = new boolean[m][n];
    boolean[][] canReachA = new boolean[m][n];

    for (int i = 0; i < m; i++) {
        dfs(i, 0, canReachP);
        dfs(i, n - 1, canReachA);
    }
    for (int i = 0; i < n; i++) {
        dfs(0, i, canReachP);
        dfs(m - 1, i, canReachA);
    }

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (canReachP[i][j] && canReachA[i][j]) {
                ret.add(Arrays.asList(i, j));
            }
        }
    }

    return ret;
}

private void dfs(int r, int c, boolean[][] canReach) {
    if (canReach[r][c]) {
        return;
    }
    canReach[r][c] = true;
    for (int[] d : direction) {
        int nextR = d[0] + r;
        int nextC = d[1] + c;
        if (nextR < 0 || nextR >= m || nextC < 0 || nextC >= n
                || matrix[r][c] > matrix[nextR][nextC]) {

            continue;
        }
        dfs(nextR, nextC, canReach);
    }
}
```

## 6. 由房间中的钥匙能到达的房间数

[leetcode-841钥匙和房间](https://leetcode-cn.com/problems/keys-and-rooms/)

```html
有 N 个房间，开始时你位于 0 号房间。每个房间有不同的号码：0，1，2，...，N-1，并且房间里可能有一些钥匙能使你进入下一个房间。

在形式上，对于每个房间 i 都有一个钥匙列表 rooms[i]，每个钥匙 rooms[i][j] 由 [0,1，...，N-1] 中的一个整数表示，其中 N = rooms.length。 

钥匙 rooms[i][j] = v 可以打开编号为 v 的房间。

最初，除 0 号房间外的其余所有房间都被锁住。你可以自由地在房间之间来回走动。

如果能进入每个房间返回 true，否则返回 false。
```

```c
/*
bfs：能到达的房间总数是否是总的房间个数
时间复杂度：o(m+n)；m为房间总个数，n为房间中钥匙总数
空间复杂度：o(m)；递归栈空间的开销
*/
class Solution {
public:
    vector<int> visited;
    vector<int> room;
    void dfs(int i,int rows,vector<vector<int>>& rooms)
    {
        if(i>=rows) return;
        visited[i]=1;
        for(auto r:rooms[i])
        {
            if(visited[r]==0)
            {
                //visited[r]=1;
                dfs(r,rows,rooms);
            }
            else continue;
        }
        room.push_back(i);//不求能到达房间的序列，所以只要有一个计数的就可以
    }
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        if(rooms.empty()) return true;
        int rows=rooms.size(),cols=rooms[0].size();
        visited.resize(rows);
        dfs(0,rows,rooms);
        if(room.size()==rows) return true;
        else return false;
    }
};
```

```c
//bfs简洁版
vector<int> visited;
    int counts=0;
    void dfs(int i,int rows,vector<vector<int>>& rooms)
    {
        if(i>=rows) return;
        visited[i]=1;
        for(auto r:rooms[i])
        {
            if(visited[r]==0)
            {
                dfs(r,rows,rooms);
            }
        }
        counts++;
    }
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        if(rooms.empty()) return true;
        int rows=rooms.size(),cols=rooms[0].size();
        visited.resize(rows);
        dfs(0,rows,rooms);
        return counts==rows;
    }
```

```c
/*
dfs
时间复杂度：o(m+n)
空间复杂度：o(m)；队列的开销
*/
class Solution {
public:
    vector<int> visited;
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        if(rooms.empty()) return true;
        int rows=rooms.size();
        visited.resize(rows);
        queue<int> q;
        //q.push(0);
        q.emplace(0);
        visited[0]=1;
        int counts=0;
        while(!q.empty())
        {
            auto n=q.front();
            q.pop();
            counts++;
            for(auto r:rooms[n])
            {
                if(visited[r]==0)
                {
                    visited[r]=1;
                    //q.push(r);
                    q.emplace(r);
                }
            }
        }
        return counts==rows;
    }
};
```

# Backtracking

## 15. N 皇后

51\. N-Queens (Hard)

[Leetcode](https://leetcode.com/problems/n-queens/description/) / [力扣](https://leetcode-cn.com/problems/n-queens/description/)

在 n\*n 的矩阵中摆放 n 个皇后，并且每个皇后不能在同一行，同一列，同一对角线上，求所有的 n 皇后的解。

一行一行地摆放，在确定一行中的那个皇后应该摆在哪一列时，需要用三个标记数组来确定某一列是否合法，这三个标记数组分别为：列标记数组、45 度对角线标记数组和 135 度对角线标记数组。

45 度对角线标记数组的长度为 2 \* n - 1，通过下图可以明确 (r, c) 的位置所在的数组下标为 r + c。

135 度对角线标记数组的长度也是 2 \* n - 1，(r, c) 的位置所在的数组下标为 n - 1 - (r - c)。

```html

回溯算法流程：

result = []
void backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        合法性判断
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```
```c
class Solution {
public:
    vector<vector<string>> res;
    vector<vector<string>> solveNQueens(int n) {
        if(n==0) return {};
        //初始化空棋盘
        vector<string> board(n,string(n,'.'));
        dfs(board,0,n);
        return res;
    }

    // 路径：board 中小于 row 的那些行都已经成功放置了皇后
    // 选择列表：第 row 行的所有列都是放置皇后的选择
    // 结束条件：row 超过 board 的最后一行
    void dfs(vector<string>& board,int row,int n)
    {
        //结束条件
        if(row==n)
        {
            res.push_back(board);
            return;
        }
        for(int col=0;col<n;++col)
        {
            if(!valid(board,row,col)) continue;
            board[row][col]='Q';
            dfs(board,row+1,n);
            board[row][col]='.';
        }
    }
    bool valid(vector<string>& board,int row,int col)
    {
        int n=board.size();
        //检查列是否有冲突
        for(int i=0;i<n;++i)
            if(board[i][col]=='Q')
                return false;
        // 检查右上方是否有皇后互相冲突
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) 
        {
            if (board[i][j] == 'Q')
                return false;
        }
        // 检查左上方是否有皇后互相冲突
        for (int i = row - 1, j = col - 1;i >= 0 && j >= 0; i--, j--) 
        {
            if (board[i][j] == 'Q')
                return false;
        }
    return true;
    }
};
```

```html
使用集合优化合理性判断
由于每个皇后必须位于不同列，因此已经放置的皇后所在的列不能放置别的皇后。
第一个皇后有 N 列可以选择，第二个皇后最多有 N−1 列可以选择，第三个皇后最多有 N−2 列可以选择
（如果考虑到不能在同一条斜线上，可能的选择数量更少），因此所有可能的情况不会超过 N! 种
遍历这些情况的时间复杂度是 O(N!)。
使用集合记录，使合理性查找的时间为o(1)(上一种方法为o(n))
同一条45°对角线：行下标-列下标=定值
同一条135°对角线：行下标+列下标=定值
时间复杂度：o(n!)
空间复杂度：o(n)
```
```c
class Solution {
public:
    vector<vector<string>> res;
    unordered_set<int> usedcol;//有皇后的列
    unordered_set<int> used45;//有皇后的左上角
    unordered_set<int> used135;//有皇后的右上角
    vector<vector<string>> solveNQueens(int n) {
        if(n<=0) return {};
        vector<string> board(n,string(n,'.'));
        backtrack(board,0,n);
        return res;
    }
    void backtrack(vector<string>& board,int row,int n)
    {
        if(row==board.size())
        {
            res.push_back(board);
            return;
        }
        for(int col=0;col<n;++col)
        {
            if(usedcol.find(col)!=usedcol.end()) continue;
            if(used45.find(row-col)!=used45.end()) continue;
            if(used135.find(row+col)!=used135.end()) continue;
            board[row][col]='Q';
            usedcol.insert(col);
            used135.insert(row+col);
            used45.insert(row-col);
            backtrack(board,row+1,n);
            board[row][col]='.';
            usedcol.erase(col);
            used45.erase(row-col);
            used135.erase(row+col);
        }
    }
};
```

## 16. 电话号码的字母组合

[leetcode-17.电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射（与电话按键相同）。注意 1 不对应任何字母。可以任意选择答案输出的顺序。

时间复杂度：O(3^m X 4^n)，其中 m 是输入中对应 3 个字母的数字个数（包括数字 2、3、4、5、6、8），n 是输入中对应 4 个字母的数字个数（包括数字 7、9），m+n 是输入数字的总个数。

当输入包含 m 个对应 3 个字母的数字和 n 个对应 4 个字母的数字时，不同的字母组合一共有 3^m X 4^n 种，需要遍历每一种字母组合。

空间复杂度：O(m+n)，其中 m 是输入中对应 3 个字母的数字个数，n 是输入中对应 4 个字母的数字个数，m+n 是输入数字的总个数。除了返回值以外，空间复杂度主要取决于哈希表以及回溯过程中的递归调用层数，哈希表的大小与输入无关，可以看成常数，递归调用层数最大为 m+n。

```c
class Solution {
public:
//不是对每个数字代表的字母全排列后再排序
//而是每次从一个数字中选择一个字母，进行排列
    void backtrack(const string& digits,string &s,vector<string>& ans,const unordered_map<char,string>& mp,int index)
    {
        if(index==digits.size())
        {
            ans.push_back(s);
        }
        else
        {
            char c=digits[index];
            const string& ss=mp.at(c);
            //mp[key]如果key不存在会添加key，不会抛出异常；mp.at(key)会检查key，如果不存在会抛出异常。
            for(auto letter:ss)
            {
                s.push_back(letter);
                backtrack(digits,s,ans,mp,index+1);
                s.pop_back();
            }
        }
    }
    vector<string> letterCombinations(string digits) {
        vector<string> ans;
        if(digits.empty()) return ans;
        unordered_map<char,string> mp
        {
            {'2',"abc"},
            {'3',"def"},
            {'4',"ghi"},
            {'5',"jkl"},
            {'6',"mno"},
            {'7',"pqrs"},
            {'8',"tuv"},
            {'9',"wxyz"}
        };
        string s;
        
        backtrack(digits,s,ans,mp,0);
        return ans;
    }
};
```


