git struggles
=======
my trouble shootings while using the git.....:scream:  

### :cloud: Pull with ignoring local change
see also: [here](https://stackoverflow.com/questions/4157189/git-pull-while-ignoring-local-changes)
```
git reset --hard
git pull
```

### :cloud: Update .gitignore when it is already in remote repo
see also: [here](https://stackoverflow.com/questions/7927230/remove-directory-from-remote-repository-after-adding-them-to-gitignore)
```
git rm -r --cached .
git add .
git commit -m 'remove all files in .gitginore"
git push
```

### :cloud: Squash commits after push
see also: [here](https://stackoverflow.com/questions/5667884/how-to-squash-commits-in-git-after-they-have-been-pushed)
```
git rebase -i HEAD~n
change pick to squash (or s)
git push -f
```

### :cloud: pull origin repo with ignoring local changes
see also: [here](https://stackoverflow.com/questions/1125968/how-do-i-force-git-pull-to-overwrite-local-files)
1. Commit of the changes
```
git reset --hard HEAD
git pull
```
2. Fetch the changes, overwrite local changes
```
git fetch origin master
git merge -s recursive -X theirs origin/master 
```
* theirs: their change를 선택 (다른 option: ours)
* fetch: local repository에서 remote repository의 내용을 업데이트

### :cloud: rebase시 주의할 점
* rebase시 충돌이 일어나면 충돌이 일어난 파일을 수정해서 rebase를 다시 해야함
* 그렇지 않으면 detatch된 상태에 머물러 있다
```
git rebase --continue
```

### :cloud: 오픈소스에 PR시 permission denied 문제
* error log
```
remote: Permission to microsoft/otdd.git denied to mysunk.
fatal: unable to access 'https://github.com/microsoft/otdd/': The requested URL returned error: 403
```
 
