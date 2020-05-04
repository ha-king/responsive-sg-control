## Summary
========
Amazon CloudWatch Events trigger this check when AWS CloudTrail logs EC2 API calls. Specifically, Amazon CloudWatch Events Rules monitoring AWS CloudTrail Logs, trigger based on an API call for "AuthorizeSecurityGroupIngress". The trigger invokes AWS Lambda which runs the python script attached. The python script evaluates the Source IP Address and CIDR range. If the CIDR range is 0.0.0.0/0 for IPv4, or ::/0 for IPv6, the Lambda function will send a violation notification and remove the offending SG rule.

## Deployment
==========

To deploy this security control, upload the security control Lambda ZIP file to a location in Amazon S3. This location must be in the same region you intend to deploy the control.

Launch the provided AWS CloudFormation template using the AWS Console and provide the following information:

  | Parameter            | Description
  | -------------------- | --------------------------------------------------------------------------------------------------
  | S3 Bucket            | The S3 bucket name you uploaded the Lambda ZIP to
  | S3 Key               | The S3 location of the Lambda ZIP. No leading slashes. (ex. Lambda.zip or controls/lambda.zip. )
  | Notification Email   | An email address where you would like violation notifications sent
  | Logging Level        | Control the verbosity of the logs. INFO should only be used for debug
  | Port Value           | TCP port value to evaluate for open ingress


### Deployment Guide
1. <a href="https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=sg-controls&templateURL=https://s3.us-west-1.amazonaws.com/g2g-security-automation/ec2-security-group-open.yml" target="_blank">![Launch](./img/launch-stack.png?raw=true "Launch")</a>
1. Click **Next** to proceed with the next step of the wizard.
1. Specify parameters for the stack.
1. Click **Next** to proceed with the next step of the wizard.
1. Click **Next** to skip the **Options** step of the wizard.
1. Check the **I acknowledge that this template might cause AWS CloudFormation to create IAM resources.** checkbox.
1. Click **Create** to start the creation of the stack.
1. Wait until the stack reaches the state **CREATE_COMPLETE**
