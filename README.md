# FHIR Works on AWS
## Installation Process
The installation process will take you through the Linux/MacOS process.<br />
**IMPORTANT NOTE: The new M1 Macs or any ARM chip computers will not work with this installation.**
### Initial Setup:
 1. Clone the FWoA repository on your local machine. 
```
git clone https://github.com/awslabs/fhir-works-on-aws-deployment.git
```
2.  Install AWS CLI
```
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```
3. Install serverless
```
sudo yarn global add serverless
```
### Configuring your AWS Credentials
1. Create an IAM User with admin access. `Access Management -> Users
