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
    * [17. 第k个排列](#17-第k个排列)
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

Backtracking（回溯）属于 DFS。

- 普通 DFS 主要用在   **可达性问题**  ，这种问题只需要执行到特点的位置然后返回即可。
- 而 Backtracking 主要用于求解   **排列组合**   问题，例如有 { 'a','b','c' } 三个字符，求解所有由这三个字符排列得到的字符串，这种问题在执行到特定的位置返回之后还会继续执行求解过程。

因为 Backtracking 不是立即返回，而要继续求解，因此在程序实现时，需要注意对元素的标记问题：

- 在访问一个新元素进入新的递归调用时，需要将新元素标记为已经访问，这样才能在继续递归调用时不用重复访问该元素；
- 但是在递归返回时，需要将元素标记为未访问，因为只需要保证在一个递归链中不同时访问一个元素，可以访问已经访问过但是不在当前递归链中的元素。

## 1. 数字键盘组合

17\. Letter Combinations of a Phone Number (Medium)

[Leetcode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/) / [力扣](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/description/)

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/9823768c-212b-4b1a-b69a-b3f59e07b977.jpg"/> </div><br>

```html
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

```java
private static final String[] KEYS = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

public List<String> letterCombinations(String digits) {
    List<String> combinations = new ArrayList<>();
    if (digits == null || digits.length() == 0) {
        return combinations;
    }
    doCombination(new StringBuilder(), combinations, digits);
    return combinations;
}

private void doCombination(StringBuilder prefix, List<String> combinations, final String digits) {
    if (prefix.length() == digits.length()) {
        combinations.add(prefix.toString());
        return;
    }
    int curDigits = digits.charAt(prefix.length()) - '0';
    String letters = KEYS[curDigits];
    for (char c : letters.toCharArray()) {
        prefix.append(c);                         // 添加
        doCombination(prefix, combinations, digits);
        prefix.deleteCharAt(prefix.length() - 1); // 删除
    }
}
```

## 2. IP 地址划分

93\. Restore IP Addresses(Medium)

[Leetcode](https://leetcode.com/problems/restore-ip-addresses/description/) / [力扣](https://leetcode-cn.com/problems/restore-ip-addresses/description/)

```html
Given "25525511135",
return ["255.255.11.135", "255.255.111.35"].
```

```java
public List<String> restoreIpAddresses(String s) {
    List<String> addresses = new ArrayList<>();
    StringBuilder tempAddress = new StringBuilder();
    doRestore(0, tempAddress, addresses, s);
    return addresses;
}

private void doRestore(int k, StringBuilder tempAddress, List<String> addresses, String s) {
    if (k == 4 || s.length() == 0) {
        if (k == 4 && s.length() == 0) {
            addresses.add(tempAddress.toString());
        }
        return;
    }
    for (int i = 0; i < s.length() && i <= 2; i++) {
        if (i != 0 && s.charAt(0) == '0') {
            break;
        }
        String part = s.substring(0, i + 1);
        if (Integer.valueOf(part) <= 255) {
            if (tempAddress.length() != 0) {
                part = "." + part;
            }
            tempAddress.append(part);
            doRestore(k + 1, tempAddress, addresses, s.substring(i + 1));
            tempAddress.delete(tempAddress.length() - part.length(), tempAddress.length());
        }
    }
}
```

## 3. 在矩阵中寻找字符串

79\. Word Search (Medium)

[Leetcode](https://leetcode.com/problems/word-search/description/) / [力扣](https://leetcode-cn.com/problems/word-search/description/)

```html
For example,
Given board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
word = "ABCCED", -> returns true,
word = "SEE", -> returns true,
word = "ABCB", -> returns false.
```

```c

//只要搜索返回 True 才返回，如果全部的格子都搜索完了以后，都返回 False ，才返回 False。

bool backtrack(vector<vector<char>>& board,string word,int x,int y,int cur)
{
  if(x<0 || x>=board.size() || y<0 || y>=board[0].size() || board[x][y]!=word[cur])
      return false;
  else if(cur==word.size()-1) return true;
  char c=board[x][y];
  board[x][y]='#';
  if(backtrack(board,word,x+1,y,cur+1) || backtrack(board,word,x-1,y,cur+1) || backtrack(board,word,x,y+1,cur+1) || backtrack(board,word,x,y-1,cur+1))
  return true;
  board[x][y]=c;
  return false;
}
bool exist(vector<vector<char>>& board, string word) {
  if(board.empty() || word.empty()) return false;
  for(int i=0;i<board.size();++i)
      for(int j=0;j<board[0].size();++j)
          if(backtrack(board,word,i,j,0)) return true;       
  return false;
}     
```

## 4. 输出二叉树中所有从根到叶子的路径

257\. Binary Tree Paths (Easy)

[Leetcode](https://leetcode.com/problems/binary-tree-paths/description/) / [力扣](https://leetcode-cn.com/problems/binary-tree-paths/description/)

```html
  1
 /  \
2    3
 \
  5
```

```html
["1->2->5", "1->3"]
```

```java

public List<String> binaryTreePaths(TreeNode root) {
    List<String> paths = new ArrayList<>();
    if (root == null) {
        return paths;
    }
    List<Integer> values = new ArrayList<>();
    backtracking(root, values, paths);
    return paths;
}

private void backtracking(TreeNode node, List<Integer> values, List<String> paths) {
    if (node == null) {
        return;
    }
    values.add(node.val);
    if (isLeaf(node)) {
        paths.add(buildPath(values));
    } else {
        backtracking(node.left, values, paths);
        backtracking(node.right, values, paths);
    }
    values.remove(values.size() - 1);
}

private boolean isLeaf(TreeNode node) {
    return node.left == null && node.right == null;
}

private String buildPath(List<Integer> values) {
    StringBuilder str = new StringBuilder();
    for (int i = 0; i < values.size(); i++) {
        str.append(values.get(i));
        if (i != values.size() - 1) {
            str.append("->");
        }
    }
    return str.toString();
}
```

## 5. 排列

46\. Permutations (Medium)

[Leetcode](https://leetcode.com/problems/permutations/description/) / [力扣](https://leetcode-cn.com/problems/permutations/description/)

```html
[1,2,3] have the following permutations:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> permutes = new ArrayList<>();
    List<Integer> permuteList = new ArrayList<>();
    boolean[] hasVisited = new boolean[nums.length];
    backtracking(permuteList, permutes, hasVisited, nums);
    return permutes;
}

private void backtracking(List<Integer> permuteList, List<List<Integer>> permutes, boolean[] visited, final int[] nums) {
    if (permuteList.size() == nums.length) {
        permutes.add(new ArrayList<>(permuteList)); // 重新构造一个 List
        return;
    }
    for (int i = 0; i < visited.length; i++) {
        if (visited[i]) {
            continue;
        }
        visited[i] = true;
        permuteList.add(nums[i]);
        backtracking(permuteList, permutes, visited, nums);
        permuteList.remove(permuteList.size() - 1);
        visited[i] = false;
    }
}
```

## 6. 含有相同元素求排列

47\. Permutations II (Medium)

[Leetcode](https://leetcode.com/problems/permutations-ii/description/) / [力扣](https://leetcode-cn.com/problems/permutations-ii/description/)

```html
[1,1,2] have the following unique permutations:
[[1,1,2], [1,2,1], [2,1,1]]
```

数组元素可能含有相同的元素，进行排列时就有可能出现重复的排列，要求重复的排列只返回一个。

在实现上，和 Permutations 不同的是要先排序，然后在添加一个元素时，判断这个元素是否等于前一个元素，如果等于，并且前一个元素还未访问，那么就跳过这个元素。

```java
public List<List<Integer>> permuteUnique(int[] nums) {
    List<List<Integer>> permutes = new ArrayList<>();
    List<Integer> permuteList = new ArrayList<>();
    Arrays.sort(nums);  // 排序
    boolean[] hasVisited = new boolean[nums.length];
    backtracking(permuteList, permutes, hasVisited, nums);
    return permutes;
}

private void backtracking(List<Integer> permuteList, List<List<Integer>> permutes, boolean[] visited, final int[] nums) {
    if (permuteList.size() == nums.length) {
        permutes.add(new ArrayList<>(permuteList));
        return;
    }

    for (int i = 0; i < visited.length; i++) {
        if (i != 0 && nums[i] == nums[i - 1] && !visited[i - 1]) {
            continue;  // 防止重复
        }
        if (visited[i]){
            continue;
        }
        visited[i] = true;
        permuteList.add(nums[i]);
        backtracking(permuteList, permutes, visited, nums);
        permuteList.remove(permuteList.size() - 1);
        visited[i] = false;
    }
}
```

## 7. 组合

77\. Combinations (Medium)

[Leetcode](https://leetcode.com/problems/combinations/description/) / [力扣](https://leetcode-cn.com/problems/combinations/description/)

```html
If n = 4 and k = 2, a solution is:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

```java
public List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> combinations = new ArrayList<>();
    List<Integer> combineList = new ArrayList<>();
    backtracking(combineList, combinations, 1, k, n);
    return combinations;
}

private void backtracking(List<Integer> combineList, List<List<Integer>> combinations, int start, int k, final int n) {
    if (k == 0) {
        combinations.add(new ArrayList<>(combineList));
        return;
    }
    for (int i = start; i <= n - k + 1; i++) {  // 剪枝
        combineList.add(i);
        backtracking(combineList, combinations, i + 1, k - 1, n);
        combineList.remove(combineList.size() - 1);
    }
}
```

## 8. 组合求和

39\. Combination Sum (Medium)

[Leetcode](https://leetcode.com/problems/combination-sum/description/) / [力扣](https://leetcode-cn.com/problems/combination-sum/description/)

```html
given candidate set [2, 3, 6, 7] and target 7,
A solution set is:
[[7],[2, 2, 3]]
```

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> combinations = new ArrayList<>();
    backtracking(new ArrayList<>(), combinations, 0, target, candidates);
    return combinations;
}

private void backtracking(List<Integer> tempCombination, List<List<Integer>> combinations,
                          int start, int target, final int[] candidates) {

    if (target == 0) {
        combinations.add(new ArrayList<>(tempCombination));
        return;
    }
    for (int i = start; i < candidates.length; i++) {
        if (candidates[i] <= target) {
            tempCombination.add(candidates[i]);
            backtracking(tempCombination, combinations, i, target - candidates[i], candidates);
            tempCombination.remove(tempCombination.size() - 1);
        }
    }
}
```

## 9. 含有相同元素的组合求和

```html
给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

解集不能包含重复的组合。 

```

```html

为了使得解集不包含重复的组合。有以下 2 种方案：

使用 哈希表 天然的去重功能，但是编码相对复杂；

使用和第 39 题和第 15 题（三数之和）类似的思路：不重复就需要按顺序搜索， 在搜索的过程中检测分支是否会出现重复结果 。

注意：这里的顺序不仅仅指数组 candidates 有序，还指按照一定顺序搜索结果。

数组 candidates 有序，是 深度优先遍历 过程中实现「剪枝」的前提.

```
```c
class Solution {
public:
void dfs(vector<int> &candidates,int target,int left,int right,vector<vector<int>>& res,vector<int>& v,vector<int>& visited)
{
    if(target==0)
    {
        res.push_back(v);
        //visited.assign(candidates.size(),0);
        return;
    }
    for(int i=left;i<candidates.size()&&(candidates[i]<=target);++i)
    //全排列里面 是所有数字都要用到 所以i一直从0开始
    //而这个里面所有数字不一定都用到 只是判断是不是加到了target
    {
        //if(candidates[i]>target) continue;
        if(i>left && candidates[i]==candidates[i-1]) continue;
        /*
         这个避免重复当思想是在是太重要了。
         这个方法最重要的作用是，可以让同一层级，不出现相同的元素。即
                           1
                          / \
                         2   2  
                        /     \
                       5       5
                           例1
         这种情况不会发生 但是却允许了不同层级之间的重复即：
                         
                           1
                          /
                         2      这种情况确是允许的
                        /
                       2  
                           例2
         为何会有这种神奇的效果呢？
         首先 cur-1 == cur 是用于判定当前元素是否和之前元素相同的语句。这个语句就能砍掉例1。
         可是问题来了，如果把所有当前与之前一个元素相同的都砍掉，那么例二的情况也会消失。 
         因为当第二个2出现的时候，他就和前一个2相同了。

         那么如何保留例2呢？
         那么就用cur > begin 来避免这种情况，你发现例1中的两个2是处在同一个层级上的，
         例2的两个2是处在不同层级上的。
         在一个for循环中，所有被遍历到的数都是属于一个层级的。我们要让一个层级中，
         必须出现且只出现一个2，那么就放过第一个出现重复的2，但不放过后面出现的2。
         第一个出现的2的特点就是 cur == begin. 第二个出现的2 特点是cur > begin.

        */
        //if(visited[i]) continue;
        v.push_back(candidates[i]);
        target-=candidates[i];
        //visited[i]=1;
        dfs(candidates,target,i+1,candidates.size(),res,v,visited);
        target+=candidates[i];
        //visited[i]=0;
        v.pop_back();
    }
}
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        if(candidates.empty()) return {};
        vector<vector<int>> res;
        vector<int> visited(candidates.size(),0);
        vector<int> v;
        sort(candidates.begin(),candidates.end());
        dfs(candidates,target,0,candidates.size(),res,v,visited);
        return res;
    }
};
```

40\. Combination Sum II (Medium)

[Leetcode](https://leetcode.com/problems/combination-sum-ii/description/) / [力扣](https://leetcode-cn.com/problems/combination-sum-ii/description/)

```html
For example, given candidate set [10, 1, 2, 7, 6, 1, 5] and target 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

```java
public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    List<List<Integer>> combinations = new ArrayList<>();
    Arrays.sort(candidates);
    backtracking(new ArrayList<>(), combinations, new boolean[candidates.length], 0, target, candidates);
    return combinations;
}

private void backtracking(List<Integer> tempCombination, List<List<Integer>> combinations,
                          boolean[] hasVisited, int start, int target, final int[] candidates) {

    if (target == 0) {
        combinations.add(new ArrayList<>(tempCombination));
        return;
    }
    for (int i = start; i < candidates.length; i++) {
        if (i != 0 && candidates[i] == candidates[i - 1] && !hasVisited[i - 1]) {
            continue;
        }
        if (candidates[i] <= target) {
            tempCombination.add(candidates[i]);
            hasVisited[i] = true;
            backtracking(tempCombination, combinations, hasVisited, i + 1, target - candidates[i], candidates);
            hasVisited[i] = false;
            tempCombination.remove(tempCombination.size() - 1);
        }
    }
}
```

## 10. 1-9 数字的组合求和

216\. Combination Sum III (Medium)

[Leetcode](https://leetcode.com/problems/combination-sum-iii/description/) / [力扣](https://leetcode-cn.com/problems/combination-sum-iii/description/)

```html
Input: k = 3, n = 9

Output:

[[1,2,6], [1,3,5], [2,3,4]]
```

从 1-9 数字中选出 k 个数不重复的数，使得它们的和为 n。

```java
public List<List<Integer>> combinationSum3(int k, int n) {
    List<List<Integer>> combinations = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    backtracking(k, n, 1, path, combinations);
    return combinations;
}

private void backtracking(int k, int n, int start,
                          List<Integer> tempCombination, List<List<Integer>> combinations) {

    if (k == 0 && n == 0) {
        combinations.add(new ArrayList<>(tempCombination));
        return;
    }
    if (k == 0 || n == 0) {
        return;
    }
    for (int i = start; i <= 9; i++) {
        tempCombination.add(i);
        backtracking(k - 1, n - i, i + 1, tempCombination, combinations);
        tempCombination.remove(tempCombination.size() - 1);
    }
}
```

## 11. 子集

78\. Subsets (Medium)

[Leetcode](https://leetcode.com/problems/subsets/description/) / [力扣](https://leetcode-cn.com/problems/subsets/description/)

找出集合的所有子集，子集不能重复，[1, 2] 和 [2, 1] 这种子集算重复

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> subsets = new ArrayList<>();
    List<Integer> tempSubset = new ArrayList<>();
    for (int size = 0; size <= nums.length; size++) {
        backtracking(0, tempSubset, subsets, size, nums); // 不同的子集大小
    }
    return subsets;
}

private void backtracking(int start, List<Integer> tempSubset, List<List<Integer>> subsets,
                          final int size, final int[] nums) {

    if (tempSubset.size() == size) {
        subsets.add(new ArrayList<>(tempSubset));
        return;
    }
    for (int i = start; i < nums.length; i++) {
        tempSubset.add(nums[i]);
        backtracking(i + 1, tempSubset, subsets, size, nums);
        tempSubset.remove(tempSubset.size() - 1);
    }
}
```

## 12. 含有相同元素求子集

90\. Subsets II (Medium)

[Leetcode](https://leetcode.com/problems/subsets-ii/description/) / [力扣](https://leetcode-cn.com/problems/subsets-ii/description/)

```html
For example,
If nums = [1,2,2], a solution is:

[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> subsets = new ArrayList<>();
    List<Integer> tempSubset = new ArrayList<>();
    boolean[] hasVisited = new boolean[nums.length];
    for (int size = 0; size <= nums.length; size++) {
        backtracking(0, tempSubset, subsets, hasVisited, size, nums); // 不同的子集大小
    }
    return subsets;
}

private void backtracking(int start, List<Integer> tempSubset, List<List<Integer>> subsets, boolean[] hasVisited,
                          final int size, final int[] nums) {

    if (tempSubset.size() == size) {
        subsets.add(new ArrayList<>(tempSubset));
        return;
    }
    for (int i = start; i < nums.length; i++) {
        if (i != 0 && nums[i] == nums[i - 1] && !hasVisited[i - 1]) {
            continue;
        }
        tempSubset.add(nums[i]);
        hasVisited[i] = true;
        backtracking(i + 1, tempSubset, subsets, hasVisited, size, nums);
        hasVisited[i] = false;
        tempSubset.remove(tempSubset.size() - 1);
    }
}
```

## 13. 分割字符串使得每个部分都是回文数

131\. Palindrome Partitioning (Medium)

[Leetcode](https://leetcode.com/problems/palindrome-partitioning/description/) / [力扣](https://leetcode-cn.com/problems/palindrome-partitioning/description/)

```html
For example, given s = "aab",
Return

[
  ["aa","b"],
  ["a","a","b"]
]
```

```java
public List<List<String>> partition(String s) {
    List<List<String>> partitions = new ArrayList<>();
    List<String> tempPartition = new ArrayList<>();
    doPartition(s, partitions, tempPartition);
    return partitions;
}

private void doPartition(String s, List<List<String>> partitions, List<String> tempPartition) {
    if (s.length() == 0) {
        partitions.add(new ArrayList<>(tempPartition));
        return;
    }
    for (int i = 0; i < s.length(); i++) {
        if (isPalindrome(s, 0, i)) {
            tempPartition.add(s.substring(0, i + 1));
            doPartition(s.substring(i + 1), partitions, tempPartition);
            tempPartition.remove(tempPartition.size() - 1);
        }
    }
}

private boolean isPalindrome(String s, int begin, int end) {
    while (begin < end) {
        if (s.charAt(begin++) != s.charAt(end--)) {
            return false;
        }
    }
    return true;
}
```

## 14. 数独

37\. Sudoku Solver (Hard)

[Leetcode](https://leetcode.com/problems/sudoku-solver/description/) / [力扣](https://leetcode-cn.com/problems/sudoku-solver/description/)

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/0e8fdc96-83c1-4798-9abe-45fc91d70b9d.png"/> </div><br>

```java
private boolean[][] rowsUsed = new boolean[9][10];
private boolean[][] colsUsed = new boolean[9][10];
private boolean[][] cubesUsed = new boolean[9][10];
private char[][] board;

public void solveSudoku(char[][] board) {
    this.board = board;
    for (int i = 0; i < 9; i++)
        for (int j = 0; j < 9; j++) {
            if (board[i][j] == '.') {
                continue;
            }
            int num = board[i][j] - '0';
            rowsUsed[i][num] = true;
            colsUsed[j][num] = true;
            cubesUsed[cubeNum(i, j)][num] = true;
        }
        backtracking(0, 0);
}

private boolean backtracking(int row, int col) {
    while (row < 9 && board[row][col] != '.') {
        row = col == 8 ? row + 1 : row;
        col = col == 8 ? 0 : col + 1;
    }
    if (row == 9) {
        return true;
    }
    for (int num = 1; num <= 9; num++) {
        if (rowsUsed[row][num] || colsUsed[col][num] || cubesUsed[cubeNum(row, col)][num]) {
            continue;
        }
        rowsUsed[row][num] = colsUsed[col][num] = cubesUsed[cubeNum(row, col)][num] = true;
        board[row][col] = (char) (num + '0');
        if (backtracking(row, col)) {
            return true;
        }
        board[row][col] = '.';
        rowsUsed[row][num] = colsUsed[col][num] = cubesUsed[cubeNum(row, col)][num] = false;
    }
    return false;
}

private int cubeNum(int i, int j) {
    int r = i / 3;
    int c = j / 3;
    return r * 3 + c;
}
```

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
## 17. 第k个排列

[leetcode-60.第k个排列](https://leetcode-cn.com/problems/permutation-sequence/)

返回[1,2,3...n]全排列的第k个

```c
/*
时间复杂度：o(n^2)
空间复杂度：o(n)
*/
class Solution {
public:
    //计算阶乘
    void calculateFactorial(int n,vector<int>& factorial)
    {
        factorial[0]=1;
        for(int i=1;i<=n;++i)
        {
            factorial[i]=factorial[i-1]*i;
        }
    }
    void dfs(int index,int& k,string& path,int n,vector<int>& factorial,vector<bool>& used)
    {
        if(index==n) return;//index代表深度 表示当前在哪一层
        int count=factorial[n-1-index];
        for(int i=1;i<=n;++i)//每一层都要挑数字
        {
            if(used[i]) continue;
            if(count<k)
            {
                k-=count;
                continue;
            }
            path.push_back(i+'0');
            used[i]=true;
            dfs(index+1,k,path,n,factorial,used);
            return;//不需要状态重置，直接返回，后面的也不需要再进行了
        }
    }
    string getPermutation(int n, int k) {
        vector<int> factorial(n+1);
        vector<bool> used(n+1,false);
        calculateFactorial(n,factorial);
        string path;
        dfs(0,k,path,n,factorial,used);
        return path;
    }
};
```

