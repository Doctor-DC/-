本地创建git

1、（先进入项目文件夹）通过命令 gitinit 把这个目录变成git可以管理的仓库  
     git init
2、把文件添加到本地版本库中，使用命令git add 文件；添加到暂存区里面去，如果后面接小数点“.”，意为添加文件夹下的所有文件  
`git add .`  

3、用命令 git commit告诉Git，把文件提交到仓库。引号内为提交说明  
`git commit -m 'first commit'`  

4、关联到远程库  
`git remote add origin 你的远程库地址`

或：  
     ` git remote rm origin   //删除origin`

      `git remote add origin git@git.oschina.net:yourname/demo.git   //重新添加origin`



git 回退


可以查看提交历史（整个工程）
     git log  
查看命令历史（本地）
     git reflog  
     
HEAD指向当前版本
     git reset --hard commit_id。
