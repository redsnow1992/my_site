~~~shell
git log origin/dev..HEAD
git diff origin/dev..HEAD
git push origin --delete db-test
git cherry-pick f07cc2b207a057d8f7f00264943f10b62dc1357a
git log --oneline --decorate --graph --all
~~~

# [delete branch locally and remotely](http://stackoverflow.com/questions/2003505/delete-a-git-branch-both-locally-and-remotely)
~~~shell
git checkout other_than_branch_to_be_deleted
git branch -D branch_to_be_deleted
git push origin --delete branch_to_be_deleted
~~~

~~~shell
tag current commit
git tag new_tag
git push --tags origin master

git add forgotten_file.rb
git commit --amend

 git commit --date=09.04.2016T14:36:00 -m 'add date'

git reset HEAD to_commit_second.rb

git reset --soft HEAD^1

git pull origin remote : local
git push origin local : remote

git remote add origin https://github.com/githug/githug

git rebase origin/master
~~~

~~~
git diff
--- a/app.rb
+++ b/app.rb
@@ -23,7 +23,7显示文件从第23行开始，一共7行
~~~

~~~
git blame config.rb  #查看某个文件的修改人
git branch new_branch # 创建一个分支
git checkout -b my_branch == git branch my_branch git checkout my_branch
git checkout v1.2 #(tag)
git checkout tags/v1.2
git branch test_branch HEAD^1
git push origin test_branch:test_branch
git fetch origin
git pull == git fetch git merge
git rebase master feature 表示将 feature 上的修改在 master 上重新应用一遍
git log --graph --all
~~~
