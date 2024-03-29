# Git使用笔记

- git把本次修改的内容添加到上次提交中

```bash
git commit --amend #会通过 core.editor 指定的编辑器进行编辑
git commit --amend --no-edit #不会进入编辑器，直接进行提交
//可以配置vim位默认编辑器
git config --global core.editor /usr/bin/vim
```




- git撤销文件的修改

```bash
#1.在工作区修改，但并未提交到暂存区（即并没有add）。
#对于单个文件的撤销修改
git checkout -- 文件名
#撤销所有文件的修改
git checkout .
```



- git切换到某个commit ID

```bash
#查看所有提交
git log
#切换到某个commit
git checkout 22dfbf1f907764c5ae70381b8191104f9af21d8c #commit号
```




- git提交代码时出现Everything up-to-date的解决办法

```shell
1. 创建一个新的分支：git branch newBranch
2. 查看分支是否创建成功：git branch
3  切换到此分支：git checkout newBranch
4. 把代码直接提到newBranch分支上，如果出现以下提示：
5.切换（如：git checkout master）到原本想要提交代码的分支，把newBranch合并到master分支上：git merge newBranch
6.合并之后如果发现有冲突，可git diff查看有冲突的文件      ------ps：基本不会有冲突
7.最后就可以重新提交一下代码了，直接git push就可
原文链接：https://blog.csdn.net/zhuhongyang_/article/details/118521264
```




 - git 创建`gitignore`文件
```shell
    <!-- 直接在项目目录下 touch .gitignore -->
    git rm -r --cached .    #新增的忽略文件没有生效，是因为git是有缓存的，而之前的文件在缓存中，并不会清除掉，还会继续提交，所以更新.gitignore文件，要清除缓存文件
    git add .
    git commit -m 'update .gitignore'
    git push origin master
```



- `git push`提示`Username for 'https://github.com'` 解决办法

```shell
$ git add .
$ git commit -m 'first commit'
[master (root-commit) 3f1b963] first commit
1 files changed, 59 insertions(+)
create mode 100644 readme.md
$ git remote add origin https://github.com/XXXX/project.git
$ git push -u origin master
Username for 'https://github.com':
```

这里的账号是没问题，可是`password`并不是账号密码的这个密码，而是`github`里面 `develop setting` 里面的`acess key`;

解决办法：修改 .git/config 文件

```http
将 https://github.com/XXXX/project.git 改为
git@github.com:XXXX/project.git
```



- `git stash` 暂存缓冲区

```shell
 - `git stash`: 保存工作现场，即可切换到其他分之开发
 - `git stash pop`: 弹出暂存的更改，继续开发，恢复的同时把stash内容也删了
 - `git stash list`: 查看被statsh的内容
 - `git stash drop`: 删除stash的内容   
```



- `git HEAD detached from origin`  游离分支问题

  这个问题是因为分支选错了，所以说后续的提交都提交到了一个匿名分支之上，整个状态是游离了的，下面说一下解决问题的步骤

  1.查看在游离状态下提交的最新commit号

  ```
  git branch -v
  ```

  2.创建一个临时的分支，创建完成之后切换到该分支上查看一下

  ```
  git branch temp 最新的commit号
  ```

  3.切换到要合并的分支智商

  ```
  git checkout 要合并临时分支的分支名称
  ```

  4.合并分支

  ```
  git merge temp
  ```

  5.删除临时分支

  ```
  git branch -d temp
  ```

  6.推送合并后的分支到远程仓库

  ```
  git push origin 分支名称:远程分支名称
  ```



- git push origin 时报错

  ![image-20230514170225074](image/image-20230514170225074.png)

  解决办法： 产生这个错误的时候我开着vpn，切换一个网络就行，比如把全局代理关闭
