# git hook

```bash
# For testing using Apache
sudo apt-get update
sudo apt-get install apache2
sudo apt-get install git
sudo chown -R `whoami`:`id -gn` /var/www/html
```

# Deploy to local server with post-commit hook
```bash
$ mkdir ~/proj
$ cd ~/proj
$ git init
$ ls -l .git/hooks

$ nano .git/hooks/post-commit

  #!/bin/bash
  echo Running $BASH_SOURCE
  set | egrep GIT
  echo PWD is $PWD
  unset GIT_INDEX_FILE
  git --work-tree=/var/www/html --git-dir=/home/demo/proj/.git checkout -f
  
$ chmod +x .git/hooks/post-commit
$ git add .
$ git commit -m "this will auto deploy to /var/www/html"

```

## Deploy to production server
```bash
$ mkdir ~/proj
$ cd ~/proj
$ git init --bare  # Bare repository does not have a working directory
$ ls -l            # Files in .git are in the main directory itself

$ nano hooks/post-receive  # Run on git push

  #!/bin/bash
  echo Running $BASH_SOURCE
  set | egrep GIT
  echo PWD is $PWD
  while read oldrev newrev ref
  do
    echo $oldrev $newrev $ref
    if [[ $ref =~ .*/master$ ]];
    then
      echo "Master ref received.  Deploying master branch to production..."
      git --work-tree=/var/www/html --git-dir=/home/demo/proj checkout -f
    else
      echo "Ref $ref successfully received.  Doing nothing."
    fi
  done

$ chmod +x hooks/post-receive

# On local machine
$ git remote add production demo@server:proj
$ git push production master
```
