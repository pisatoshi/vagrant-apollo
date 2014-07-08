# vagrant-apollo

Apache Apollo MQTT broker

## Manual Setting on AWS
Create a AWS security group.  
default using "apollo".

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
    Java runtime home
- EC2_HOME  
    ec2-api-tools install directory
- EC2_URL  
    EC2 API endpoint url
- AWS_ACCESS_KEY_ID  
    Access key id
- AWS_SECRET_ACCESS_KEY  
    Seret access key
- AWS_KEYPAIR_NAME  
    Key pair name
- AWS_PRIVATE_KEY_PATH  
    Private key(.pem) file location

## Create instance (VurtualBox)
```Bash
$ vagrant up apollo
```

## Create instance (AWS)
```Bash
$ . awsrc
$ vagrant up apollo_aws --provider=aws
```
