## Refs
[中文版教程](https://git-scm.com/book/zh/v2)

## Essential Concepts of Git
1. What is git?
> Git is a distributed system that stores all the versions of source code, along with each change.

2. What is the difference between Git and GitHub?
> Git is a revision control system, a tool to manage your source code history.
> GitHub is a hosting service for Git repositories.
> So they are not the same thing: Git the tool, GitHub the service for projects that based on Git.     
![](https://ws2.sinaimg.cn/large/006tNc79gy1flnj06kc2ij30ue0qmab3.jpg)
![](https://ws4.sinaimg.cn/large/006tNc79gy1flnizvq023j30qs0p0765.jpg)
;


3. Structure of Git system（local repository）  
- .git directory:
  - commit
  - branch
  - HEAD and heads(this is a configuration file directory)  
- working directory(actual code files and directories.)  
- index( a schema for interaction)


## Essential Concepts of Github

1. Two working mode
- fork & pull mode
- Shared Repository mode

2. Repositories
- A basic unit of GitHub, most commonly a single project, which is actually just a directory that created in server
- Users store anything including image, text file, or folders that project needs.

3. Issue
- An Issue is a note on a repository about something that needs attention. It could be a bug, a feature request, a question or lots of other things.

4. Branch
- Branches could be created from a master. A branch is used for expressing different comment, new idea, another feature.

5. Commits
- Any changes made to any file in a repository is called a commit, no matter add, delete, change a file, code, or add comments. A history of tracking changes of master. Often commits happen first at branches then are merged into master.

6. Pull Requests
- The heart of collaboration on GitHub.
- When you make a pull request, you’re proposing your changes and requesting that someone pull in your contribution - aka merge them into their branch. you could even just raise a comment, or even a screen shot, anything you need to discuss with anyone, using @ function to mention someone specific you want to collaborate with. 

7. Deploy
- You could deploy your branch any time in production.

8. Merged
- The maintainer of the collaborative repository merge pull request once your pull request has been discussed 


## Common used Commands
1. Initialization & configuration 
- 在新的系统上，都需要先配置一次git的工作环境以及各种变量
- git config: 此命令专门用于环境和变量的配置
  - 首先，要知道配置的环境有三层，分别是system/user/project，scope依次递减，优先级依次递增。system级别的对应的是/etc/gitconfig, user级别对应的是~/.gitconfig，project级别的只作用在某一个项目下。
    
  - 三者对应的命令开头分别是
    ···
      git config —system
      git config —global
      git config   
    ···  
    - personal information 
    - git config —global user.name “"
          - git config —global user.email “"     
     - git config  — list : 看当前所有的配置好的变量
     - git help <verb> or git <help> —verb or man git-<verb>

- Remote repos setting up 
- git remote : check current added remote repositories 
     - origin is the default name indicating original repos you cloned
- git clone git:// or http:// 
     > 默认情况下 git clone 命令本质上就是自动创建了本地的 master 分支用于跟踪远程仓库中的 master 分支（假设远程仓库确实有 master 分支）。所以一般我们运行 git pull，目的都是要从原始克隆的远端仓库中抓取数据后，合并到工作目录中的当前分支。

- git remote add origin(这个名字可以随便改) git@server-name:path/repo-name.git （SSH）
- git remote add origin(这个名字可以随便改) git:// erver-name:path/repo-name.git （A/P）
- git push -u origin master/ git push origin master
- git fetch
* the only differences between git pull and fetch is that fetch unnecessarilly merge remote and local files.


- mkdir … cd … pwd ...
- git init 

save changed version of text files— attention! don’t use MS word
- git add file: put modified files into a cache list
- git commit -m “put my comments here”: commit all modified stored in cache
- git status
- git diff file
- git diff —staged : to see any differences between all modified files stored in cache and ones last commit 

trace history log of moves
git log —pretty=oneline
git reset —hard HEAD^ or HEAD^^  or HEAD~100 or version ID
cat filename.txt #see content of this file, linux command
git reflog #see commands history 

roll back to previous versions
- git checkout -- filename.txt #when you didn't stage the file
- git reset -- hard HEAD filename.txt





