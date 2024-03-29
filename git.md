# Git

- [Create branch](#create-branch)
- [Delete branch](#delete-branch)
- [Remove remote](#remove-remote)
- [Rename branch](#rename-branch)
- [Log](#log)
- [Changes](#changes)
- [Add](#add)
- [Origin](#origin)
- [Commit](#commit)
- [Reset](#reset)
- [Stash](#stash)
- [Rebase](#rebase)
- [Hooks](#hooks)
- [Github Pages](#github-pages)
- [Git clone SSH](#git-clone-ssh)
- [Display branch name](#display-branch-name)

## Create branch

```bash
git checkout -b <branchName>
```

## Delete branch

### Delete branch locally

```bash
git branch -d <localBranchName>
```

### Delete branch remotely

```bash
git push origin --delete <remoteBranchName>
```

### Remove remote references

Will remove the references of the remote branches. e.g. _remotes/origin/name-of-branch_

```bash
git remote prune origin
```

## Remove remote

```bash
git remove remote <nameOfTheRemoteToRemove>
```

## Rename branch

If on another branch  
`git branch -m <oldName> <newName>`

Current branch  
`git branch -m <newName>`

## Log

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

## Add

Will **staged** the modified files

- add .
- add -p
- reset --mixed

`git add .` => will add all files: new and updated ones  
`git add -p` => will list all the changes in the updated files and allow the selection of which changes will be staged for commit. It will not take into account the new files  
`git reset --mixed` => will unstaged the files

## Origin

Setting up the origin of a branch:  

```bash
git push -u origin <branch-name>
# or
git push --set-upstream origin <branch-name>

```

## Commit

- Amend

### Amend

Will update the last commit and allow for the update of the commit's message
`git commit --amend`

## Reset

Will go back to the desired commit:  

`git reset --[hard || soft] HEAD~1` // Goes back to the previous commit  
`git reset --soft <commitId>` // Will keep the changes  
`git reset --hard <commitId>` // Will discard the changes

## Stash

Git stash saves the uncommited work:

- [push](#push)
- [clear](#clear)
- [list](#list)
- [pop](#pop)
- [apply](#apply)

### push

Will stash the modifications:  

```bash
git stash push '[nameOfTheStash]' // will stash all the staged modifications
git stash push '<path/to/file>' // will stash only the given file
```

Will stash the **unstaged** modifications

`git stash push --keep-index '[nameOfTheStash]'`

### clear

Will **delete** all the stashes:  

`git stash clear`

### list

Will display a list of all the stashes:  

`git stash list`

### pop

Will unstash the last entry:

`git stash pop`

### apply

Will unstash the desired stash:  

`git stash apply stash@{<stashNumber>}`

## Rebase

- [base](#base)
- [interactive](#interactive)

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

Merge all commit into one: `git rebase --root -i`
Drop all unwanted commits and rename the commit left

## Hooks

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

_Deploying on infomaniaks with Angular Ionic_  
`ionic build --prod` to build the production version in the **www** folder  
`git push infomanik master` This will push the code directly on the remote branch **infomaniak**

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

## Github Pages

GitHub Pages allows the deployment of static websites and apps.

- [VanillaJS/Vue.js](#vanillajsvuejs)
- Ionic - Angular

### VanillaJS/Vuejs

`git subtree push --prefix <nameOfTheSubfolderToDeploy> origin gh-pages`

```bash
git subtree push --prefix src/pwa origin gh-pages
# or to force push
git push origin git subtree split --prefix <nameOfTheSubfolderToDeploy> main`:gh-pages --force
```

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

## Display branch name

Display the **branch name** in the Bash prompt.

Open the _.bashrc_ file in editing mmode:  

`nano ~/.bashrc`

Add these lines to the file:

```bash
# Show git branch name
force_color_prompt=yes
color_prompt=yes
parse_git_branch() {
 git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
if [ "$color_prompt" = yes ]; then
 PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;31m\]$(parse_git_branch)\[\033[00m\]\$ '
else
 PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w$(parse_git_branch)\$ '
fi
unset color_prompt force_color_prompt
```

Run the file:

`source ~/.bashrc`
