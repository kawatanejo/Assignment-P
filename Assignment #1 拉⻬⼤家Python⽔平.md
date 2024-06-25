Assignment #1: 拉⻬⼤家Python⽔平

2024 spring, Complied by 吴至超 城环

*说明： 1）数算课程的先修课是计概，由于计概学习中可能使⽤了不同的编程语⾔，⽽数算课程要求Python语⾔，因此第 ⼀周作业练习Python编程。如果有同学坚持使⽤C/C++，也可以，但是建议也要会Python语⾔。 2）请把每个题⽬解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含 Accepted），填写到下⾯作业模版中（推荐使⽤ typora https://typoraio.cn ，或者⽤word）。AC 或者没有AC， 都请标上每个题⽬⼤致花费时间。 3）课程⽹站是Canvas平台, https://pku.instructure.com, 学校通知3⽉1⽇导⼊选课名单后启⽤。作业写好后，保 留在⾃⼰⼿中，待3⽉1⽇提交。 提交时候先提交pdf⽂件，再把md或者doc⽂件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交⽂件有 pdf、"作业评论"区有上传的md或者doc附件。 4）如果不能在截⽌前提交作业，请写明原因。*



 操作系统：Windows11

 编程环境：PyCharm 2023.2.3



1. 题⽬ 20742: 泰波拿契數 http://cs101.openjudge.cn/practice/20742/ 

   思路：类比斐波那契数列进行递归

   代码：

   ```python
   from functools import lru_cache
   
   @lru_cache(maxsize=1000000000)
   def tai(n):
       if n == 2:
           return 1
       elif n == 1 or n == 0:
           return 1 if n == 1 else 0
       else:
           return tai(n - 1) + tai(n - 2) + tai(n - 3)
   
   n = int(input("Enter a number: "))
   print(tai(n))
   ```

代码运⾏截图：![image-20240226224839359](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240226224839359.png)







58A. Chat room greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A 

思路： 设计一个i与str(hello),一个字母一个字母地判定，最后根据i的数值判断是否符合题意

代码 ：

```python
s=input()
t="hello"
j=0
for i in range(len(s)):
    if s[i]==t[j]:
        j+=1
        if j==5:
            print("YES")
            break
if j !=5:
    print("NO")
```

代码运⾏截图 （⾄少包含有"Accepted"）

![image-20240226225249715](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240226225249715.png)









118A. String Task implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A 

思路：

 代码 

```python
s = input("Enter a string: ")
s_upper = s.upper()  # 将字符串转换为大写，但未保存结果
ss = s.lower()  # 将字符串转换为小写
ans = ""
vowels = ["a", "e", "y", "o", "u", "i"]

for char in ss:
    if char not in vowels:
        ans += "."
        ans += char

print(ans)
```

代码运⾏截图 （AC代码截图，⾄少包含有"Accepted"）

![image-20240226232440184](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240226232440184.png)









22359: Goldbach Conjecture http://cs101.openjudge.cn/practice/22359/ 

思路： 取一半遍历，判断是否符合双不等素数

代码：

```python
def is_prime(a):
    if a == 1:
        return 1  # 1 表示非素数
    elif a == 2:
        return 0  # 0 表示素数
    else:
        for i in range(2, int(a**0.5) + 1):
            if a % i == 0:
                return 1  # 1 表示非素数
        return 0  # 0 表示素数

s = int(input("Enter a number: "))
for i in range(2, s // 2 + 1):
    flag = 0
    m = i
    n = s - i
    if is_prime(m) == 0 and is_prime(n) == 0 and m != n:
        print(m, n)


```



代码运⾏截图

![image-20240226232145601](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240226232145601.png)











23563: 多项式时间复杂度 http://cs101.openjudge.cn/practice/23563/ 

思路：用split（“”）来分离出数字，建立二维数组，lambda降序排序

代码：

```python
sample = input("Enter a sample: ")
a = [x for x in sample.split("+")]

# 初始化dp列表
dp = [[0]] * len(a)

# 在a列表中以字符串 'n' 开头的元素，添加'1'作为系数
for t in range(len(a)):
    if a[t][0] == "n":
        a[t] = '1' + a[t]

m = ''
# 将字符串转换为二维列表dp
for i in range(len(a)):
    m = a[i]
    dp[i] = [int(x) for x in m.split("n^")]

# 按照指数大小对dp列表进行排序
dp.sort(key=lambda x: -x[1])

max_exp = 0
# 寻找最大的指数
for i in range(len(a)):
    if dp[i][0] != 0 and dp[i][1] != 1:
        max_exp = dp[i][1]
        break
    elif dp[i][0] != 0 and dp[i][1] == 1:
        max_exp = 0
        break

print(f"n^{max_exp}")

```

代码运⾏截图 （AC代码截图，⾄少包含有"Accepted"） 

![image-20240226232755088](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240226232755088.png)

 









24684: 直播计票 http://cs101.openjudge.cn/practice/24684/ 

思路： 建立长度为100001的列表

代码 ：

```python
x = [int(x) for x in input("Enter a list of integers separated by spaces: ").split()]

# 统计每个数字出现的次数
calculate = [0] * 100001
for i in x:
    calculate[i] += 1

# 找到出现次数最多的数字
max_count = max(calculate)

# 找到所有出现次数最多的数字
most_frequent = [m for m in range(len(calculate)) if calculate[m] == max_count]

# 输出结果
if len(most_frequent) > 1:
    for n in range(len(most_frequent) - 1):
        print(most_frequent[n], end=' ')
    print(most_frequent[-1])
else:
    print(most_frequent[0])

```

代码运⾏截图 （AC代码截图，⾄少包含有"Accepted"）

![image-20240226233128691](C:\Users\max\AppData\Roaming\Typora\typora-user-images\image-20240226233128691.png)











2. **学习总结和收获 如果作业题⽬简单，有否额外练习题⽬，⽐如：OJ“数算pre每⽇选做”、CF、LeetCode、洛⾕等⽹站题⽬**

1.总体而言本次题目较简单，但还是把上学期期中机考之后的知识捡回来一些，并且尝试运用了一些之前没尝试过的语法。

如：新了解了map映射函数

```python
#map映射（函数，想要变换的数）
a = input().split(" ")
a = list(map(int, a))
print(a)
```



2.努力跟每日选做

