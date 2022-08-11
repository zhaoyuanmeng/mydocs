# git工作常用命令




# 查看git提交历史

>git log
>
>git log --oneline 查看简洁版的
>
>git log --graph  开启拓扑图选项,查看历史中什么时候出现了分支 合并
>
>git log --author 查看指定用户的提交日志
>
>

# 添加远程仓库

> git remote add origin 仓库地址

# 推送到远程

> git push -u origin master



# 如果想要保存代码,又不想要提交到远程分支的时候

需求是这样的
>在进行多分支开发时，比如你在A分支上开发，但是突然发现B分支上有个bug需要修复，以前往往会把A分支上开发一半的功能本地commit，切换到B分支修复bug，然后再切换回A分支继续开发，这样往往log上会有大量不必要的记录。现在可以使用git stash将你当前未提交到本地（和服务器）的代码推入到Git的栈中，放心切换到B分支修复代码，完事儿后切换回A分支使用git stash apply将以前一半的工作应用回来
>
>
>git 
>
>
>








## 合并多次commit

> git rebase -i  [startpoint]  [endpoint] 其中-i的意思是--interactive，即弹出交互式的界面让用户编辑完成合并操作，[startpoint] [endpoint]则指定了一个编辑区间，如果不指定[endpoint]，则该区间的终点默认是当前分支HEAD所指向的commit(注：该区间指定的是一个前开后闭的区间)。
>
>pick d2cf1f9 fix: 第一次提交  s 47971f6 fix: 第二次提交  s fb28c8d fix: 第三次提交 意思就是把第二次、第三次提交都合并到第一次提交上
>

# 新建本地分支，但不切换
>git branch <branch-name> 
# 查看本地分支
>git branch
# 查看远程分支
>git branch -r
# 查看本地和远程分支
>git branch -a
# 删除本地分支
>git branch -D <branch-name>
