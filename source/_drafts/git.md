~~~shell
git log origin/dev..HEAD
git diff origin/dev..HEAD
git push origin --delete db-test
git cherry-pick f07cc2b207a057d8f7f00264943f10b62dc1357a
git log --oneline --decorate --graph --all
~~~
