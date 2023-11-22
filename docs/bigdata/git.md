# git

```bash
# 创建一个文件夹rec，初始化Git并将远程仓库内容拉取到本地
mkdir rec  # 新建一个文件夹
cd rec
git init  # 初始化Git
git pull http://gitlab.****.git  # 从远程仓库拉取内容到本地

# 这一步进行修改，如新增、修改文件等
git init
git add .  # 将新增的文件/文件夹添加到本地Git库，可以使用`.`来添加所有文件，也可以单独添加文件
git remote add origin http://gitlab.****.git
git remote add origin http://gitlab.****.git
git commit -m "My commit"

# 获取远程库与本地同步合并
git pull --rebase origin master # 使用git pull可以从远程获取最新版本并自动合并到本地
git push -u origin master # 使用git pull相当于执行git fetch和git merge。
```
## gitignore
```bash
# 用于提交时默认被忽略的文件
/shelf/
workspace.xml
.idea/misc.xml
.idea/workspace.xml
.idea/vcs.xml
.idea
*.pyc
/log
```
### 如果.gitignore文件是后续添加的，首先需要清除.idea目录的Git缓存：
```
git rm -r -f __pycache__/
git rm -r --cached .idea
```
### 遇到的问题一：
```
! [rejected] master -> master (non-fast-forward)
error: failed to push some refs to 'xxx.git'
```
#### 解决办法：执行 git pull。

### 遇到的问题二：
```
error: The following untracked working tree files would be overwritten by ...
```
#### 解决办法：执行 git clean -d -fx 清除未跟踪的文件：
```
git clean -d -fx
```

### 合并代码的问题
```
//查询当前远程的版本
$ git remote -v
//获取最新代码到本地
$ git fetch origin master  [示例1：获取远端的origin/master分支]
$ git fetch origin dev [示例2：获取远端的origin/dev分支]
//查看版本差异
$ git log -p master..origin/master [示例1：查看本地master与远端origin/master的版本差异]
$ git log -p dev..origin/dev   [示例2：查看本地dev与远端origin/dev的版本差异]
//合并最新代码到本地分支
$ git merge origin/master  [示例1：合并远端分支origin/master到当前分支]
$ git merge origin/dev [示例2：合并远端分支origin/dev到当前分支]
```

### git pull和git fetch
#### 
git  pull(拉取)  即从远程仓库抓取本地没有的修改并【自动合并】到远程分支     git pull origin master

git  fetch(提取) 从远程获取最新版本到本地不会自动合并   git fetch  origin master  
```
//先查看状态，是否有改动
git status
//把更新的代码添加到暂存区
git add [xxx]  //xxx为文件名，
/*
git add . 会把本地所有untrack的文件都加入暂存区，并且会根据.gitignore做过滤。
git add * 会忽略.gitignore把任何文件都加入。
*/
//将暂存区的更新提交到仓库区。
git commit -m "【更新】更新说明"
//先git pull,拉取远程仓库所有分支更新并合并到本地
git pull
//将本地分支的更新全部推送到远程仓库。
git push origin master
//再次查看状态，看是否还有文件没推送
    git status
```

### gitlab的账号与github冲突push不了的问题
```
git config user.name "woo" #改为github的账号
git config user.email "xxx@qq.com"
git remote rm origin #移除
git remote add origin https://github.com/Klring/wwlblog #重新添加
```

### 设置代理
```
git config http.proxy 127.0.0.1:7890
git config https.proxy 127.0.0.1:7890
全局+  --global
取消代理
git config --global --unset https.proxy
git config --global --unset http.proxy
```