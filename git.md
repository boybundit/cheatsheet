# Git

Clone
```bash
git clone https://github.com/user/repo.git
```

Pull
```bash
git pull
```

Branch
```bash
git branch

# Create local branch
git checkout -b feature-x
# Create remote branch
git push --set-upstream origin feature-x

# Delete remote branch
git push origin --delete feature-x
# Delete local branch
git branch -d feature-x
```

Commit
```bash
git status
git add . // Undo by git reset (no dot)
gid commit -m "commit message"
```

Merge
```bash
git checkout master
git merge --no-ff feature-x
git tag -a v1.2 "v1.2"
git push --follow-tags
```

Squash
https://ariejan.net/2011/07/05/git-squash-your-latests-commits-into-one/
```bash
git rebase -i HEAD~3
// Editor will be opened with the last 3 commits
> pick f392171 Added new feature X
> squash ba9dd9a Added new elements to page design
> squash df71a27 Updated CSS for new elements
git push -f origin master // -f = force rewrite
```

Update forked branch
http://stackoverflow.com/questions/7244321/how-do-i-update-a-github-forked-repository
```bash
git remote add upstream https://github.com/whoever/whatever.git
git fetch upstream
git checkout master
git rebase upstream/master
git push -f origin master
```

[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

Check file changes
```bash
git log -p [--follow] [-1] <path>
git log -p -1 <commit>
gitk <file>
```
