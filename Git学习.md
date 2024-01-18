# Git学习

## 1、版本控制

> 什么是版本控制？

版本控制是一种在开发过程中用于管理我们对文件、目录或工程的修改历史，方便查看历史记录，备份以便恢复历史版本的软件工程技术。

常见的版本控制器：

- Git
- SVN
- CVS

> 版本控制分类

1、**本地控制版本**

本地记录每次文件的更新

<img src="https://gitee.com/lyydsheep/pic/raw/master/202401141207742.png" alt="image-20240114120705647" style="zoom:50%;" />

2、**集中版本控制**

所有的历史版本都记录在服务器上，每次开发都需要从服务器下载最新代码至本地，本地更新完后再同步到服务器上

<img src="https://gitee.com/lyydsheep/pic/raw/master/202401141207600.png" alt="image-20240114120743499" style="zoom:50%;" />

3、**分布式版本控制**

每个人都拥有全部代码，所有版本信息仓库全部同步到本地的每个用户，可在本地查看所有历史版本。用户可以离线提交，联网后在push到服务器或其他用户即可。

<img src="https://gitee.com/lyydsheep/pic/raw/master/202401141208187.png" alt="image-20240114120855108" style="zoom:60%;" />

Git是分布式版本控制系统，没有中央服务器，每个人的电脑就是一个完整的版本库，工作的时候不需要联网，因为版本都在自己的电脑上。Git可以直接看到更新了哪些代码和文件。

## 2、Git安装以及Linux命令

Git Bash:Unix和Linux风格的命令行，使用最多

Git CMD:Windows风格的命令行

Git GUI:图形界面Git

> 基本的Linux命令

1. cd：改变目录
2. cd ..：退回上级
3. pwd：显示当前目录路径
4. ls：显示当前目录下的所有文件
5. touch：创建文件
6. rm：删除文件
7. mkdir：创建文件夹
8. rm -r：删除一个文件夹
9. mv src dst：移动src文件至dst文件夹下
10. reset：重视初始化终端/清屏
11. clear：清屏
12. history：查看历史命令
13. help：帮助
14. exit：退出
15. #：表示注释
16. cp -r /home/user05/lab07/* /home/user05/lab09
17. cp flags.c /home/user05/lab09/flags_revised.c

> Git相关的配置(本质是个配置文件)

1. 设置用户名

   ```go
   git config --global user.name "lyydsheep"
   ```

2. 设置邮箱

   ```go
   git config --global user.email "2230561977@qq.com"
   ```

   

<img src="https://gitee.com/lyydsheep/pic/raw/master/202401141504246.png" alt="image-20240114150354162"  />

![image-20240114150511059](https://gitee.com/lyydsheep/pic/raw/master/202401141505087.png)

## 3、Git基本理论

> 工作区域

Git本地有三个工作区域：Workink Directory、Stage、Repository以及远程的Remote Directory。

![image-20240114151821391](https://gitee.com/lyydsheep/pic/raw/master/202401141518446.png)

## 4、Git基本操作

1. 项目创建及克隆

   ```go
   git init //在当前目录下创建一个项目, .git是隐藏文件
   git clone [url]//克隆一个项目
   git remote add origin https://gitee.com/charken/elm-ssm.git //关联仓库
   ```

2. 文件操作

   ```go
   git add . //将所有文件添加至Stage
   git commit -m "message"//将Stage中所有文件添加至Repository
   git status
   git status [filename]//获取指定文件状态
   git push origin master
   ```

3. 忽略文件

   ```go
   //提交工程项目时建立并配置一个.gittignore文件，忽略不必要commit/push的文件
   ```

4. 配置SSH公钥及创建Remote Repository

   ```go
   cd ~/.ssh//可查看是否创建过公钥
   ssh-keygen -t rsa//cd到C盘user目录后,调用命令，生成公钥
   ssh -T git@github.com//测试是否配置成功
   ssh-keygen -t rsa -C "xxx"  //你的github邮箱帐号
   ```

   创建本地仓库（init）--->关联远程仓库--->下拉合并--->修改项目代码--->（add, commit,push)提交
   
   后续只需从“修改项目”开始即可
   
   ![image-20240114172449220](https://gitee.com/lyydsheep/pic/raw/master/202401141724272.png)