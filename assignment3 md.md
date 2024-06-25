# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024

2024 spring, Complied by ==吴至超 城环



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows 11

Python编程环境：pycharm2023.2.3





## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/



思路：动态规划

1.建立一个长度为导弹个数的列表dp，初始值都为1（省去遍历第一个），dp[i]意义在于：对于missile[0:i+1]的最长减序列的长度

2.状态转移，局部最优解逐渐扩大为全局最优解。对missile[i]以前的每一个数字遍历，如果missile[j]>=missile[i],表明原最优解可以再加1变长

一次次遍历完之后，得到新的局部最优解



##### 代码

```python
k=int(input())
missile=[int(x) for x in input().split()]
dp=[1]*k

for i in range(1,k):#循环，dp[i]表示[0，i]的最长增序列
    for j in range(i):
        if missile[i]<=missile[j]:
            dp[i]=max(dp[i],dp[j]+1)
print(max(dp))

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240310155425863](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240310155425863.png)





**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147



思路：递归思想，过渡柱和目标柱的变换有点复杂。





##### 代码

```python
#汉诺塔
n,a,b,c=map(str,input().split())
n=int(n)
def hannuo(n,a,b,c):#n层，a，b，c柱子
    if n==1:
        print(f"1:{a}->{c}")
    else:
        hannuo(n-1,a,c,b)
        print(f"{n}:{a}->{c}")
        hannuo(n-1,b,a,c)
hannuo(n,a,b,c)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240310155555010](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240310155555010.png)



**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253



思路：一边进另一边出。

当列表长度大于p时，踢去p位置的人

当列表长度小于p时，踢去末位置的人



##### 代码

```python
flag=1

while flag==1:
    pp = []
    n,p,m=(int(x) for x in input().split())
    if n==0:
        flag=0
        break
    else:
        alist=[]
        alist=list(range(1,n+1))
        while len(alist)>=1:
            if len(alist) >= p:
                for u in range(1,m):
                    alist.insert(len(alist)-1,alist.pop(0))
                pp.append(alist.pop(p-1))
            else:
                for u in range(1,m+1):
                    alist.insert(len(alist) - 1, alist.pop(0))
                pp.append(alist.pop())

        print(",".join(map(str,pp)))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310155847298](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240310155847298.png)



**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554



思路：贪心，观察样例也可以猜出。

对于n个人，壹人消耗时间为t1，贰人消耗t2(大于t1)，如果把贰人放在第一位，那团队耗时（n-1）*t2，肯定大于壹人在前只需消耗（n-1）*t1

##### 代码

```python
n=int(input())
T=[int(x) for x in input().split()]
for i in range(len(T)):
    T[i]=[i+1,T[i]]
T.sort(key=lambda x : (x[1],x[0]))

cac=0

if len(T)==1:
    print(T[0][0])
    print("0.00")
else:
    for m in range(len(T)-1):
        print(T[m][0],end=" ")
        cac+=(len(T)-m-1)*T[m][1]

    print(T[-1][0])
    print(f"{cac/n:.2f}")
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310160349760](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240310160349760.png)



**19963:买学区房**

http://cs101.openjudge.cn/practice/19963



思路：正常逻辑，就是要避免a=[1,2,3],f=a,f.sort()会对a进行排序的情况（指针），如要避免，新建一个f，然后把a的元素一个个添加进去。



##### 代码

```python
n=int(input())
distance=[x[1:len(x)-1] for x in input().split()]
price=[int(x) for x in input().split()]
newdistance=[]
for i in distance:
    x,y=i.split(",")
    x,y=int(x),int(y)
    newdistance.append(x+y)
price_pos=[]
for m in range(n):
    price_pos.append(newdistance[m]/price[m])
#找中位数
ee=[int(x) for x in price]
ee.sort()
ff=[float(x) for x in price_pos]
ff.sort()
if n%2==0:
    price_mid=(ee[n//2]+ee[n//2-1])/2
    price_posmid=(ff[n//2]+ff[(n//2)-1])/2
else:
    price_mid=ee[(n-1)//2]
    price_posmid=ff[(n-1)//2]
cnt=0
for p in range(n):
    if price_pos[p]>price_posmid and price[p]<price_mid:
        cnt+=1
print(cnt)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310160423526](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240310160423526.png)



**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300



思路：注意字典sorted（）会生成一个已经sort好的列表，正合我意。



##### 代码

```python
n=int(input())
dict={}
for i in range(n):
    m=input()
    x,y=m.split("-")
    if x not in dict:
        dict[x]=[]
    dict[x].append(y)
so=sorted(dict)
for i in so: #key
    jilu=[]
    for m in range(len(dict[i])): #value
        if dict[i][m][-1]=="B":
            jilu.append([dict[i][m],float(dict[i][m][0:-1:1])*1000000000])
        else:

            jilu.append([dict[i][m],float(dict[i][m][0:-1:1]) * 1000000])
    jilu.sort(key=lambda x: x[1])
    print(i+":",end=" ")
    for t in range(len(jilu)-1):
        print(jilu[t][0]+",",end=" ")
    print(jilu[-1][0])

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240310160656354](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240310160656354.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

至于月考，个人心理素质比较差，看到旁边大佬飞速做完会感觉到很强大的压迫感，希望下次月考能把心思更多放在自己思考的层面。

对递归与动态规划的理解并不是很好，底子很薄，学习完第一第二题之后又明白一些。第三题回来看课件做出1.0版本，2.0版本并不难。第四题也简单。第五第六题只是字多反而很简单，却被标成T，下次遇到不会做的别吊死，多看看其他的题目有没有思路。

原本打算周六周日要花时间在数算上的，不幸的是周六凌晨突然发烧，不得不休息。下周周中也可以多练练，不一定全砸在周末。

