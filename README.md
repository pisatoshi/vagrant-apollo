# vagrant-apollo
==============

Apache Apollo MQTT broker

## Manual Setting on AWS
Create a AWS security group "apollo".

Allow inbound access to the following ports.

- TCP 22
- TCP 61613
- TCP 61614
- TCP 61623
- TCP 61624
- TCP 61680
- TCP 61681

## Setup environment variables for AWS.
Copy "awsrc.example" to ".awsrc", and edit environment variables.

- JAVA_HOME
- EC2_HOME
  ec2-api-tools install directory
- EC2_URL
  api endpoint url
- AWS_ACCESS_KEY
- AWS_SECRET_KEY
- AWS_KEYPAIR_NAME
- AWS_PRIVATE_KEY_PATH

## Create instance (VurtualBox)
```Bash
$ vagrant up apollo
```

## Create instance (AWS)
```Bash
$ . awsrc
$ vagrant up apollo_aws --provider=aws
```
