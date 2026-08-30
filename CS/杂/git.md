`git init` 初始化一个空的Git仓库
`git add` 将更改的文件添加到 Git 的暂存区
`git commit` 创建一个新的提交，该提交将会抵消先前的提交效果
`git commit -m "Initial commit"`
`git merge` 合并不同分支的代码
`git log` 查看代码的历史提交记录
`git push` 将代码推送到远程仓库
`git push -u origin <branch-name>` 推送当前分支到远程仓库，并与远程分支关联
`git push origin <branch-name>` 推送当前分支到远程仓库，并与远程分支合并
`git push -f origin <branch-name>` 强制推送当前分支到远程仓库
`git push --delete origin <branch-name>` 删除远程分支
`git pull` 从远程仓库中获取代码
`git status` 查看当前工作区和暂存区的状态
`git remote` 管理远程仓库
`git reset HEAD~1` 使HEAD指向上一个提交，但不会删除您最新的更改
`git reset -- hard HEAD~1` 将永久删除您最新的更改
`git checkout <branch_name>` 切换到新创建的分支
`git checkout -b <branch_name>` 创建并立即切换到该分支
`git checkout -b <new-branch-name> origin/<remote-branch-name>` 在本地创建基于远程分支的新分支
`git checkout --track origin/<remote-branch-name>` 拉取远程分支并自动与本地分支关联
`git merge <branch_name>` 合并分支
`git fetch --all` 拉取所有远程分支并更新本地分支
`git fetch origin <branch-name>` 拉取一个特定的远程分支到本地

`git remote add origin https://` 添加远程仓库
`git add .` 将所有文件添加到暂存区
`git commit -m 'first upload'` 添加说明
`git push -u origin main` 上传

Workspace：工作区
指您电脑文件系统上用于修改文件的目录。在这里，您可以创建、编辑和删除文件。
Index / Stage：暂存区
中间状态，它充当了您提交更改的缓冲区。您必须明确地将文件添加到暂存区，然后才能将其提交到版本库中。
Repository：仓库区（或本地仓库）
版本库包含Git存储库的所有历史记录和元数据。它是Git存储库的核心组成部分，是由Git自动维护的。
Remote：远程仓库