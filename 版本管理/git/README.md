# Command

git reset HEAD    重置暂存区到前一个提交状态，工作区不受影响  
git reset --hard HEAD   重置暂存区和工作区到前一个提交的状态  
git reset --hard origin/main  重置到远程分支的最新提交   
git push -f  来覆盖被 reset 的 commit  


git revert 21dcd937fe555f58841b17466a99118deb489212  回退自己的提交 

git revert -m 1 \<commitHash\>    revert合并提交


git checkout .         检出，用暂存区的内容覆盖工作区的内容


git rm —-cached file    从暂存区删除文件，工作区不改变

git status     查看当前仓库状态
git diff       比较暂存区和工作区
git diff master  比较工作区和历史区  
git diff --cached 比较暂存区和历史区  

git log        查看历史提交记录

git checkout -b \<新分支名\> origin/main  创建一个基于远程main的分支


git branch -vv 查看本地分支与远程分支对应关系

git merge branch  将branch合并到当前分支  

git stash 保存暂未commit的代码  
git pop 应用最新的一次stash，并删除记录  
git stash save "xxxx"  保存  
git stash list  列出stash记录  


git relog  记录了 commit 的历史操作。  

git rebase -i   用于合并提交记录  

git push origin HEAD   push当前分支到远程

git push origin a:b  push本地分支a到远程b


对比两个版本库： 
```
	git remote add -f b https://xxxxpath/to/repo_b.git  
	git remote update  
	git diff master remotes/b/master  
	git remote rm b  
```


# Util
工作区   add    暂存区  commit   本地分支（历史区）  push  远程分支

通常来说在 ====== 标记之前的内容来自于接收合并的分支，而在这之后的内容来自于要合并的分支。
```
<<<<<<<<

========

>>>>>>>>
```






# gitignore

Git更新ignore文件直接修改gitignore是不会生效的，需要先去掉已经托管的文件，修改完成之后再重新添加并提交。
第一步：git rm -r --cached .
去掉暂存区已经托管的文件
第二步：修改自己的igonre文件内容
第三步：git add .
git commit -m "clear cached"


# gitconfig

Git的三个重要配置文件分别是/etc/gitconfig，${HOME}/.gitconfig，.git/config。这三个配置文件都是Git运行时所需要读取的，但是它们分别作用于不同的范围。  

- /etc/gitconfig: 系统范围内的配置文件，适用于系统所有的用户； 使用 git config 时， 加 --system 选项，Git将读写这个文件。  
- ${HOME}/.gitconfig: 用户级的配置文件，只适用于当前用户； 使用 git config 时， 加 --global 选项，Git将读写这个文件。  
- .git/config: Git项目级的配置文件，位于当前Git工作目录下，只适用于当前Git项目； 使用 git config 时，不加选项（ --system 和 --global  ），Git将读写这个文件。  



