# Assignment #4: 排序、栈、队列和树

Updated 0005 GMT+8 March 11, 2024

2024 spring, Complied by 城环 吴至超



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows11

Python编程环境：pycharm2023.2.3



## 1. 题目

### 05902: 双端队列

http://cs101.openjudge.cn/practice/05902/



思路：简单，正常语法



代码

```python
# t=int(input())
for i in range(t):
    queue=[]
    n=int(input())
    for u in range(n):#读取操作数据
        x,y=input().split()
        if x=="1":
            queue.append(y)
        if x =="2" and y=="0":
            queue.pop(0)
        if x=="2" and y=="1":
            queue.pop()
    if queue:
        print(" ".join(map(str,queue)))
    else:
        print("NULL")

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240317194356044](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240317194356044.png)



### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/



思路：用栈的思路比较简单，递归的思路比较巧妙。

eval（）可以运行字符串中的运算式子。



代码

```python
# #波兰表达式
s=list(map(str,input().split()))
def bolan():
    a=s.pop(0)
    if a in "+-*/":
        return str(eval(bolan()+a+bolan()))
    else:
        return a
print(f"{float(bolan()):.6f}")

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240317194525053](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240317194525053.png)



### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/



思路：shunting yard算法



代码

```python
# 
n=int(input())

#建立一个符号优先级
char={"+":1,"-":1,"*":2,"/":2,"(":0}
for i in range(n):
    sample=input()
    #处理一下数据
    samlist=list(sample)
    sam_list=[]
    tempo = ""
    for p in samlist:
        if p in "+-*/()" :
            if tempo:
                sam_list.append(tempo)
                tempo=""
            sam_list.append(p)
        else:
            if p.isdigit() or p==".":
                tempo+=p
    if tempo!="":
        sam_list.append(tempo)
    caculate=[]#运算集合
    pre_print=[]#输出集合
    for b in sam_list:
        if b=="(":
            caculate.append(b)
            #接下来遇到运算符：如果优先级比caculate的栈顶大或者caculate的栈顶是左括号或者是caculate是空集，则加入caculate，否则一直弹出caculate栈顶直到满足条件为止
        elif b in "+-*/" :
            #判断空集先放前面
            while (caculate and char[b] <= char[caculate[-1]]):
                pre_print.append(caculate.pop())
            caculate.append(b)
        elif b==")":
            while caculate[-1]!="(":
                pre_print.append(caculate.pop())
            caculate.pop()
        else:
            pre_print.append(b)
    if caculate:
        pre_print.extend(caculate[::-1])
    print(" ".join(map(str,pre_print)))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



![image-20240317194648350](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240317194648350.png)

### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/



思路：栈的思想



代码

```python
# 
yuan=input()
while True:
    try:
        stack=[]
        a=input()#测试样例
        c=list(yuan)
        flag=0
        if len(a)!=len(yuan):
            print("NO")

        else:
            for i in a:#对待输入数据的一个个字母进行判断
                while ( not stack or stack[-1]!=i) and c:
                    stack.append(c.pop(0))

                if  stack[-1]==i:
                    stack.pop(-1)
                else:
                    flag=1
                    break
            if flag==0:
                print("YES")
            else:
                print("NO")
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240317194907449](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240317194907449.png)



### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/



思路：构建class 树。



代码

```python
# class treenode:
    def __init__(self):
        self.left=None
        self.right=None

n=int(input())
ab=[treenode() for _ in range(n)]#列表中每一位都是treenode化的实例
for m in range(n):
    ab[m].left,ab[m].right=map(int,input().split())

def measure(treenode,ab):
    if treenode.left!=-1 and treenode.right!=-1:
        return 1+max(measure(ab[treenode.left-1],ab),measure(ab[treenode.right-1],ab))
    elif treenode.left==-1 and treenode.right==-1:#基本结束条件
        return 1
    elif treenode.left!=-1 and treenode.right==-1:
        return 1+measure(ab[treenode.left-1],ab)
    else:
        return 1+measure(ab[treenode.right-1],ab)
print(measure(ab[0],ab))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240317194941825](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240317194941825.png)



### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/



思路：归并排序求逆序数



代码

```python
# flag=0
while flag==0:
    n=int(input())
    if n==0:
        flag=1
        break
    arr=[]
    for i in range(n):
        arr.append(int(input()))
    cnt=0


    def merge_div(arr):

        if len(arr) == 1:
            return arr
        mid = len(arr) // 2
        left = arr[:mid]
        right = arr[mid:]
        # 开始递归
        left = merge_div(left)  # 返回排好序的左边
        right = merge_div(right)  # 返回排好序的右边
        return merged(left, right)


    def merged(left, right):
        global cnt
        cc = []
        in_left = in_right = 0
        while in_left < len(left) and in_right < len(right):
            if left[in_left] <= right[in_right]:
                cc.append(left[in_left])
                in_left += 1

            else:
                cc.append(right[in_right])
                in_right += 1
                cnt += len(left)-in_left

        cc.extend(left[in_left::])
        cc.extend(right[in_right::])
        return cc
    ac=merge_div(arr)
    print(cnt)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240317195140200](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240317195140200.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

1.感觉本周的难度还是蛮大的）自己觉得有些东西比如shunting yard算法啥的短时间内自己肯定想不出来，于是有些题目都是看答案理解大致思路以后再自己从头写一遍，基本上会有很多bug，然后慢慢debug慢慢纠正理解，应该也是一种学习吧。

2.并且在这个过程中继续加深了对递归的理解，比如波兰表达式和归并排序。

3.原来切片也会引起时间复杂度的增加。

4.把pre的积木和浇水给写了，都属于思路比较清晰不太涉及算法的题目，其中积木了解了全排列的包。

5.总的来说还是学了很多，希望下次遇到类似的思路可以回想起来。



