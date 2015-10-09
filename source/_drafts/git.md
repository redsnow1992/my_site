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
