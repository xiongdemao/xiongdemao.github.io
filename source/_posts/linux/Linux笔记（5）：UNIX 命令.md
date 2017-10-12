

####1.1 Listing files and directories
     ls (list) 
      
#### 1.2 Making Directories
     mkdir (make directory) 
####1.3 Changing to a different directory 
     cd (change directory)
1.4 The directories . and .. 
    
     % cd . 

     % cd .. 

####1.5 Pathnames（路径名）
    pwd (print working directory) 

####1.6新建文件
    touch a.txt
    





| Command | Meaning|
| ------------- |:-------------:|
| ls 	| list files and directories | 
| ls -a | 	list all files and directories | 
| mkdir | 	make a directory | 
| cd | directory 	change to named directory | 
| cd 	| change to home-directory | 
| cd ~ | 	change to home-directory | 
| cd .. | 	change to parent directory | 
| pwd | 	display the path of the current directory | 




   


#### 2.1 Copying Files
     cp (copy)

```
% cp /vol/examples/tutorial/science.txt .
```
>Note: Don't forget the dot . at the end. Remember, in UNIX, the dot means the current directory.

####2.2 Moving files
     mv (move)
     
     mv file1 file2 moves (or renames) file1 to file2 
     
     % mv science.bak backups/.

####2.3 Removing files and directories
    rm (remove)(删除文件)
    rmdir (remove directory)（删除空文件夹）

####2.4 Displaying the contents of a file on the screen
    clear (clear screen)
	    % clear 
    cat (concatenate)
	    % cat science.txt 
    less
	    % less science.txt 
    head
	    % head science.txt
	    % head -5 science.txt 
    tail
	    % tail science.txt 
####2.5 Searching the contents of a file
#####        2.5.1  ***less***
	 % less science.txt
then, still in less, type a forward slash [/] followed by the word to search

	/science
			
#####        2.5.2   ***grep***
	% grep -i science science.txt 
#####        2.5.3 ***wc*** (word count)
	% grep -i science science.txt 

A handy little utility is the wc command, short for word count. To do a word count on science.txt, type

	% wc -w science.txt

To find out how many lines the file has, type

	% wc -l science.txt 


| Command | Meaning|
| ------------- |:-------------:|
| cp file1 file2  |	copy file1 and call it file2 |
| mv file1 file2 | 	move or rename file1 to file2 |
|rm file | 	remove a file |
|rmdir directory 	| remove a directory|
|cat file |	display a file |
|less file 	| display a file a page at a time |
|head file |	display the first few lines of a file |
|tail file 	| display the last few lines of a file |
|grep 'keyword' file |	search a file for keywords |
|wc file |	count number of lines/words/characters in file|

 Linux中more和less命令用法 http://www.cnblogs.com/aijianshi/p/5750911.html



### 3.
####3.1 Redirection   (重定向)
	% cat
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ cat
ddd
ddd
```
####3.2 Redirecting the Output(重定向输出)
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ cat > list1
pear
banana
apple
```
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ cat list1
pear
banana
apple
```
####3.2.1 Appending to a file (附加到文件)
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ cat >> list1
peach
grape
orange
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ cat list1
pear
banana
apple
peach
grape
orange
```
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ cat > list2
orange
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ cat list1 list2 >biglist
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ cat biglist
pear
banana
apple
peach
grape
orange
orange
```
###3.3 Redirecting the Input (重定向输入)
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ sort(排序)
dog
cat
bird
ape 
# ^D (Control D to stop) 
ape
bird
cat
dog
```
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ sort < biglist #(对列表biglist排序)
apple
banana
grape
orange
# ^D (Control D to stop) 
orange
peach
pear
```
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ sort < biglist > slist   #(对biglist排序并保存到slist)
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ cat slist
apple
banana
grape
orange
orange
peach
pear
```
###3.4 Pipes
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ who
kuo      :0           2017-09-20 14:32 (:0)
kuo      pts/1        2017-09-20 14:41 (:0)
```

|Command |	Meaning|
| ------------- |:-------------:|
|  command > file |	redirect standard output to a file |
|command >> file |	append standard output to a file|
|command < file |	redirect standard input from a file|
|command1 l command2 | pipe the output of command1 to the input of command |
|cat file1 file2 > file0 	| concatenate file1 and file2 to file0
|sort 	| sort data
|who 	| list users currently logged in

###4.1 Wildcards（通配符 *）
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ ls list*
list1  list2
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ ls *list
biglist  slist
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ ls ?list
slist
```
###4.2 Filename conventions (文件名约定)

|Good filenames |	Bad filenames|
|- - - - - - |:    - - - - - - - : |
|project.txt |	project|
|my_big_program.c |	my big program.c|
|fred_dave.doc 	|fred & dave.doc|


###4.3 Getting Help
```
% man wc 

% whatis wc  
  #（gives a one-line description of the command, but omits any information about options etc.）

% apropos keyword
```

|Command 	|Meaning|
|- - - |: - - -:|
|*  	|match any number of characters|
|? 	|match one character
|man command |	read the online manual page for a command|
|whatis command |	brief description of a command |
|apropos keyword| 	match commands with keyword in their man pages|


###5.1 File system security (access rights) (文件系统安全（访问权限）)


```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ ls -l
总用量 28
-rw-rw-r-- 1 kuo kuo 55  9月 20 16:51 a.txt
-rw-rw-r-- 1 kuo kuo  6  9月 20 16:37 a.txt~
-rw-rw-r-- 1 kuo kuo 44  9月 20 18:28 biglist
-rw-rw-r-- 1 kuo kuo  0  9月 20 16:01 c.tat
-rw-rw-r-- 1 kuo kuo 37  9月 20 18:27 list1
-rw-rw-r-- 1 kuo kuo  7  9月 20 18:28 list2
-rw-rw-r-- 1 kuo kuo 88  9月 20 18:34 names.txt
-rw-rw-r-- 1 kuo kuo 44  9月 20 18:32 slist
```
###5.2 Changing access rights(更改访问权限)
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ chmod go-rwx biglist
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ ls -l
总用量 28
-rw-rw-r-- 1 kuo kuo 55  9月 20 16:51 a.txt
-rw-rw-r-- 1 kuo kuo  6  9月 20 16:37 a.txt~
-rw------- 1 kuo kuo 44  9月 20 18:28 biglist
-rw-rw-r-- 1 kuo kuo  0  9月 20 16:01 c.tat
-rw-rw-r-- 1 kuo kuo 37  9月 20 18:27 list1
-rw-rw-r-- 1 kuo kuo  7  9月 20 18:28 list2
-rw-rw-r-- 1 kuo kuo 88  9月 20 18:34 names.txt
-rw-rw-r-- 1 kuo kuo 44  9月 20 18:32 slist
```
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ chmod a+rw biglist
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ ls -l
总用量 28
-rw-rw-r-- 1 kuo kuo 55  9月 20 16:51 a.txt
-rw-rw-r-- 1 kuo kuo  6  9月 20 16:37 a.txt~
-rw-rw-rw- 1 kuo kuo 44  9月 20 18:28 biglist
-rw-rw-r-- 1 kuo kuo  0  9月 20 16:01 c.tat
-rw-rw-r-- 1 kuo kuo 37  9月 20 18:27 list1
-rw-rw-r-- 1 kuo kuo  7  9月 20 18:28 list2
-rw-rw-r-- 1 kuo kuo 88  9月 20 18:34 names.txt
-rw-rw-r-- 1 kuo kuo 44  9月 20 18:32 slist
```

###5.3 Processes and Jobs (流程和工作)
####1.**PS** 显示正在工作的进程
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ ps
  PID TTY          TIME CMD
 2533 pts/1    00:00:00 bash
 7467 pts/1    00:00:00 ps
```
####2.Running background processes(运行后台进程)
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ sleep 10
#前台运行休眠10s
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ sleep 10 &
[1] 7491
#后台运行休眠10s
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ sleep 1000
hh
ffff
#后台运行休眠10s
^Z[1]   已完成               sleep 10
#Crtl+z 停止前台运行
[2]+  已停止               sleep 1000
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ bg
[2]+ sleep 1000 &
#bg 转到后台运行
```
###5.4 Listing suspended and background processes （列出暂停和后台进程）

```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ sleep 1000 
^Z
# ^Z 是暂停前台进程
[1]+  已停止               sleep 1000
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ bg
# bg 后台运行进程
[1]+ sleep 1000 &
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ jobs
# jobs 显示后台进程
[1]+  运行中               sleep 1000 &
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ fg %1
# fg%1 将后台进程1转到前台
sleep 1000
ddd
ddd
^Z
# ^Z 是暂停进程
[1]+  已停止               sleep 1000
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ jobs
[1]+  已停止               sleep 1000
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ bg
[1]+ sleep 1000 &
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ fg %1
sleep 1000
^C
# ^C是杀死进程
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ jobs
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ 
```
###5.5 Killing a process（杀死一个进程）
1.
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ sleep 100 
^Z
[1]+  已停止               sleep 100
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ bg
[1]+ sleep 100 &
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ kill %1
# kill%1 杀死进程1
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ jobs
[1]+  已终止               sleep 100
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ jobs
```
2.
```
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ sleep 100 &
[1] 7579
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ jobs
[1]+  运行中               sleep 100 &
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ ps
  PID TTY          TIME CMD
 2533 pts/1    00:00:00 bash
 7579 pts/1    00:00:00 sleep
 7580 pts/1    00:00:00 ps
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ kill 7579
# kill 7579 使用（PID）号杀死进程
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ jobs
[1]+  已终止               sleep 100
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ jobs
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ ps
  PID TTY          TIME CMD
 2533 pts/1    00:00:00 bash
 7581 pts/1    00:00:00 ps
kuo@kuo-Inspiron-7420:~/unixstuff/backups$ 
```
|Command |	Meaning|
|- - - |:- - -:|
| ls -lag | 	list access rights for all files | 
|chmod [options] file| 	change access rights for named file|
|command &| 	run command in background|
|^C 	|kill the job running in the foreground|
|^Z |	suspend the job running in the foreground|
|bg |	background the suspended job|
|jobs |	list current jobs|
|fg %1 |	foreground job number 1|
|kill %1 |	kill job number 1|
|ps |	list current processes|
|kill 26152 	|kill process number 26152|


### 6.环境变量

% echo $OSTYPE

More examples of environment variables are

    USER (your login name)
    HOME (the path name of your home directory)
    HOST (the name of the computer you are using)
    ARCH (the architecture of the computers processor)
    DISPLAY (the name of the computer screen to display X windows)
    PRINTER (the default printer to send print jobs)
    PATH (the directories the shell should search to find a command)

