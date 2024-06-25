# sunnywhy——bfs与dfs专题

## bfs（宽度优先搜索）

### 特点：步长为一，一圈圈向外寻找答案

### 适用数据结构：队列

### 例题：

#### 1、数字操作[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/2/318)

思路：类似pots，find the multiple

代码

```python
from collections import deque
desti=int(input())
def bfs(n):
    queue=deque()
    vis=set()
    queue.append((1,0))
    vis.add(1)
    while queue:
        pro=queue.popleft()
        newpro1=pro[0]+1
        newpro2=pro[0]*2

        if newpro1 not in vis:
            if newpro1==n:
                return pro[1]+1
            queue.append((newpro1,pro[1]+1))
            vis.add(newpro1)

        if newpro2 not in vis:
            if newpro2==n:
                return pro[1]+1
            queue.append((newpro2,pro[1]+1))
            vis.add(newpro2)
print(bfs(desti))
```

截图：

![image-20240624175113612](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624175113612.png)



#### 2、矩阵中的块 [晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/2/319)

思路：类似算鹰和水坑

代码：

```python
from collections import deque
n,m=map(int,input().split())
matrix=[]
for i in range(n):
    sample=[int(x) for x in input().split()]
    matrix.append(sample)
cnt=0
moves=[[1,0],[0,1],[-1,0],[0,-1]]
def bfs(startx,starty):
    queue=deque()
    queue.append((startx,starty))
    vis=set()
    vis.add((startx,starty))
    while queue:
        tempo=queue.popleft()
        x=tempo[0]
        y=tempo[1]
        matrix[x][y] = 0
        for i in moves:
            newx,newy=x+i[0],y+i[1]
            if 0<=newx<n and 0<=newy<m and matrix[newx][newy]==1 :
                queue.append((newx,newy))
                vis.add((newx,newy))
for i in range(n):
    for c in range(m):
        if matrix[i][c]==1:
            bfs(i,c)
            cnt+=1
          
print(cnt)

```

截图：

![image-20240624174753603](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624174753603.png)

#### 3、迷宫问题[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/2/320)

思路：典型

代码：

```python
from collections import deque
n,m=map(int,input().split())
matrix=[]
moves=[[0,1],[1,0],[0,-1],[-1,0]]
for i in range(n):
    sample=[int(x) for x in input().split()]
    matrix.append(sample)
def bfs(x,y):
    queue=deque()
    vis=set()
    queue.append((0,0,0))
    vis.add((0,0))
    while queue:
        tempo=queue.popleft()
        xx,yy=tempo[0],tempo[1]
       
        times=tempo[2]
        for i in moves:
            newx=xx+i[0]
            newy=yy+i[1]
            if 0<=newx<n and 0<=newy<m :
                if newx == n - 1 and newy == m - 1:
                    return tempo[2] + 1
                if matrix[newx][newy]==0 and (newx,newy)not in vis:
                    queue.append((newx,newy,tempo[2]+1))
                    vis.add((newx,newy))

    return -1
print(bfs(0,0))

```

截图：

![image-20240624174900235](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624174900235.png)

#### 4、迷宫最短路径[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/2/321)

思路：要记录路径，就要记录前驱，用字典来建立

代码：

```python
from collections import deque
n,m=map(int,input().split())
matrix=[]
moves=[[0,1],[1,0],[0,-1],[-1,0]]
for i in range(n):
    sample=[int(x) for x in input().split()]
    matrix.append(sample)
previous={}
def bfs(x,y):
    queue=deque()
    vis=set()
    queue.append((0,0,0))
    vis.add((0,0))
    previous[(0,0)]=None
    while queue:
        tempo=queue.popleft()
        xx,yy=tempo[0],tempo[1]
        times=tempo[2]
        for i in moves:
            newx=xx+i[0]
            newy=yy+i[1]
            if 0<=newx<n and 0<=newy<m :
                if matrix[newx][newy]==0 and (newx,newy)not in vis:
                    queue.append((newx,newy,tempo[2]+1))
                    if  (newx,newy)not in previous:
                        previous[(newx,newy)]=[xx,yy]

                    vis.add((newx,newy))

#如果单纯求解最少次数之类的可以立即add，如果每一步路径有差异需要heappop再add，需要付出时间的代价
#这道题如果不立即add，会导致冗杂，会有重复入队的情况
ans=[]
def traverse(x,y):
    while True:
        if previous[(x,y)]!=None:
            tempo=previous[(x,y)]
            ans.append((x+1,y+1))
            x,y=tempo[0],tempo[1]
        else:
            ans.append((x+1,y+1))
            break

bfs(0,0)
traverse(n-1,m-1)
for i in ans[::-1]:
    print(i[0],i[1])
```

截图：

![image-20240624185441429](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624185441429.png)

#### 5、跨步迷宫[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/2/322)

思路：可走一格或两格，那么就**根据路况**把可能的情况都加到队列里

代码：

```python
from collections import deque

n, m = map(int, input().split())
matrix = []
moves = [[0,1],[0,2],[2,0],[1,0],[0,-2],[0,-1],[-2,0],[-1,0]]
for i in range(n):
    sample = [int(x) for x in input().split()]
    matrix.append(sample)


def bfs(x, y):
    queue = deque()
    vis = set()
    queue.append((0, 0, 0))
    vis.add((0, 0))
    while queue:
        tempo = queue.popleft()
        xx, yy = tempo[0], tempo[1]

        times = tempo[2]
        for i in moves:
            newx = xx + i[0]
            newy = yy + i[1]

            if 0 <= newx < n and 0 <= newy < m:

                if i[0]==2:
                    if matrix[newx-1][newy] == 0 and matrix[newx][newy]==0 and (newx, newy) not in vis:
                        queue.append((newx, newy, tempo[2] + 1))
                        vis.add((newx, newy))
                elif i[0]==-2:
                    if matrix[newx+1][newy] == 0 and matrix[newx][newy]==0 and (newx, newy) not in vis:
                        queue.append((newx, newy, tempo[2] + 1))
                        vis.add((newx, newy))
                elif i[1]==2:
                    if matrix[newx][newy-1] == 0 and matrix[newx][newy]==0 and (newx, newy) not in vis:
                        queue.append((newx, newy, tempo[2] + 1))
                        vis.add((newx, newy))
                elif i[1]==-2:
                    if matrix[newx][newy+1] == 0 and matrix[newx][newy] == 0 and (newx, newy) not in vis:
                        queue.append((newx, newy, tempo[2] + 1))
                        vis.add((newx, newy))
                else:
                    if matrix[newx][newy]==0 and (newx, newy) not in vis:
                        queue.append((newx, newy, tempo[2] + 1))
                        vis.add((newx, newy))
                if (n-1,m-1) in vis:
                    return tempo[2] + 1

    return -1


print(bfs(0, 0))
```

截图：![image-20240624191344132](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624191344132.png)

#### 6、字符迷宫[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/2/323)

思路：典型，只是把数字换成了字符

代码：

```python
from collections import deque

n, m = map(int, input().split())
matrix = []
moves = [[0, 1], [1, 0], [0, -1], [-1, 0]]
starty,startx,flag=0,0,0
for i in range(n):
    sample = list(input())
    if flag==0:
        for t in range(len(sample)):
            if sample[t]=="S":
                starty=t
                startx=i
                flag=1
                break
    matrix.append(sample)

def bfs(x, y):
    queue = deque()
    vis = set()
    queue.append((x, y, 0))
    vis.add((x, y))
    while queue:

        tempo = queue.popleft()
        xx, yy = tempo[0], tempo[1]
        times = tempo[2]
        for i in moves:
            newx = xx + i[0]
            newy = yy + i[1]
            if 0 <= newx < n and 0 <= newy < m:
                if matrix[newx][newy]=="T":
                    return tempo[2] + 1
                if matrix[newx][newy] == "." and (newx, newy) not in vis:
                    queue.append((newx, newy, tempo[2] + 1))
                    vis.add((newx, newy))
    return -1


print(bfs(startx, starty))
```

截图：![image-20240624191535896](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624191535896.png)

7、多终点迷宫问题[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/2/324)

思路：弹出时记录到该点的步数即为最短路径

代码：

```python
from collections import deque

n, m = map(int, input().split())
matrix = []
moves = [[0, 1], [1, 0], [0, -1], [-1, 0]]

for i in range(n):
    sample = [int(x) for x in input().split()]
    matrix.append(sample)
dis=[]
for i in range(n):
    sample=[False]*m
    dis.append(sample)

def bfs(x, y):
    queue = deque()
    vis = set()
    queue.append((x, y, 0))
    vis.add((x, y))
    while queue:
        tempo = queue.popleft()
        xx, yy = tempo[0], tempo[1]
        dis[xx][yy]=tempo[2]
        for i in moves:
            newx = xx + i[0]
            newy = yy + i[1]
            if 0 <= newx < n and 0 <= newy < m:
                if matrix[newx][newy] == 0 and (newx, newy) not in vis:
                    queue.append((newx, newy, tempo[2] + 1))
                    vis.add((newx, newy))
bfs(0,0)
for v in dis:
    ans=[]
    for i in range(m):
        if v[i]!=False:
            ans.append(str(v[i]))
        else:
            ans.append(str(-1))
    if v==dis[0]:
        ans[0]="0"
    print(" ".join(ans))

```

截图：![image-20240624192649978](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624192649978.png)

#### 7、迷宫问题-传送点[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/2/325)

思路：要考虑几种情况：起点和传送点可以到达终点？传送走的快还是不传送走得快？

所以开3个bfs，一组从起点为开头，正常记录到终点的步数，若可以到达记作S1，到达传送点1距离S2，到达传送点2距离S3；另一组以传送点1为开头，同样记录到终点的步数，若可到达终点则距离记作d1，将S3+d1添加到ans列表作备选答案；第三组同理，添加S2+d2到ans列表。若情况理想，应出现三种可能，起点-终点、起点-传送1-传送2-终点，起点-传送2-传送1-终点，取最小即可，若备选答案列表为空，则输出-1.

代码：

```python
from collections import deque
n, m = map(int, input().split())
matrix = []
moves = [[0, 1], [1, 0], [0, -1], [-1, 0]]
flag=0
sofa=[]
for i in range(n):
    sample = [int(x) for x in input().split()]
    if flag==0:
        for t in range(m):
            if sample[t]==2:
                sofa.append((i,t))
                if len(sofa)==2:
                    flag=1
                break
    matrix.append(sample)
def bfs(x, y):
    dis = {}
    for i in range(n):
        for c in range(m):
            dis[(i, c)] = -1
    queue = deque()
    vis = set()
    queue.append((x, y, 0))
    vis.add((x, y))
    while queue:
        tempo = queue.popleft()
        xx, yy = tempo[0], tempo[1]
        dis[(xx,yy)]=tempo[2]
        for i in moves:
            newx = xx + i[0]
            newy = yy + i[1]
            if 0 <= newx < n and 0 <= newy < m:
                if (matrix[newx][newy] == 0 or matrix[newx][newy]==2) and (newx, newy) not in vis:
                    queue.append((newx, newy, tempo[2] + 1))
                    vis.add((newx, newy))
    return dis

diss=bfs(0,0)
ans=[diss[(n-1,m-1)]]#不经过传送点到右下角的距离
ans.append(diss[(sofa[0][0],sofa[0][1])])#到第一个传送点的距离
ans.append(diss[(sofa[1][0],sofa[1][1])])#到第二个传送点的距离


if ans[1]!=-1:#可以到第一个传送点
    diss1 = bfs(sofa[1][0], sofa[1][1])#二号传送点到右下角的距离
    if diss1[(n-1,m-1)]!=-1:#如果传送后可以到达右下角
        ans[1]+=diss1[(n-1,m-1)]
    else:
        ans[1]=-1


if ans[2]!=-1:
    diss2 = bfs(sofa[0][0], sofa[0][1])
    if diss2[(n-1,m-1)]!=-1:
        ans[2]+=diss2[(n-1,m-1)]
    else:
        ans[2]=-1
n=max(ans)
if n>0:
    for i in ans:
        if 0<i<n:
            n=i
else:
    print(-1)

if n>0:
    print(n)
```

截图：![image-20240624192852259](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624192852259.png)

#### 8、中国象棋-马-无障碍[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/2/326)

思路：类似马走日

代码：

```python
from collections import deque
n,m,x,y=map(int,input().split())
moves=[[-1,-2],[-2,-1],[-2,1],[-1,2],[1,-2],[2,-1],[1,2],[2,1]]
matrix=[]
for i in range(n):
    sample=["-1"]*m
    matrix.append(sample)
def bfs(x,y):
    queue=deque()
    vis=set()
    queue.append((x,y,0))
    vis.add((x,y))
    while queue:
        tempo=queue.popleft()
        xx,yy=tempo[0],tempo[1]
        matrix[xx][yy]=str(tempo[2])
        for i in moves:
            newx=xx+i[0]
            newy=yy+i[1]
            if 0<=newx<n and 0<=newy<m and (newx,newy)not in vis:
                queue.append((newx,newy,tempo[2]+1))
                vis.add((newx,newy))
bfs(x-1,y-1)
for i in matrix:
    print(" ".join(i))

```

截图：![image-20240624193943083](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624193943083.png)

#### 9、中国象棋-马-有障碍[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/2/327)

思路：在添加新节点到queue之前判断是否有马脚存在挡路

代码：

```python
from collections import deque
n,m,x,y=map(int,input().split())
moves={(0,-1):[[-1,-2],[1,-2] ],(0,1):[[1,2],[-1,2]],(1,0):[[2,1],[2,-1]],(-1,0):[[-2,-1],[-2,1]]}
matrix=[]
for i in range(n):
    sample=["-1"]*m
    matrix.append(sample)
t=int(input())
for i in range(t):
    obx,oby=map(int,input().split())
    matrix[obx-1][oby-1]="-2"

def bfs(x,y):
    queue=deque()
    vis=set()
    queue.append((x,y,0))
    vis.add((x,y))
    while queue:
        tempo=queue.popleft()
        xx,yy=tempo[0],tempo[1]
        matrix[xx][yy]=str(tempo[2])
        for i in moves:
            judx=xx+i[0]
            judy=yy+i[1]
            if 0<=judx<n and 0<=judy<m and matrix[judx][judy]!="-2":
                for g in moves[i]:
                    newx=xx+g[0]
                    newy=yy+g[1]
                    if 0<=newx<n and 0<=newy<m and matrix[newx][newy]!="-2" and (newx,newy)not in vis:
                        queue.append((newx,newy,tempo[2]+1))
                        vis.add((newx,newy))

bfs(x-1,y-1)
for i in matrix:
    for c in range(m):
        if i[c]=="-2":
            i[c]="-1"
    print(" ".join(i))
```

截图：![image-20240624204223148](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624204223148.png)





## dfs（深度优先搜索）

### 特点：一条路走到黑

### 适用数据结构：栈，不过一般用递归与回溯实现更多些

### 例题：

#### 1、迷宫可行路径数[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/1)

思路：典型

代码：

```python
n,m=map(int,input().split())
matrix=[]
moves = [[0, 1], [1, 0], [0, -1], [-1, 0]]
for i in range(n):
    sample=[int(x) for x in input().split()]
    matrix.append(sample)
ans=0
def dfs(x,y):
    global ans
    matrix[0][0]=1
    if x==n-1 and y==m-1:
        ans+=1
        return
    for i in moves:
        newx=x+i[0]
        newy=y+i[1]
        if 0<=newx<n and 0<=newy<m and matrix[newx][newy]==0:
            matrix[newx][newy]=1
            dfs(newx,newy)#递归保证了路线不会重复
            matrix[newx][newy]=0
dfs(0,0)
print(ans)
```

截图：

![image-20240624205132743](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624205132743.png)

#### 2、指定步数的迷宫问题[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/1/314)

思路：正常dfs，不过加了个步数限制条件

代码：

```python
n,m,k=map(int,input().split())
matrix=[]
moves = [[0, 1], [1, 0], [0, -1], [-1, 0]]
for i in range(n):
    sample=[int(x) for x in input().split()]
    matrix.append(sample)
flag=0
def dfs(x,y,k):
    global flag
    matrix[0][0]=1
    if x==n-1 and y==m-1 and k==0:
        flag=1
        return
    elif x==n-1 and y==m-1 and k>0:
        return
    elif k<0:
        return

    for i in moves :
        newx=x+i[0]
        newy=y+i[1]
        if 0<=newx<n and 0<=newy<m and matrix[newx][newy]==0:
            matrix[newx][newy]=1
            dfs(newx,newy,k-1)#递归保证了路线不会重复,统一性
            matrix[newx][newy]=0
dfs(0,0,k)
if flag==0:
    print("No")
else:
    print("Yes")
```

截图：![image-20240624205816449](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624205816449.png)

#### 3、矩阵最大权值[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/1/315)

思路：先把各种备用答案集中最后返回列表最大值

代码：

```python
n,m=map(int,input().split())
matrix=[]
moves = [[0, 1], [1, 0], [0, -1], [-1, 0]]
for i in range(n):
    sample=[int(x) for x in input().split()]
    matrix.append(sample)
ans=set()
start=matrix[0][0]
final=matrix[n-1][m-1]

def dfs(x,y,k):
    global ans
    matrix[0][0]="a"
    if x==n-1 and y==m-1:
        ans.add(k)
        return#纯粹的返回
    for i in moves :
        newx=x+i[0]
        newy=y+i[1]

        if 0<=newx<n and 0<=newy<m and not str(matrix[newx][newy]).isalpha():#避免往回走
            text = matrix[newx][newy]
            matrix[newx][newy]="a"#关键的回溯
            dfs(newx,newy,k+text)#递归保证了路线不会重复,统一性
            matrix[newx][newy]=text

dfs(0,0,start)
print(max(ans))
```

截图：![image-20240624210107915](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624210107915.png)

#### 4、矩阵最大权值路径[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/1/316)

思路：各种可能路径的步数与路径索引一致，通过max权值对应的索引找到对应的路径

代码：

```python
n,m=map(int,input().split())
matrix=[]
moves = [[0, 1], [1, 0], [0, -1], [-1, 0]]
for i in range(n):
    sample=[int(x) for x in input().split()]
    matrix.append(sample)
ans=[]
start=matrix[0][0]
final=matrix[n-1][m-1]
ans1=[]
def dfs(x,y,k,seq):
    matrix[0][0]="a"
    if x==n-1 and y==m-1:
        ans.append(k)
        ans1.append(seq.copy())#如果写成seq，就代表这里的东西会随着seq的变化而变化
        return#纯粹的返回
    for i in moves :
        newx=x+i[0]
        newy=y+i[1]
        if 0<=newx<n and 0<=newy<m and not str(matrix[newx][newy]).isalpha():#避免往回走
            text = matrix[newx][newy]
            matrix[newx][newy]="a"#关键的回溯
            seq.append([newx+1,newy+1])
            dfs(newx,newy,k+text,seq)#递归保证了路线不会重复,统一性
            seq.pop()
            matrix[newx][newy]=text
dfs(0,0,start,[[1,1]])

n=max(ans)
idx=ans.index(n)
for i in ans1[idx]:
    print(" ".join(map(str,i)))
```

截图：![image-20240624210324633](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624210324633.png)

#### 5、迷宫最大权值[晴问算法 (sunnywhy.com)](https://sunnywhy.com/sfbj/8/1/317)

思路：添加障碍判断即可

代码：

```python
n,m=map(int,input().split())
matrix=[]
values=[]
moves = [[0, 1], [1, 0], [0, -1], [-1, 0]]
for i in range(n):
    sample=[int(x) for x in input().split()]
    matrix.append(sample)
for t in range(n):
    sample = [int(x) for x in input().split()]
    values.append(sample)

ans=set()
start=values[0][0]
def dfs(x,y,k):
    global ans

    matrix[0][0]="a"
    if x==n-1 and y==m-1:
        ans.add(k)
        return#纯粹的返回
    for i in moves :
        newx=x+i[0]
        newy=y+i[1]

        if 0<=newx<n and 0<=newy<m and matrix[newx][newy]!=1 and not str(matrix[newx][newy]).isalpha():#避免往回走
            text = matrix[newx][newy]
            matrix[newx][newy]="a"#关键的回溯

            dfs(newx,newy,k+values[newx][newy])#递归保证了路线不会重复,统一性

            matrix[newx][newy]=text


dfs(0,0,start)

print(max(ans))

```

截图：![image-20240624210453829](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240624210453829.png)

#### 6、我是最快的马[OpenJudge - 07206:我是最快的马](http://cs101.openjudge.cn/2024sp_routine/07206/)

思路：与有障碍的马类似，只是输出比较麻烦，需要建立一个{步数：[[路径]]}的字典来记录

代码：

```python
from collections import defaultdict
startx,starty=map(int,input().split())
endx,endy=map(int,input().split())
num=max(endx,endy,startx,starty)+1
M=int(input())
majiao=set()
for i in range(M):
    x,y=map(int,input().split())
    majiao.add((x,y))
qipan=[]
ran=max(endx,endy,starty,startx)
for i in range(num):
    sample=[True]*num
    qipan.append(sample)
ans=defaultdict(list)
moves=[[-1,-2],[-2,-1],[-2,1],[-1,2],[1,-2],[2,-1],[1,2],[2,1]]
qipan[startx][starty]=False
for t in majiao:
    qipan[t[0]][t[1]]=False
def dfs(x,y,step,path):
    if x==endx and y==endy:
        ans[step].append(path.copy())
        return
    for i in moves:
        flag=0
        if i==[-1,-2] or i==[1,-2]:
            if 0<=y-1 and(x,y-1) not in majiao:
                flag=1
        elif i==[-2,-1] or i==[-2,1]:
            if 0<=x-1 and (x-1,y) not in majiao:
                flag=1
        elif i==[-1,2] or i==[1,2]:
            if y+1<10 and (x,y+1) not in majiao:
                flag=1
        elif i==[2,-1] or i==[2,1]:
            if x+1<10 and (x+1,y) not in majiao:
                flag=1
        if flag==1:
                newx = x + i[0]
                newy = y + i[1]
                if 0<=newx<num and 0<=newy<num and qipan[newx][newy]:
                    qipan[newx][newy]=False
                    path.append(f"({newx},{newy})")
                    dfs(newx,newy,step+1,path)
                    path.pop()
                    qipan[newx][newy]=True
dfs(startx,starty,0,[f"({startx},{starty})"])
a=sorted(list(ans))
a=a[0]
if len(ans[a])>1:
    print(len(ans[a]))
else:
    print("-".join(map(str,ans[a][0])))
```

截图：

![image-20240625203523246](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240625203523246.png)



