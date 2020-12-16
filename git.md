# Git

- Create branch
- Delete branch
- Log
- Rollback to previous commit
- Stash
- Hooks
- Github Pages

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
## Log
---
Log the commits starting with the latest   
`git log` 

---
## Rollback to previous commit
---

### soft 
`git checkout --soft <commitId>`

### hard
`git checkout --hard <commitId>`

---
## Stash
---
Git stash saves the uncommited work.

---

## Hooks

---

### Deploy on production server

Connect to the production server using SSH.  
`ssh [name]@[url] -p [portNumber]`  
i.e: `ssh icsfrdjg@ssh.cluster056.hosting.ovh.net -p 22`  
Create a git folder. i.e: [appName.git]  
cd into the folder and initialize git: `cd [appName.git] && git init --bare`  
Create a **post-receive** bash file in the hooks/ folder: `touch [appName.git]/hooks/post-receive`  
Make the script executable: `chmod +x post-receive`  
Populate it with the required code. i.e:

```bash
# The production folder
TARGET="/srv/www/"

# A temporary directory for deployment
TEMP="/srv/tmp/"

# The Git repo
REPO="/srv/[appName].git"

# Deploy the content to the temporary directory
mkdir -p $TEMP
git --work-tree=$TEMP --git-dir=$REPO checkout -f

# cd $TEMP
# do stuff like npm install...

# Replace the production directory with the temporary directory
# cd /
rm -rf $TARGET
mv $TEMP $TARGET
```

On the local repo, add a remote pointing to the server: `git remote add [remoteName]`  
Push to the production repo: `git push [remoteName] [localBranchToPush]`

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
- Ionic - Angular
  
GitHub Pages allows the deployment of static websites and apps.

### Ionic / Angular

Add the module to the project:  
`ng add angular-cli-ghpages`  

Build the app:  
`ionic build --prod -- --base-href https://<username>.github.io/<repository>/`  

Run the command to deploy the required code (i.e: in the www folder after a build) and create a new gb-pages branches that will only have the code necessary to run the app in production mode:  
`npx angular-cli-ghpages --dir=www`  