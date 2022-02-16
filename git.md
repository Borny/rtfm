# Git

- Create branch
- Delete branch
- Rename branch
- Log
- Changes
- Add
- Commit
- Stash
- Rebase
- Hooks
- Github Pages
- Git clone SSH

---

## Create branch

---

```bash
git checkout -b <branchName>
```

---

## Delete branch

---

### Delete branch locally

```bash
git branch -d <localBranchName>
```

### Delete branch remotely

```bash
git push origin --delete <remoteBranchName>
```

---

## Rename branch

---

If on another branch  
`git branch -m <oldName> <newName>`

Current branch  
`git branch -m <newName>`

---

## Log

---

- log
- reflog

### log

Logs the commits starting with the latest  
`git log`

### reflog

Logs everything that happened (commit, merge, pull, ...)
`git reflog`

## Changes

See changes in the log history:  
`git log -p -[commitNumber]`

---

## Add

---

Will **staged** the modified files

- add .
- add -p
- reset --mixed

`git add .` => will add all files: new and updated ones  
`git add -p` => will list all the changes in the updated files and allow the selection of which changes will be staged for commit. It will not take into account the new files  
`git reset --mixed` => will unstaged the files

---

## Commit

---

- Reset
- Amend

### Reset

Will go back to the desired commit  
`git reset --soft <commitId>` // Will keep the changes  
`git reset --hard <commitId>` // Will discard the changes

Will undo the last commit  
`git reset --[hard || soft] HEAD~1`

### Amend

Will update the last commit and allow for the update of the commit's message
`git commit --amend`

---

## Stash

---

Git stash saves the uncommited work

- push
- clear
- list
- pop
- apply

### push

Will stash the modifications

```bash
git stash push '[nameOfTheStash]'
```

Will stash the **unstaged** modifications

`git stash push --keep-index '[nameOfTheStash]'`

### apply

`git stash apply stash@{[stashNumber]}`

## Rebase

- base
- interactive

### Base

Merge the code from an origin branch into the local branch.

`git rebase master` => will merge all the code from the master branch onto the local branch

e.g:  
`git checkout master` => will checkout on the local master branch
`git pull` => will update the local master branch
`git checkout` => will checkout on the previous branch
`git rebase master` => will merge the code from the local master branch onto the local branch

### Interactive

Can be used to merge commits into each other.

`git rebase -i [commitNumber]`
Use 's' to squash a commit into the previous one.  
Then use the desired commit message

---

## Hooks

---

### Deploy on production server

Connect to the production server using SSH.  
`ssh [name]@[url] -p [portNumber]`  
e.g: `ssh ssh://icsfrai@ssh.cluster028.hosting.ovh.net:22`  
Create a git folder. i.e: [appName.git]  
`cd` into the folder and initialize git: `cd [appName.git] && git init --bare`  
Create a **post-receive** bash file in the hooks/ folder: `touch [appName.git]/hooks/post-receive`  
Make the script executable: `chmod +x post-receive`  
Populate it with the required code. i.e:

```bash
#!/bin/bash
TARGET="/home/clients/3ad4849b308c07bc6da4e58f445b589b/web"
GIT_DIR="/home/clients/3ad4849b308c07bc6da4e58f445b589b/lcdq.git"
BRANCH="master"

while read oldrev newrev ref
do
 # only checking out the master (or whatever branch you would like to deploy)
 if [ "$ref" = "refs/heads/$BRANCH" ];
 then
  echo "Ref $ref received. Deploying ${BRANCH} branch to production..."
  rm -r $TARGET/*
  git --work-tree=$TARGET --git-dir=$GIT_DIR checkout -f $BRANCH -- www/
  cd $TARGET
  cp -r www/* .
  rm -rf www/
 else
  # perform more tasks like migrate and run test, the output of these commands will be shown on the push screen
  echo "Ref $ref received. Doing nothing: only the ${BRANCH} branch may be deployed on this server."
 fi
done
```

On the local repo, add a remote pointing to the server: `git remote add [remoteName]`  
Push to the production repo: `git push [remoteName] [localBranchToPush]`

Example:  
_Deploying on an OVH server_  
`git remote add ovh icsfrai@ssh.cluster028.hosting.ovh.net:icsfrance.git`  
`git push ovh master`

_ADD SSH KEY TO SERVER_  
The first thing you’ll need to do is make sure you’ve run the keygen command to generate the keys:  
`ssh-keygen -t rsa`  
Then use this command to push the key to the remote server, modifying it to match your server name.  
`cat ~/.ssh/id_rsa.pub | ssh user@hostname 'cat >> .ssh/authorized_keys'`

### Angular deploy

Make sure the **/dist** folder is not listed in the .gitignore file
Run `ng build --prod`

In the **post-receive** file:

```bash
#!/bin/bash
TARGET="/srv/www"
GIT_DIR="/srv/[appName].git"
BRANCH="[localBranchToDeploy]"

while read oldrev newrev ref
do
 # only checking out the master (or whatever branch you would like to deploy)
 if [ "$ref" = "refs/heads/$BRANCH" ];
 then
  echo "Ref $ref received. Deploying ${BRANCH} branch to production..."
  git --work-tree=$TARGET --git-dir=$GIT_DIR checkout -f $BRANCH -- dist/[subFolderName]/
  rm -rf $TARGET
  mv $TEMP $TARGET
  cp -r dist/ics/* .
  rm -rf dist/
 else
  # perform more tasks like migrate and run test, the output of these commands will be shown on the push screen
  echo "Ref $ref received. Doing nothing: only the ${BRANCH} branch may be deployed on this server."
 fi
done
```

---

## Github Pages

---

GitHub Pages allows the deployment of static websites and apps.

- VanillaJS
- Ionic - Angular

### VanillaJS

`git subtree push --prefix <nameOfTheSubfolderToDeploy> origin gh-pages`  
i.e: `git subtree push --prefix build origin gh-pages`
**or**
`git push origin `git subtree split --prefix <nameOfTheSubfolderToDeploy> master`:gh-pages --force`

### Ionic / Angular

Add the module to the project:
`ng add angular-cli-ghpages`

Build the app:
`ionic build --prod -- --base-href https://<username>.github.io/<repository>/`

Run the command to deploy the required code (i.e: in the www folder after a build) and create a new gb-pages branches that will only have the code necessary to run the app in production mode:  
`npx angular-cli-ghpages --dir=www`

## Git clone SSH

Git clone using SSH requires the client's key to be stored in Github.

- Go to settings/SSH and GPG keys
- Click the **New SSH key** button
- Enter a name for the key
- Copy the key from the `.ssh/id_rsa.pub` (on WSL2 `~/.ssh/id_rsa.pub`) file on the client's machine
- git clone [URL to clone]
