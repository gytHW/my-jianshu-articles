### 1. 本地已经commit还没提交想撤销
  ```git reset HEAD~1```  这里1表示回退一个commit版本，视实际情况需要写几，此操作不会恢复代码，只会改变git status的状态。

### 2. 取消add
git add文件之后想取消add的文件，几种方法：
- 1）```git reset head + 文件名```,适用于一个个文件恢复
- 2) 干脆继续commit，然后```git reset HEAD~1```

###3.  设置本地分支的远程分支
```git push --set-upstream origin hotfix/20190110```,若远程不存在此分支会自动创建

### 4. 修改分支名
本地修改：在该分支上``` git branch -m 新分支名```，
同步远程：然后删除远程分支 把本地push到远程,参见上面第三条的写法

***
待补充......
