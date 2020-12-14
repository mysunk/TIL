git struggles
=======
my trouble shootings while using the git.....:scream:  

### Pull with ignoring local change
see also: [here](https://stackoverflow.com/questions/4157189/git-pull-while-ignoring-local-changes)
```
git reset --hard
git pull
```

### Update .gitignore when it is already in remote repo
see also: [here](https://stackoverflow.com/questions/7927230/remove-directory-from-remote-repository-after-adding-them-to-gitignore)
```
git rm -r --cached .
git add .
git commit -m 'remove all files in .gitginore"
git push
```

### Squash commits after push
see also: [here](https://stackoverflow.com/questions/5667884/how-to-squash-commits-in-git-after-they-have-been-pushed)
```
git rebase -i HEAD~n
change pick to squash (or s)
git push -f
```