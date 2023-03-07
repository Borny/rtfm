# AWS SAM

AWS Serverless Application Model

Installation walkthrough:  
<https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html>

## Steps

- Download the AWS SAM CLI .zip file.
- unzip the installation files: `unzip aws-sam-cli-linux-x86_64.zip -d sam-installation`
- Install the AWS SAM CLI: `sudo ./sam-installation/install`
- Check the SAM version: `sam --version`

## Create the servers

<https://aws.amazon.com/blogs/storage/enable-password-authentication-for-aws-transfer-family-using-aws-secrets-manager-updated/>

- download the zip file with the SAM template
- unzip it
- run `sam deploy -g` // **-g** is for **--guided**
- update the S3 bucket path in the **samconfig.toml** file
- deploy the servers
- open the Secrets Manager console
- Select **Store a new secret**
- Select **Other type of secret**
- Fill in the following variables: Password (password that will be used with the username), Role (e.g. arn:aws:iam::377136339800:role/sftp-s3-access), HomeDirectory (the S3 bucket path to use)
- enter the secretName as the transfer server id and the username : s-c74efdb045614d309/someUsername (s-c586d11425364568a/matt-sftp)
- testing the setup: `aws transfer test-identity-provider --server-id "s-xxxxxxxxxx" --user-name charlie --user-password password --region us-east-1`
- connect with a client : url: transfer server URL, port : 22, username: username entered while creating the secret, password: password entered while creating the secret variables
