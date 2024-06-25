# Assignment #F: All-Killed 满分

Updated 1844 GMT+8 May 20, 2024

2024 spring, Complied by城环 吴至超



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：windows11

Python编程环境：pycharm2023.2.3



## 1. 题目

### 22485: 升空的焰火，从侧面看

http://cs101.openjudge.cn/practice/22485/



思路：bfs，记录层数放进vis



代码

```python
# 
from collections import deque
N=int(input())
children={i:[] for i in range(N)}
floor={i:0 for i in range(N)}

for i in range(N):
    a,b=map(int,input().split())
    if a!=-1:
        newa=a-1
        children[i].append(newa)
    if b!=-1:
        newb=b-1
        children[i].append(newb)

def addfloor(i,dep):
    floor[i]=dep
    if children[i]:
        for m in children[i]:
            addfloor(m,dep+1)
addfloor(0,1)


def bfs(children,floor):
    ans=[0]
    queue=[]
    queue.append((0,1))
    vis=set()
    vis.add(1)
    while queue:
        tempo=queue.pop()
        name=tempo[0]

        if children[name]:
            for i in children[name][::-1]:
                if i!=-1 :
                    queue.insert(0,(i,floor[i]))
                    if floor[i] not in vis:
                        ans.append(i)
                        vis.add(floor[i])
    return ans
a=bfs(children,floor)
a=[i+1 for i in a]
print(" ".join(map(str,a)))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240528225608322](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240528225608322.png)



### 28203:【模板】单调栈

http://cs101.openjudge.cn/practice/28203/



思路：那个print必须得用*解码，没懂和正常的join差别在哪。



代码

```python
# 
n=int(input())
sample=[int(x) for x in input().split()]
ans=[0]*n
stack=[]

for idx in range(n):
    if not stack or sample[idx]<=sample[stack[-1]]:
        stack.append(idx)
    else:
        while stack and sample[stack[-1]]<sample[idx]:
            ans[stack.pop()]=idx+1
        stack.append(idx)
print(*ans)


或者
n = int(input())
sample = [int(x) for x in input().split()]

ans = [0] * n
stack = []

for idx, tempo in enumerate(sample):

    if not stack or tempo <= stack[-1][1]:
        stack.append((idx, tempo))
    else:

        while stack and stack[-1][-1] < tempo:
            ans[stack.pop()[0]] = idx + 1

        stack.append((idx, tempo))
print(*ans)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240528225633087](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240528225633087.png)

![image-20240528232108548](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240528232108548.png)



### 09202: 舰队、海域出击！

http://cs101.openjudge.cn/practice/09202/



思路：有向图的拓扑排序。注意从a到b可能有多条路。



代码

```python
# 
def judge(info,indgree,N):#info 字典 储藏邻接表信息，indgree 列表，储藏入度信息
    flag=1
    vis=set()
    while flag==1:
        flag=0
        for t in range(N):
            if indgree[t]==0:
                vis.add(t)
                flag=1
                for m in info[t]:
                    indgree[m]-=1
                indgree[t]=-1
    return len(vis)



T=int(input())
for i in range(T):
    N,M=map(int,input().split())
    info={v:[] for v in range(N) }
    indgree=[0]*N
    for c in range(M):
        x,y=map(int,input().split())
        x,y=x-1,y-1
        indgree[y]+=1
        info[x].append(y)
    new=judge(info,indgree,N)
    if new<N:
        print("Yes")
    else:
        print("No")
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528225753914](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240528225753914.png)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### 07735: 道路

http://cs101.openjudge.cn/practice/07735/



思路：随意地用了字典套字典（也许增加了时间复杂度？？），注意两点间可能有多条路，一个点在某个位置如果拥有不同的金钱，可能导致的选择不一样，所以vis应该存（金钱，位置），queue存（已走路程，金钱，位置名称），联系走山路，每一步的路长并不相同，所以应该在堆弹出后加入vis，在queue弹出与vis添加之间可以判断是否来到终点，第一次来到一定是最短距离（最小堆）。



代码

```python
# 
import heapq
k=int(input())#最大金币数
N=int(input())#城市数目
R=int(input())
dic={i:{m:[] for m in range(N)} for i in range(N)}
for i in range(R):
    s,d,l,t=map(int,input().split())
    s,d=s-1,d-1
    dic[s][d].append((l,t))
    #(道路长，金钱)
def bfs(start,end,k):
    queue=[]
    queue.append((0,k,start))
    vis=set()
    while queue:
        tempo=heapq.heappop(queue)
        if tempo[2]==end:
            return tempo[0]
        vis.add((tempo[2],tempo[1]))
        for i in dic[tempo[2]].keys():
            if dic[tempo[2]][i]:
                for m in dic[tempo[2]][i]:
                    needmoney=m[1]
                    if tempo[1]>=needmoney:
                        newdis=tempo[0]+m[0]
                        if (i,tempo[1]-needmoney) not in vis:
                            heapq.heappush(queue,(newdis,tempo[1]-needmoney,i))
    return -1
print(bfs(0,N-1,k))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528231410122](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240528231410122.png)



### 01182: 食物链

http://cs101.openjudge.cn/practice/01182/



思路：



代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==



因为食物链和月度开销不考，所以就先不做了，感觉不是很好理解的东西。

其他题目感觉还ok，比较常规，就是自己迪杰斯特拉有点忘记了，一开始忘记用堆了hhh

单调栈也比较神奇，必须要*才能不超内存，奇奇怪怪不明白

周末跟着gw老师班限时模考了一次，ac7，被dp给卡了一下，虽然计概到现在都没系统讲过dp，不过还是能凭借理解做，然后自己对那些经典排序的代码不太熟悉，只知道笔试的手摸原理，在填空题那里卡了一会儿，不过根据上下文的“对称性”猜了猜就过了hhh结果还行吧，不过是在很舒适的环境下尝试的，键盘什么的用的也比较顺利

![image-20240528232646531](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240528232646531.png)

因为计概底子很薄，数算可以说是这学期花时间最多最多的科目了，尽管前几次月考都跌宕起伏的样子，还是希望最后能拿优秀吧！！马上结束了，坚持住，周末整理一下cheating paper



