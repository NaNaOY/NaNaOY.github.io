### Git远程仓库

　　要添加一个仓库，首先必须知道仓库的地址，然后使用git remote add 命令添加远程仓库，也可使用git clone命令克隆。（Git支持多种协议，包括http、https，但通过ssh支持的原生git协议速度最佳。） 


　　要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git，关联后，使用命令git push -u origin master第一次推送master分支的所有内容，此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改

1. 1. `# git remote add [remote] [url]#添加(关联)远程库`
   2. `# git remote set-url [remote] [url] #修改远程仓库`
   3. `# git clone [url] #克隆远程仓库项目`
   4. `# git remote #查看指定远程仓库命名简写`
   5. `# git remote –v #查看远程仓库详细信息以及名称对应URL`
   6. `# git push -u remote master #第一次推送master分支的所有内容`
   7. `# git fetch remote [branch/tag] #下载远程仓库的所有变动`
   8. `# git pull remote [branch/tag] #拉取主分支最新版本(可以拉取其他分支)`
   9. `# git push remote [branch/tag] --force #强行推送当前分支至远程分支,及时冲突`
   10. `# git push remote [branch/tag] --all #推送所有分支到远程仓库`
   11. `# git remote rename [oldname] [newname] #修改远程仓库名称`
   12. `# git remote remove [name] #删除远程仓库名称以及URL地址`



再打开本地Git Bash，配置全局的 user.name 和 user.email：

git config --global user.name "root"
git config --global user.email "yuanjie@397.com"

首先cd到你需要导入的项目目录下，再执行导入命令：

git init
git remote add origin git@10.3.1.12:root/test.git
git add .
git commit -m "测试-test"
git push -u origin master
