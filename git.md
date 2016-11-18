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
git add .
gid commit -m "commit message"
```

Merge
```bash
git checkout master
git merge --no-ff feature-x
git tag -a v1.2 "v1.2"
git push --follow-tags
```

[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
