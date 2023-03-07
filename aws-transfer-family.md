# AWS Transfer Family

- [About](#about)

## About



## VPC

Virtual Private CLoud.
Will host the server.
Will act as the endpoint for the server.

## SFTP Server

Will transfer the data from the client to a dedicated S3 bucket.
Will be hosted by a VPC.
Has an Internet-facing endpoint.

## Subnets

A range of IP addresses in your VPC

## Elastic IP

Elastic IP address is preserved after you stop and start your instance in a virtual private cloud (VPC)

## Route Tables



## Internet Gateway



## Security Group



## ACL



**SFTP server**:

Endpoint Configuration:

- Endpoint type => VPC hosted
- Access => Internet Facing

- Create a VPC
- Create Gateway <https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html>
- Attach Gateway to VPC
- Edit Route table => <https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html> by adding a Route Destination
- Allocate an Elastic IP address (**3.69.223.53**)
- Associate the Elastic IP address
