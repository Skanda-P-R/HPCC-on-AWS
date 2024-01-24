# Installing HPCC Systems on AWS Platform
## Steps to be followed
1.  Create an AWS account.
2.	Create the IAM user.
3.	Configure the AWS CLI, EKSCTL, Helm and Kubectl.
4.  Create Virtual Private Cloud (VPC).
5.  Create Subnets within the VPC.
6.	Create the EFS service.
7.	Deploy an EKS cluster in the VPC.
8.	Create Secrity group in VPC
9.	Deploy the HPCC Systems cluster on EKS.
## Create AWS Account
Head over to https://aws.amazon.com/console/ to create an AWS Account.
## Create the IAM user
Identity and Access Management (IAM), is a service provided by AWS, which allows us to manage access to certain AWS services and resources securely. This should be added to the user who manages the cluster. It is given to the user in the AWS Console page.<br>
In the Console page, select your account and open “Security Credentials”, which will take you to Identity and Access Management Page. Here you enable the Multi Factor Authentication by using an application like Google Authenticator.<br><br>
![image](https://github.com/Skanda-P-R/HPCC-on-AWS/assets/115518870/1f97b51d-cd98-4d20-a64e-5672700f9e27)
<br><br>
![image](https://github.com/Skanda-P-R/HPCC-on-AWS/assets/115518870/fc8ffa25-87a7-4bb0-aeca-49916eb873f6)
<br><br><br>
Next create an IAM user and make a note of the ACCESS_KEY and the SECRET_KEY.
<br><br><br>
![image](https://github.com/Skanda-P-R/HPCC-on-AWS/assets/115518870/25098f0b-7bf0-4675-b2e8-04e3452da9b5)
<br><br>
Here, we can see an IAM user has been created. In the left panel, select Policies, and click on Create Policies. In the JSON section, add this policy and name it anything, for the instruction purpose, its named as AmazonEKSAdmin:
```
{
    "Version": "2012-10-17",
    "Statement": [
      {
         "Effect": "Allow",
         "Action": [
           "eks:*"
         ],
         "Resource": "*"
      },
      {
         "Effect": "Allow",
         "Action": "iam:PassRole",
         "Resource": "*",
         "Condition": {
           "StringEquals": {
             "iam:PassedToService": "eks.amazonaws.com"
             }
         }
      }
  ]
}
```
Also, search for all the below policies and add them:
```
AmazonElasticFileSystemFullAccess
AWSCloudFormationFullAccess
AmazonEC2FullAccess
IAMFullAccess
AmazonEKSClusterPolicy
AmazonEKSWorkerNodePolicy
AmazonS3FullAccess
CloudFrontFullAccess
AmazonVPCFullAccess
AmazonEKSServicePolicy
```
![image](https://github.com/Skanda-P-R/HPCC-on-AWS/assets/115518870/238b651f-d713-41af-b4fa-c7e4058cb713)<br><br>
Add all these policies to the IAM user created.
## Setting up the prerequisites
These tools are prerequisites to install the HPCC System on AWS, hence install them by clicking on the provided links and make sure to add all the files related to the tools to the Environment PATH variables.
* AWS CLI v2:  https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html
* Kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/
* EKSCTL: https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html
* Helm: https://helm.sh/docs/intro/install/<br>
![image](https://github.com/Skanda-P-R/HPCC-on-AWS/assets/115518870/7ba24cef-c78d-4d95-902c-4c8f845ee346)
<br><br>
Next open your command prompt and paste the below command:
```
aws configure
```
Provide the ACCESS KEY and the SECRET ACCESS KEY.
## Deploy a Virtual Private Cloud (VPC) and Subnets
VPC is a logically isolated section where we can launch AWS resources.<br>
Subnets are subdivisions of a larger network like VPC. Subnetting allows us to divide these larger networks into smaller and manageable segments. A minimum of two subnets must be created within the VPC.<br>
