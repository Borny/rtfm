# AWS DOC

- [EC2 Node.js](#ec2-nodejs)
- [Database](#database)

## Database

Create the database  
Make it publicly accessible  
Modify the security group to allow inbound traffic(either a specific IP or all 0.0.0.0)

## EC2 Node.js

- Create instance
- Create a .pem file that will be stored where the app is on local
- Allow inbound traffic to http, https, ssh and the port that the app will listen to (e.g: 7000) from anywhere or a specific IP
- Connect to the instance via SSH (e.g: ssh -i "feijao-api.pem" ubuntu@ec2-3-76-207-240.eu-central-1.compute.amazonaws.com)
- Copy the project to the EC2 instance
- Generate an SSL certificate to serve the app over HTTPS
