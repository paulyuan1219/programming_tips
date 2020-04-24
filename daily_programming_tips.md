# 日常编程技巧
## git
### 每次生成ticket的标准流程
```
Git checkout master
Git pull
Git checkout -b topic-mrxxx
...
git push -u origin topic-mr386

Git checkout — file name # 本地修改了一堆文件(并没有使用git add到暂存区)，想放弃修改 单个文件或者文件夹
Git checkout . # 所有文件夹
```

### 新建的local工程提交至远程仓库
```
git init
// 将“README.md”文件保存至缓存区，实际开发中一般使用 `git add -A`
// 使用-A:将新增、删除、修改的文件改动全保存至缓存区
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/wteam-xq/testGit.git # origin就是远程仓库的名字，遥远的地方
git push -u origin master # 把本地的master分支推送到远程仓库origin上。实际应用中，把master替换成topic-mrxxx即可。 -u指定默认主机
```

## python

### [Counter](https://stackoverflow.com/questions/19356055/summing-the-contents-of-two-collections-counter-objects)

```
>>> from collections import Counter
>>> a = Counter({'menu': 20, 'good': 15, 'happy': 10, 'bar': 5})
>>> b = Counter({'menu': 1, 'good': 1, 'bar': 3})
>>> a + b
Counter({'menu': 21, 'good': 16, 'happy': 10, 'bar': 8})
```

### [Enumerate()](https://www.geeksforgeeks.org/enumerate-in-python/)

```
enumerate(iterable, start=0)

Parameters:
Iterable: any object that supports iteration
Start: the index value from which the counter is 
              to be started, by default it is 0
```
```
l1 = ["eat","sleep","repeat"] 
  
# printing the tuples in object directly 
for ele in enumerate(l1): 
    print ele 
print 
# changing index and printing separately 
for count,ele in enumerate(l1,100): 
    print count,ele 
    
(0, 'eat')
(1, 'sleep')
(2, 'repeat')

100 eat
101 sleep
102 repeat 
```

### [itertools.chain](https://www.geeksforgeeks.org/python-ways-to-flatten-a-2d-list/)

把二维list转化为一维

```
from itertools import chain 
  
ini_list = [[1, 2, 3], 
            [3, 6, 7], 
            [7, 5, 4]] 
              
# printing initial list 
print ("initial list ", str(ini_list)) 
  
# converting 2d list into 1d 
# using chain.from_iterables 
flatten_list = list(chain.from_iterable(ini_list)) 
  
# printing flatten_list 
print ("final_result", str(flatten_list))

```


## linux Mac 命令
+ `grep -r VideoPartial .` # 循环寻找一个pattern
+ `find . -name "foo*"` # find files with names recursively
+ `echo $PATH`
+ `which echo` # show the absolute path of this command

## Vim

### pattern related
+ `:%s/pattern//gn` # count number of matches of a pattern
+ `:10,50s/pattern//gn` # count number of matches of a pattern
+ `:%s/pattern//n` # count number of lines matching a pattern


## DNS 相关

Some useful dns related cmds3

### windows

```
ping www.google.com
ipconfig /displaydns
ipconfig /flushdns # clear out cache
nslookup www.google.com
nslookup # show local dns
```

+ `netstat -ano` 
  + `a` shows all connections, 
  + `n` shows ip addresses in stead of host names, 
  + `o` shows the process id that owns the connection


### Mac
+ `dig` querying DNS name servers for information about host addresses, mail exchanges, name servers, and related information.
+ `netstat -nr | grep default` find ip address
+ `traceroute 8.8.8.8` trace ip包

## Docker

+ `docker system prune -a` 删除所有用不到的image之类。我之前用这条命令，空出了将近90GB的空间
+ `du -sh ~/Library/Containers/com.docker.*` 显示该目录大小
+ `docker image ls`
+ `docker info`
+  