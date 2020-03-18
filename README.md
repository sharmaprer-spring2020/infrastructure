# Infrastructure As code with AWS Cloud Formation

### Prerequisites ###
* AWS CLI installation and setup

### AWS CLI Installation ###
* Please copy and paste below commands one by one for installation  
```
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```  
* [Click here](https://docs.aws.amazon.com/cli/latest/userguide/install-macos.html) for additional ways intsall AWS CLI
* Once done with above step, try below command to confirm installation  
```
aws --version
```
### AWS CLI Profile Setup ###
- Please run below command to setup _`dev`_ and _`prod`_ profiles in aws cli with default region _`us-east-1`_ . For more information refer [creating multiple profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration-multi-profiles)  
```
aws configure --profile dev
aws configure --profile prod
```  
To get project on local do following:
1. **`git clone git@github.com:prernsha/infrastructure.git`**   
2. **`cd`** into **`infrastructure`**

### CloudFormation Instructions ###
We will create a stack as specified in the infrastructure template file _`networking.json`_. Below values are provided as command line arguments at run time:
- __`--region`__ : this is aws region for eg: us-east-1, us-east-2, or us-west-2 etc
- __`--stack-name`__ : your stack name has to be unique in each region
- __`ParameterValue`__ : Provide any values for **vpcCidr**, **subnetCidr**, and **vpcName**.

#### Create Stack from template #### 
1. Execute commands one by one on terminal following below steps
   - Execute below command to [create-stack](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html)
   ```
   aws cloudformation create-stack  \                                                          
    --stack-name $stackName \
    --region $region \
    --parameters ParameterKey=vpcCidr,ParameterValue=0.0.0.0/0 \
    ParameterKey=subnetCidr,ParameterValue="0.0.0.0/0\,0.0.0.0/0\,0.0.0.0/0" \
    ParameterKey=vpcName,ParameterValue=[your-VPCName]] \
    ParameterKey=EC2VolSize,ParameterValue=20 \
    ParameterKey=RDSPublicAccessibility,ParameterValue=false \
    ParameterKey=DbStorage,ParameterValue=50 \
    ParameterKey=EC2AMI,ParameterValue=[your-ami] \
    ParameterKey=awsKeyname,ParameterValue=[your-key]] \
    ParameterKey=DbPass,ParameterValue=[your-dbPass] \
    ParameterValue=DbUsername,ParameterValue=[your-DbUsername]] \
    ParameterKey=CodeDeployBucketName,ParameterValue=[your-CodeDeploy-bucketName] \
    --template-body file://application.json --capabilities CAPABILITY_NAMED_IAM 
   ```
   - Execute below command to describe the stack
   ```
   aws cloudformation describe-stack --stack-name YOUR-STACK-NAME
   ```
   - Execute below command to delete the stack
   ```
   aws cloudformation delete-stack --stack-name YOUR-STACK-NAME
   ```

2. Execute **`stackOp.sh`** providing the _profile, operation type, stack name, and aws region._

   ```
    sh stackOp.sh  
   ```
#### Conflict ####
Error could not find login credentials. This is because we have no deafault profile set. Two ways we can avoid this error:

1. In create-stack command add a flag for --profile profile-name
2. in your favorite terminal execute below command replacing profilename with your preferred profile name
 ```
    export AWS_DEFAULT_PROFILE=profilename 
 ```