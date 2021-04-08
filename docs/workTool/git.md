# git 报错 处理

在执行：
```dash
git push -u 或者 git push origin:branchName 报错

error: src refspec x matches more than one.
error: failed to push some refs to ''
```
因为是branch名和tag名相同了。

> 解决办法：删除分支，删除tag

git branch -r -d origin/branchName
git tag -d tagName 

注意：tag远程和本地仓库都要删除的【按需删除，我的场景远程没有这个tag，只有本地有，所以只删除本地就可以了】。删除远程tag后，本地也要执行 git tag 查看的。若没有删除则 
git tag -d tagName 