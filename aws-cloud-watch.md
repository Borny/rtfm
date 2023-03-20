# AWS CloudWatch

- [CloudWatch Agent](#cloudwatch-agent)

## CloudWatch Agent

- [Install](#install)
- [Start](#start)
- [Status](#status)

### Install

`sudo yum install amazon-cloudwatch-agent`

### Start

`sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:configuration-file-path`

### Status

`sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status`
