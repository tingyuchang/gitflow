# gitflow
Git flow 練習用專案

## Git flow 簡介
<img src="http://cl.ly/XgJS/gitflow.png" width="400">

+ 主要分支
	+ master: 永遠處在 production-ready 狀態，也就是跟 itunes 上的版本一致
	+ develop: 最新的下次發佈開發狀態
+ 支援性分支
	+ feature: 開發新功能都從 develop 分支出來，完成後 merge 回 develop
	+ release: 要 release 的版本，只修 bugs。從 develop 分支出來，完成後 merge 回 master 和 develop。這裏使用 build version 當作 release name, **ex: release/1.0.0**
	+ hotfix branches: 等不及 release 版本就必須馬上修 master 趕上線的情況。會從 master 分支出來，完成後 merge 回 master 和 develop。一樣使用 build version 當作 hotfix name, **ex: hotfix/1.0.0**

+ 小工具（建議熟悉 git 指令之後再來使用）
	+  [git-flow](https://github.com/nvie/gitflow/)

## 指令

+ push branch 到遠端
	+ `git flow feature publish some_awesome_feature`
	+ `git push origin feature/some_awesome_feature`

+ 追蹤一個遠端的 branch
	+ `git flow feature track some_awesome_feature` 
	+ `git checkout -b feature/some_awesome_feature -t origin/feature/some_awesome_feature`
+ 刪除遠端的 branch
	+ `git push origin :feature/some_awesome_feature`
	+ 注意：git-flow 小工具不會自動刪除已經存在遠端的 feature branch，要手動刪除

## 如何合併 Branch

1. 先確定 develop 是最新的 `git pull origin develop`
2. 對 feature branch 做 `git rebase develop` (這裏最難，要學會 interactive mode，可以拿掉某個 commit 以及合併或修改內容)
3. 在從 develop bracnh 做 `git merge feature/some_awesome_feature -–no-ff`，-–no-ff 的意思是會強制留一個 merge commit log 記錄，這可以讓 commit tree 看清楚發生了 merge 動作。(因為我們剛做了 rebase，而 git 預設的合併模式是 fast-forward，所以如果不加 –no-ff 是不會有 merge commit 的) 這個 merge commit 的另一個額外方便之處是，如果想要 reset/revert 整個 branch 只要 reset/revert 這個 commit 就可以了
4. 如果此 feature branch 有 remote branch，要先砍掉 `git push origin :feature/some_awesome_feature` 再 `git push origin develop` (這是因為 rebase 一個已經 push 出去的 repository，然後又把修改的 history push 出去，會讓 develop branch 產生大問題)


## [Pull Request](https://github.com/blog/785-pull-request-diff-comments)

待補充











***
### 以上內容參考 [ihower blog: Git flow 開發流程](https://ihower.tw/blog/archives/5140)